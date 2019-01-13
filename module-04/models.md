# Models with SQLAlchemy and Marshmallow

SQLAlchemy is an ORM for Python to programmatically access a SQL database. You can think of it as being similar to Mongoose.

SQLAlchemy allows us to create database entries with out needing to write SQL statements, since SQLAlchemy will do that for us.

## Installation

SQLAlchemy is a 3rd-party plugin which is in no way related to Flask. However there is a specific Flask integration which is designed to be used with Flask applications called `flask-sqlalchemy`.

We also need to install a PostgreSQL adapter for Python called `psycopg2-binary`

We can install them with `pipenv`:

```sh
pipenv install flask-alchemy psycopg2-binary
```

We can now hook it up to our Flask app like so:

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgres://localhost:5432/flask-sqlalchemy-example'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False # this speeds up SQLAlchemy

db = SQLAlchemy(app)

@app.route('/')
def home():
  return 'Hello World!', 200
```

## Usage

Now that SQLAlchemy has been installed, we need to create a model, which will define the structure of the database table that will store the data for our resource. For this example we will use cars as our resource.

### Defining a model

To define a model, we will create a class which extends SQLAlchemy's `Model` class:

```py
# models/car.py
from app import db

class Car(db.Model):

    __tablename__ = 'cars'

    id = db.Column(db.Integer, primary_key=True)
    make = db.Column(db.String(40), nullable=False)
    model = db.Column(db.String(40), nullable=False)
    lisence = db.Column(db.String(8), unique=True, nullable=False)
    color = db.Column(db.String(20))
```

The _magic_ property `__tablename__` is used to name our table. If we omit this property the table will be named after the class (`car` in this case).

We define the table structure using the `Column` class, and various datatypes, which include:

* `Integer`
* `Float`
* `String`
* `Text`
* `Enums`
* `Boolean`
* `Date`
* `Time`
* `DateTime`

### Creating tables and seeding data

Now that we have defined our model and table structure, we can get SQLAlchemy to generate our tables, and seed some data:

```py
# seeds.py
from app import app, db

from models.car import Car

with app.app_context():
    db.drop_all()   # drop all the database tables
    db.create_all() # create all the database tables

    # create some cars
    polo = Car(make="VW", model="Golf", lisence="GH12 7LL", color="blue")
    porche = Car(make="Porche", model="911", lisence="SL7 7LL", color="black")

    # add the cars to the session
    db.session.add(polo)
    db.session.add(porche)

    # save the data to the database
    db.session.commit()
```

The `db.session` collates all the database interactions. When `commit()` is called SQLAlchemy generates the appropriate SQL statements to update the database.

We should now be able to seed the database with the following command:

```sh
pipenv run python seeds.py
```

> **Note:** You will need to ensure that you have created a database before you attempt to seed it. The database name should match the `SQLALCHEMY_DATABASE_URI` variable in `main.py`:
> ```sh
  createdb flask-sqlalchemy-example
  ```

To check that the database has seeded correctly, we can check it using `psql`:

```sh
psql flask-sqlalchemy-example
```

```sql
SELECT * FROM `cars`
```

You should see the following:

```
 id |  make  | model | lisence  | color
----+--------+-------+----------+-------
  1 | VW     | Golf  | GH12 7LL | blue
  2 | Porche | 911   | SL7 7LL  | black
```

## Serialisation with Marshmallow

Finally we need to be able to retrieve data from our database with SQLAlchemy, and convert it into JSON to be sent to the client. Unfortunately, SQLAlchemy models cannot be converted into JSON directly. To do that, we are going to use another package called Marshmallow.

> **Note:** Converting an object (like a model), into a string (like JSON), is known as _serialisation_. Converting a string into an object is known as _deserialisation_

Marshmallow is a stand-alone package, but does come with integrations for SQLAlchemy and Flask. Install them like so:

```sh
pipenv install marshamallow-sqlalchemy flask-marshmallow
```

Now now we can add them to `main.py`:

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgres://localhost:5432/flask-sqlalchemy-example'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
ma = Marshmallow(app)

@app.route('/')
def home():
  return 'Hello World!', 200
```

Back in the model, we can create a _schema_ for the car:

```py
from app import db, ma # import Marshmallow here

class Car(db.Model):

    __tablename__ = 'cars'

    id = db.Column(db.Integer, primary_key=True)
    make = db.Column(db.String(40), nullable=False)
    model = db.Column(db.String(40), nullable=False)
    lisence = db.Column(db.String(8), unique=True, nullable=False)
    color = db.Column(db.String(20))


class CarSchema(ma.ModelSchema):

  class Meta:
    model = Car
```

With that done we can replace our homepage route with an INDEX route for our cars resource:

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgres://localhost:5432/flask-sqlalchemy-example'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
ma = Marshmallow(app)

from models.car import Car, CarSchema
car_schema = CarSchema(many=True) # many=True will handle an array of cars

@app.route('/cars')
def index():
  cars = Car.query.all() # get all the cars from the database

  return car_schema.jsonify(cars), 200 # serialise the cars to JSON
```

To check this works, start the server, and test the endpoint in insomnia. You should get the following response:

```json
[
  {
    "id": 1,
    "make": "VW",
    "model": "Polo",
    "lisence": "GH12 7LL",
    "color": "blue"
  }, {
    "id": 2,
    "make": "Porche",
    "model": "911",
    "lisence": "SL7 7LL",
    "color": "black"  
  }
]
```

## Further reading

* [Flask SQLAlchemy Docs](http://flask-sqlalchemy.pocoo.org/2.3/)
* [Flask Marshmallow Docs](https://flask-marshmallow.readthedocs.io/en/latest/)
