# Controllers with Blueprint

We will be using Flask's Blueprint pattern to create mini routers for each resource, which will include all the logic for getting data from the models and returning JSON to the client. Essentially each controller will also contain its own routing.

Let's take a look at a simple RESTful controller for a `Car` model.

## Setup

Initialise the controller will the required modules:

```py
from flask import Blueprint, jsonify, request
from marshmallow import ValidationError
from models.car import Car, CarSchema
from app import db
```

* `Blueprint` will be use to make the mini controller
* `jsonify` will be used to send custom JSON responses
* `request` will be used to access request data like URL parameters and JSON sent from the client. _Equivalent to `req` in Express._

We can create our car router like so:

```py
router = Blueprint(__name__, 'cars')
```

An instantiate our car schema, this will be used to _serialise_ and _deserialise_ our data to and from JSON:

```py
car_schema = CarSchema()
```

## INDEX route

```py
@router.route('/cars', methods=['GET'])
def index():
    cars = Car.query.all()

    return cars_schema.jsonify(cars, many=True), 200
```

The schema's `jsonify` method converts the data from the database into JSON. Note the `many=True` keyword argument. This tells the schema to expect a collection of cars, rather than just one.

## SHOW route

```py
@router.route('/cars/<int:id>', methods=['GET'])
def show(id):
    car = Car.query.get(id)

    if not car:
        return jsonify({'message': 'Not found'}), 404

    return car_schema.jsonify(car), 200
```

Here Flask's `jsonify` method can be used to convert dictionaries to JSON, which is great for sending custom messages to the client.

## CREATE route

```py
@router.route('/cars', methods=['POST'])
def create():
    car, errors = car_schema.load(request.get_json())

    if errors:
        return jsonify(errors), 422

    car.save()

    return car_schema.jsonify(car), 201
```

The `request.get_json` method will convert JSON sent from the client into a dictionary. This can then be used by the schema's `load` method to create a car object or any errors that may have occurred. If there are errors, we can send them back to the client with a `422` response. Otherwise we can save the car and return it as JSON.

## UPDATE route

```py
@router.route('/cars/<int:id>', methods=['PUT'])
def update(id):
    car = Car.query.get(id)

    if not car:
        return jsonify({'message': 'Not found'}), 404

    car, errors = car_schema.load(request.get_json(), instance=car)

    if errors:
        return jsonify(errors), 422

    car.save()

    return car_schema.jsonify(car), 200
```

Very similar to the _CREATE_ route, except here you will notice the `instance` keyword argument in the schema's load method. This supplies the model which should be updated by the incoming JSON. Again any errors can be returned to the client, otherwise we save the updated car, and return it as JSON.

## DELETE route

```py
@router.route('/cars/<int:id>', methods=['DELETE'])
def delete(id):
    car = Car.query.get(id)

    if not car:
        return jsonify({'message': 'Not found'}), 404

    car.remove()

    return '', 204
```

The should be fairly clear. The only new part is the car's `remove` method, which we created earlier, which will remove the car from the database.

## Further reading

* [Routing - Flask Docs](http://flask.pocoo.org/docs/1.0/quickstart/#routing)
* [What are Flask Blueprints, exactly? - StackOverflow](https://stackoverflow.com/questions/24420857/what-are-flask-blueprints-exactly)
