# Creating relationships with SQLAlchemy

Creating relationships between models is vital for any application. Let's look at how we can do that with SQLAlchemy. To do that we'll use the cheese example we used earlier.

Let's remind ourselves of that:

![erd](https://media.git.generalassemb.ly/user/15120/files/9357bb80-0796-11e9-82a0-737dbcf72168)


## One-to-many relationship

Let's start with the relationship between cheeses and comments:

* A cheese can have **many** comments
* A comment belongs to **one** cheese

Let's start with our models:

```py
# models/cheese.py
from app import db, ma

class Cheese(db.Model):

    __tablename__ = 'cheeses'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(40), unique=True, nullable=False)
    origin = db.Column(db.String(40), nullable=False)
    tasting_notes = db.Column(db.Text)
    image = db.Column(db.String(140))


class CheeseSchema(ma.ModelSchema):

    class Meta:
        model = Cheese


class Comment(db.Model):

    __tablename__ = 'comments'

    id = db.Column(db.Integer, primary_key=True)
    content = db.Column(db.Text, nullable=False)
    cheese_id = db.Column(db.Integer, db.ForeignKey('cheeses.id'))
    cheese = db.relationship('Cheese', backref='comments')


class CommentSchema(ma.ModelSchema):

    class Meta:
        model = Comment
```

> **Note:** We are putting both models in the same file here because the comment would not exist without a cheese. Similar to an embedded record in mongo.

Notice the `db.ForeignKey` this indicates that the `cheese_id` column is going to store a reference to a cheese ID. SQLAlchemy will ensure that only ID that are present in the `cheeses` table are allowed here.

> **Note:** It's important that the argument passed to `db.ForeignKey` matches the `__tablename__` of the table that you are making a relationship with. Here we are using `cheeses.id` not `cheese.id`

You will also see the `db.relationship` method. This tells SQLAlchemy which model it should use when creating and retrieving data from the database. The `backref` means that a `comments` property will be created on the cheese model, containing all of the comments for that cheese.

> **Note:** You must import the model at the top of the file that you want to add to the relationship, otherwise you will get an error.

### Adding some data

Ok, now we have our models set up, let's seed some data:

```py
# seeds.py
from app import app, db

from models.cheese import Cheese
from models.comment import Comment

with app.app_context():
    db.drop_all()   # drop all the database tables
    db.create_all() # create all the database tables

    # create some cheeses
    brie = Cheese(name='Brie', origin='France', tasting_notes='An excellent dessert cheese')
    mozzarella = Cheese(name='Mozzarella', origin='Italy', tasting_notes='Milky')

    # create some comments
    comment1 = Comment(content='Not for me, mouldy nonsense!', cheese=brie)
    comment2 = Comment(content='Great on pizza or in a salad', cheese=mozzarella)
    comment3 = Comment(content='Bland', cheese=mozzarella)

    # add the data to the session
    db.session.add(brie)
    db.session.add(mozzarella)
    db.session.add(comment1)
    db.session.add(comment2)
    db.session.add(comment3)

    # save the data to the database
    db.session.commit()
```

## Retrieving the data as JSON

Now that we have some data to work with, let's create a simple _INDEX_ route to display the data:

```py
# controllers/cheeses.py
from flask import Blueprint, jsonify, request
from models.cheese import Cheese, CheeseSchema
from app import db

router = Blueprint('cheeses', __name__)

cheeses_schema = CheeseSchema(many=True)

@router.route('/cheeses', methods=['GET'])
def index():
    cheeses = Cheese.query.all()

    return cheeses_schema.jsonify(cheeses), 200
```

Let's add that to our routes:

```py
# config/routes.py
from app import app
from controllers import cheeses

app.register_blueprint(cheeses.router, url_prefix='/api')
```

And add our routes to our main file:

```py
# app.py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_marshmallow import Marshmallow

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgres://localhost:5432/flask-sqlalchemy-relationships'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)
ma = Marshmallow(app)

from config import routes # import routes at the bottom of the file
```

We can now test our endpoint in Insomnia. We should get something like this:

```json
[
  {
    "id": 1,
    "image": null,
    "name": "Brie",
    "origin": "France",
    "tasting_notes": "An excellent dessert cheese"
  },
  {
    "id": 2,
    "image": null,
    "name": "Mozzarella",
    "origin": "Italy",
    "tasting_notes": "Milky"
  }
]
```

You should notice that none of the comments have appeared. Although the relationship has been correctly established, we need to update the `CheeseSchema` to include the comments as a nested field:

```py
class CheeseSchema(ma.ModelSchema):

    class Meta:
      model = Cheese

    comments = fields.Nested('CommentSchema', many=True)
```

With the `CheeseSchema` updated our JSON output should look something like this:

```json
[
  {
    "comments": [
      {
        "cheese": 1,
        "content": "Not for me, mouldy nonsense!",
        "id": 1
      }
    ],
    "id": 1,
    "image": null,
    "name": "Brie",
    "origin": "France",
    "tasting_notes": "An excellent dessert cheese"
  },
  {
    "comments": [
      {
        "cheese": 2,
        "content": "Great on pizza or in a salad",
        "id": 2
      },
      {
        "cheese": 2,
        "content": "Bland",
        "id": 3
      }
    ],
    "id": 2,
    "image": null,
    "name": "Mozzarella",
    "origin": "Italy",
    "tasting_notes": "Milky"
  }
]
```

You should notice the `cheese` property on each comment. This matched the cheese ID of the cheese that it relates to. While this is important for the database to establish the relationship, for the use it is redundant. We can remove this in the schema:

```py
class CheeseSchema(ma.ModelSchema):

    class Meta:
      model = Cheese

    comments = fields.Nested('CommentSchema', many=True, exclude=('cheese',))
```

## Many-to-many relationship

Let's now tackle the relationship between cheeses and categories:

* A cheese can belong to **many** categories
* A category can have **many** cheeses

Firstly let's create our category model:

```py
from app import db, ma
from marshmallow import fields

class Category(db.Model):

    __tablename__ = 'categories'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(40), unique=True, nullable=False)


class CategorySchema(ma.ModelSchema):

    class Meta:
        model = Category
```

When we have a many-to-many relationship, we need a _join table_. To create that we can add the following code to `models/cheese.py`:

```py
categories_cheeses = db.Table('categories_cheeses',
    db.Column('category_id', db.Integer, db.ForeignKey('categories.id'), primary_key=True),
    db.Column('cheese_id', db.Integer, db.ForeignKey('cheeses.id'), primary_key=True)
)
```

Here we have defined a _join table_ called `categories_cheeses`, with `category_id` and `cheese_id`. With this established we can update our `Cheese` and `CheeseSchema` to make use of it:

```py
categories_cheeses = db.Table('categories_cheeses',
    db.Column('category_id', db.Integer, db.ForeignKey('category.id'), primary_key=True),
    db.Column('cheese_id', db.Integer, db.ForeignKey('cheese.id'), primary_key=True)
)

class Cheese(db.Model):

    __tablename__ = 'cheeses'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(40), unique=True, nullable=False)
    origin = db.Column(db.String(40), nullable=False)
    tasting_notes = db.Column(db.Text)
    image = db.Column(db.String(140))
    categories = db.relationship('Category', secondary=categories_cheeses, backref='cheeses')


class CheeseSchema(ma.ModelSchema):

    class Meta:
        model = Cheese

    comments = fields.Nested('CommentSchema', many=True, exclude=('cheese',))
    categories = fields.Nested('CategorySchema', many=True)
```

Notice the `secondary` keyword argument. This tells SQLAlchemy that it needs to use the specified join table to make the relationship work.

Let's add some categories in our seeds file:

```py
from app import app, db

from models.cheese import Cheese, Comment
from models.category import Category

with app.app_context():
    db.drop_all()   # drop all the database tables
    db.create_all() # create all the database tables

    # create some categories
    soft = Category(name='soft')
    hard = Category(name='hard')
    blue = Category(name='blue')

    # create some cheeses
    brie = Cheese(name='Brie', origin='France', tasting_notes='An excellent dessert cheese', categories=[soft])
    mozzarella = Cheese(name='Mozzarella', origin='Italy', tasting_notes='Milky', categories=[soft])
    gorgonzola = Cheese(name='Gorgonzola', origin='Italy', tasting_notes='Tangy', categories=[soft, blue])

    # create some comments
    comment1 = Comment(content='Not for me, mouldy nonsense!', cheese=brie)
    comment2 = Comment(content='Great on pizza or in a salad', cheese=mozzarella)
    comment3 = Comment(content='Bland', cheese=mozzarella)
    comment4 = Comment(content='An absolute classic!', cheese=gorgonzola)

    # add the data to the session
    db.session.add(soft)
    db.session.add(hard)
    db.session.add(blue)
    db.session.add(brie)
    db.session.add(mozzarella)
    db.session.add(gorgonzola)
    db.session.add(comment1)
    db.session.add(comment2)
    db.session.add(comment3)
    db.session.add(comment4)

    # save the data to the database
    db.session.commit()
```

However if we run the seeds file we will get the following error:

```
sqlalchemy.exc.InvalidRequestError: When initializing mapper Mapper|Cheese|cheeses, expression 'Category' failed to locate a name ("name 'Category' is not defined"). If this is a class name, consider adding this relationship() to the <class 'models.cheese.Cheese'> class after both dependent classes have been defined.
```

The issue here is that the `Category` model has not been loaded into memory at the point that we are referencing it in the `Cheese` model. To fix this we can import it like so:

```py
# models/cheese.py
from app import db, ma
from models.category import Category # load the category model here

class Cheese(db.Model):

    __tablename__ = 'cheeses'

    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(40), unique=True, nullable=False)
    origin = db.Column(db.String(40), nullable=False)
    tasting_notes = db.Column(db.Text)
    image = db.Column(db.String(140))
    categories = db.relationship('Category', secondary=categories_cheeses, backref='cheeses')
```

The seeds file should run now without any errors. We should now be able to test the endpoint in Insomnia:

```json
[
  {
    "categories": [
      {
        "id": 1,
        "name": "soft"
      }
    ],
    "comments": [
      {
        "content": "Not for me, mouldy nonsense!",
        "id": 1
      }
    ],
    "id": 1,
    "image": null,
    "name": "Brie",
    "origin": "France",
    "tasting_notes": "An excellent dessert cheese"
  },
  {
    "categories": [
      {
        "id": 1,
        "name": "soft"
      }
    ],
    "comments": [
      {
        "content": "Great on pizza or in a salad",
        "id": 2
      },
      {
        "content": "Bland",
        "id": 3
      }
    ],
    "id": 2,
    "image": null,
    "name": "Mozzarella",
    "origin": "Italy",
    "tasting_notes": "Milky"
  },
  {
    "categories": [
      {
        "id": 1,
        "name": "soft"
      },
      {
        "id": 2,
        "name": "blue"
      }
    ],
    "comments": [
      {
        "content": "An absolute classic!",
        "id": 4
      }
    ],
    "id": 3,
    "image": null,
    "name": "Gorgonzola",
    "origin": "Italy",
    "tasting_notes": "Tangy"
  }
]
```

We can now check the reverse relationship. To do that we first need to create a category endpoint:

```py
# controllers/categories.py
from flask import Blueprint, jsonify, request
from models.category import Category, CategorySchema
from app import db

router = Blueprint('categories', __name__)

categories_schema = CategorySchema(many=True)

@router.route('/categories', methods=['GET'])
def index():
    categories = Cheese.query.all()

    return categories_schema.jsonify(categories), 200
```

```py
# config/routes.py
from app import app
from controllers import cheeses, categories

app.register_blueprint(cheeses.router, url_prefix='/api')
app.register_blueprint(categories.router, url_prefix='/api')
```

We can now test the endpoint in Insomnia:

```json
[
  {
    "id": 1,
    "name": "soft"
  },
  {
    "id": 2,
    "name": "blue"
  },
  {
    "id": 3,
    "name": "hard"
  }
]
```

To add the cheeses, let's update the schema:

```py
class CategorySchema(ma.ModelSchema):

    class Meta:
        model = Category

    cheeses = fields.Nested('CheeseSchema', many=True)
```

If we test the endpoint again, we'll see the following error in the terminal:

```
RecursionError: maximum recursion depth exceeded while calling a Python object
```

This is because the category going to display the cheeses, which will in turn display the categories, which will display the cheeses and so on. To prevent this from happening we need to exclude the categories from the nested schema:

> **Note:** To keep things concise we are also excluding the comments

```py
class CategorySchema(ma.ModelSchema):

    class Meta:
        model = Category

    cheeses = fields.Nested('CheeseSchema', many=True, exclude=('categories', 'comments'))
```

Testing the endpoint again should give us the following JSON:

```json
[
  {
    "cheeses": [
      {
        "id": 1,
        "image": null,
        "name": "Brie",
        "origin": "France",
        "tasting_notes": "An excellent dessert cheese"
      },
      {
        "id": 2,
        "image": null,
        "name": "Mozzarella",
        "origin": "Italy",
        "tasting_notes": "Milky"
      },
      {
        "id": 3,
        "image": null,
        "name": "Gorgonzola",
        "origin": "Italy",
        "tasting_notes": "Tangy"
      }
    ],
    "id": 1,
    "name": "soft"
  },
  {
    "cheeses": [
      {
        "id": 3,
        "image": null,
        "name": "Gorgonzola",
        "origin": "Italy",
        "tasting_notes": "Tangy"
      }
    ],
    "id": 2,
    "name": "blue"
  },
  {
    "cheeses": [],
    "id": 3,
    "name": "hard"
  }
]
```

## Further reading

* [Declaring Models - Flask SQLAlchemy](http://flask-sqlalchemy.pocoo.org/2.3/models/)
* [Nesting Schemas - Flask Marshmallow](https://marshmallow.readthedocs.io/en/2.x-line/nesting.html)
