# Authentication

To authenticate our app we will use the same basic methodology as we did with Express:

1. When a user registers we will hash their password with BCrypt before storing it in the database
1. When a user logs in we will validate the password they supply against the hashed password in the database
1. If the password is valid we will send a _JSON web token_ (JWT)
1. The JWT can then be used to access certain routes that would otherwise be unavailable

## Setting up BCrypt

Install with pipenv:

```sh
pipenv install flask-bcrypt
```

And add to the `app.py` file:

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow
from flask_bcrypt import Bcrypt

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgres://localhost:5432/flask-bcrypt-example'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy()
ma = Marshmallow(app)
bcrypt = Bcrypt(app)

from config import routes
```

## Creating a user model

Let's start with a basic user model:

```py
# models/user.py
class User(db.Model, BaseModel):

    __tablename__ = 'users'

    username = db.Column(db.String(20), nullable=False, unique=True)
    email = db.Column(db.String(128), nullable=True, unique=True)
    password_hash = db.Column(db.String(128), nullable=True)


class UserSchema(ma.ModelSchema, BaseSchema):

    class Meta:
        model = User
        exclude = ('password_hash',)
```

> **Note:** We are excluding the `password_hash` property in the schema, because we should **never** send it with in our JSON response to the client

### `hybrid_property`

We need to add a `hybrid_property` which is similar to a _virtual_ in Mongoose:

```py
# models/user.py
from sqlalchemy.ext.hybrid import hybrid_property

class User(db.Model):

    __tablename__ = 'users'

    username = db.Column(db.String(20), nullable=False, unique=True)
    email = db.Column(db.String(128), nullable=True, unique=True)
    password_hash = db.Column(db.String(128), nullable=True)

    @hybrid_property
    def password(self):
        pass
```

So now we have created a `password` property on the user model. Unlike the `password_hash` that will contain the actual hashed password in the database, the `password` _hybrid property_ will be used to receive the plain text password from the user. Since the plain text password will **never** be store in the database, we can use a _hybrid property_.

### `setter` method

We can now create a setter method for our _hybrid property_. This allows us to control what happens when we receive a plaintext password from the user.

```py
# models/user.py
class User(db.Model):

    __tablename__ = 'users'

    username = db.Column(db.String(20), nullable=False, unique=True)
    email = db.Column(db.String(128), nullable=True, unique=True)
    password_hash = db.Column(db.String(128), nullable=True)

    @hybrid_property
    def password(self):
        pass

    @password.setter
    def password(self, plaintext):
        self.password_hash = bcrypt.generate_password_hash(plaintext).decode('utf-8')
```

> **Note:** The `@password.setter` function **must** be named `password` to match the name of the `hybrid_property`

When we receive the plain text password we use `bcrypt` to hash it and set that hash to the user's `password_hash` property.

### `validate_password` method

We also need a `validate_password` method that will check a plain text password against the hash stored in the database:

```py
# models/user.py
class User(db.Model):

    __tablename__ = 'users'

    username = db.Column(db.String(20), nullable=False, unique=True)
    email = db.Column(db.String(128), nullable=True, unique=True)
    password_hash = db.Column(db.String(128), nullable=True)

    @hybrid_property
    def password(self):
        pass

    @password.setter
    def password(self, plaintext):
        self.password_hash = bcrypt.generate_password_hash(plaintext).decode('utf-8')

    def validate_password(self, plaintext):
        return bcrypt.check_password_hash(self.password_hash, plaintext)
```

Luckily `bcrypt` does the heavy lifting for us here.

## Checking `password` matches `password_confirmation`

When the user registers they will send a `password` _and_ `password_confirmation` to the API. We need to check these match _before_ hashing the password.

Somewhat counterintuitively we'll do this check in the _schema_:

```py
class UserSchema(ma.ModelSchema, BaseSchema):

    @validates_schema
    def check_passwords_match(self, data):
        if data.get('password') != data.get('password_confirmation'):
            raise ValidationError(
                'Passwords do not match',
                'password_confirmation'
            )

    password = fields.String(required=True)
    password_confirmation = fields.String(required=True)

    class Meta:
        model = User
        exclude = ('password_hash',)
```

We also need to add the `password` and `password_confirmation`, since we need to use the schema to validate the user data in the register controller in the next step.

## Register route

Since the hashing is being performed in the model, the register route is fairly straightforward:

```py
# controllers/auth.py
from flask import Blueprint, jsonify, request
from models.user import User, UserSchema

router = Blueprint('auth', __name__)
user_schema = UserSchema()

@router.route('/register', methods=['POST'])
def register():
    user, errors = user_schema.load(request.get_json())

    if errors:
        return jsonify(errors), 422

    user.save()

    return jsonify({ 'message': 'Registration successful' }), 201
```

We are using the `user_schema` to _load_ the data from sent from the client. This returns a tuple, containing a `user` object if successful, or an `errors` dict containing the validation errors.

We check for errors, and if present send them back to the client as JSON with a 422 status code.

If there are no errors, we can save the user and send our 201 respone.

## Login route

The login route is a little more complex because we need to make a JWT if the user successfully authenticates. Firstly we need to install `pyjwt`:

```sh
pipenv install pyjwt
```

We can now write a function that generates a token. We could do this in the controller, but we want to keep our controllers skinny, so we'll add it to the `User` model, not forgetting to import `datetime`, `timedelta` build in Python modules:

```py
# models/user.py
def generate_token(self):
    payload = {
        'exp': datetime.utcnow() + timedelta(days=1),
        'iat': datetime.utcnow(),
        'sub': self.id
    }

    token = jwt.encode(
        payload,
        secret,
        'HS256'
    ).decode('utf-8')

    return token
```

The `generate_token` has been added to the `user` as a method, so `self` refers to the user that is logging in. We create a JWT using the `encode` method. Notice that we are importing a secret from `config/enviroment.py`. `HS256` is the encoding algorithm we are using.

With that in place we can write our login route:

```py
# controllers/auth.py
@router.route('/login', methods=['POST'])
def login():
    credentials = request.get_json()
    user = User.query.filter_by(email=credentials['email']).first()

    if not user or not user.validate_password(credentials['password']):
        return jsonify({ 'message': 'Unauthorized' }), 401

    return jsonify({ 'token': user.generate_token(), 'message': f'Welcome back {user.username}!' }), 200
```

## Securing routes

Just like with Express we can now write a `secure_route` function that we can use to check for a valid token on specific routes. This is an ideal use case for a _decorator_:

```py
# lib/secure_route.py
import jwt
from functools import wraps
from flask import request, jsonify, g
from config.environment import secret
from models.user import User

def secure_route(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        token = request.headers.get('Authorization', '').replace('Bearer ', '')

        try:
            payload = jwt.decode(token, secret)
            g.current_user = User.query.get(payload['sub'])

        except jwt.ExpiredSignatureError:
            # token has expired
            return jsonify({ 'message': 'Token expired' }), 401

        except Exception as e:
            # any other error has occurred
            return jsonify({ 'message': 'Unauthorized' }), 401

        return func(*args, **kwargs)

    return wrapper
```

Firstly we get the Authorization header from the request and extract the token from it. Then we attempt to decode the token. If successful, we attempt to find the user by the `sub` property in the token's payload, the user's ID. If the user is found we add it to Flask's `g` module.

`g` is a global object which can be used to hold data across the whole of our application. We can use it to store the user for access in our controllers.

If the token has expired an `ExpiredSignatureError` will be raised, which we can then use to inform the client of that specific issue. Any other error caused by the token, or the user lookup will be handled with a generic `Unauthorized` message.

We can now decorate specific routes with our function:

```py
from lib.secure_route import secure_route

@router.route('/cars', methods=['POST'])
@secure_route
def create():
    car, errors = car_schema.load(request.get_json())

    if errors:
        return jsonify(errors), 422

    car.save()

    return car_schema.jsonify(car), 201
```

If we need to access the current user inside the route we can use the `current_user` property stored on the `g` module:

```py
@router.route('/cars', methods=['POST'])
@secure_route
def create():
    car, errors = car_schema.load(request.get_json())

    if errors:
        return jsonify(errors), 422

    car.user = g.current_user
    car.save()

    return car_schema.jsonify(car), 201
```
