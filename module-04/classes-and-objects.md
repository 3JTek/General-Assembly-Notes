# Classes & Objects

In Python an object is identical to an object in JavaScript, however, an object in JavaScript might be more similar to a dictionary in some circumstances.

Whenever we want to make an object which represents something in the real world, or one which has its own methods and properties we need to first make a `class` in Python. The `class` is the blueprint for the objects or _instances_ that the class can create. It defines the properties and methods of the instance and how they should behave.

> **Note:** We can also create classes in JavaScript, although technically speaking these are _constructor functions_. The concept is the same though.

## Creating a class

Let's looks at an example. We shall make a `Car` class that will have properties and methods that will define the car and allow it to modify its properties:

```py
class Car:

  def __init__(self, make, color, seats):
    self.make = make
    self.color = color
    self.seats = seats
    self.mph = 0

  def accelerate(self, amount):
    self.mph += amount

  def decelerate(self, amount):
    self.mph -= amount


ford = Car('ford', 'red', 2)
fiat = Car('fiat', 'yellow', 5)

print(ford) # <__main__.Car instance at 0x105bac7e8>
print(fiat) # <__main__.Car instance at 0x105bac950>

ford.mph # 0
ford.accelerate(60)
ford.mph # 60
ford.decelerate(40)
ford.mph # 20
```

The syntax is quite similar to ES6 `class` syntax, with a couple of notable differences:

* The constructor method is called `__init__`
* `this` is replaced with `self`
* The first argument to each method definition is `self`, even though it is not pass when the method is called
* The methods are prefixed with the `def` keyword

## `__str__` vs `__repr__`

As well as the `__init__` method, there are two other special methods `__str__` and `__repr__` which are very similar.

You'll notice when you print an object in Python it will give you a rather odd output:

```sh
<__main__.Car instance at 0x105bac7e8>
```

This tells us the class that was used to create the object, and the position in memory that it is located. We can modify this using `__str__` or `__repr__`. They both override this _representation_ of the object.

`__repr__` is a representation of the object, `__str__` is what the object should output if converted to a string. The difference is **very** subtle.

According to the docs:

- `__repr__` is designed to be unambiguous (ie, should contain something that uniquely identifies the object)
- `__str__` is designed to be ambiguous (ie, could be the same for multiple objects)

We can add it to our `Car` class like so:

```py
class Car:

  def __init__(self, make, color, seats):
    self.make = make
    self.color = color
    self.seats = seats
    self.mph = 0

  def accelerate(self, amount):
    self.mph += amount

  def decelerate(self, amount):
    self.mph -= amount

  def __repr__(self):
    return "<%s %s>" % (self.__class__, hex(id(self)))

  def __str__(self):
    return "%s %s travelling at %s mph" % (self.color, self.make, self.mph)


ford = Car('ford', 'red', 2)
ford # <__main__.Car 0x105bac7e8>
print(fiat) # "red ford travelling at 0 mph"
```

## Further reading

* [A Beginner's Python Tutorial/Classes - wikibooks](https://en.wikibooks.org/wiki/A_Beginner%27s_Python_Tutorial/Classes)
* [How to Construct Classes and Define Objects in Python 3 - DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-construct-classes-and-define-objects-in-python-3)
