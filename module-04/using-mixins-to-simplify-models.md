# Using Mixins to Simplify Models and Schemas

There are a number of properties and methods that we might want to share across all our models and schemas, so rather than duplicate them, we can use a _mixin_. A _mixin_ can be used to inherit a secondary set of properties and methods to a class.

## Model helper methods

All models will need an `id` field, but we can also add `save` and `remove` helper methods to simplify our controller code. Let's add theses to a `BaseModel` class:

```py
# models/base.py
from app import db

class BaseModel:

    id = db.Column(db.Integer, primary_key=True)

    def save(self):
      db.session.add(self)
      db.session.commit()

    def remove(self):
      db.session.delete(self)
      db.session.commit()
```

We can now add this to a model like so:

```py
# models/car.py
from app import db
from .base import BaseModel

class Car(db.Model, BaseModel):

    __tablename__ = 'cars'

    make = db.Column(db.String(40), nullable=False)
    model = db.Column(db.String(40), nullable=False)
    lisence = db.Column(db.String(8), unique=True, nullable=False)
    color = db.Column(db.String(20))
```

Notice that the `id` has now been removed from the `Car` class. This will be added, along with the `save` and `remove` methods from the `BaseModel`.

### Adding timestamps

With this in place we can easily add timestamps to all of our models:

```py
from app import db

class BaseModel:

    id = db.Column(db.Integer, primary_key=True)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    updated_at = db.Column(db.DateTime, default=datetime.utcnow)

    def save(self):
      self.updated_at = datetime.utcnow()

      db.session.add(self)
      db.session.commit()

    def remove(self):
      db.session.delete(self)
      db.session.commit()
```

All models will now have a `created_at` and `updated_at` property that will be updated on save.

## Schema helper methods

We can use the same concept to add functionality to all our schemas. For example we can format the timestamps like so:

```py
# models/base.py
class BaseSchema:

  created_at = fields.DateTime(format='%Y-%m-%d %H:%M:%S')
  updated_at = fields.DateTime(format='%Y-%m-%d %H:%M:%S')
```

Making sure we add our _mixin_ to our schemas:

```py
# models/car.py
from .base import BaseSchema

class CarSchema(ma.ModelSchema, BaseSchema):

    class Meta:
        model = Car
```
