# Constructors & Prototype

When working with objects we have a _class_ or, in the case of JavaScript a _constructor_, and an _instance_. A constructor is a blueprint of the object, it holds instructions on what properties and methods an object should have. The _instance_ is actual object itself.

## Using constructors

We can create an array using a constructor function like so:

```js
const array = new Array();
```

`Array` is the constructor function. When we call the function (and precede it with the `new` keyword) we ask it to create a new _instance_ of the array constructor.

The array has been _instantiated_ (it is created in memory) and all of its properties and methods are ready to be used. In the case of an array, things like `.push()`, `.pop()`, `.length` etc.

>**Note**: when we create an array with an array literal `const array = []`, the array is still instantiated with a constructor function in the background.

However, if we console log the array, we don't see all these methods, even though they are there.

## Prototype

This is where the constructor function's `prototype` comes in to play. The `prototype` is where all the instance's built-in methods live. Rather than keeping a copy of the in-built methods on the instance, they remain on the constructor function and can be freely accessed by the instance when needed. This massively reduces memory requirements.

Let's imagine we have a program which has 200,000 lines of code, and 100,000 arrays. If each array had its own `.push()` and `.pop()` methods, those functions would be declared 100,000 times each requiring huge amounts of memory. By keeping them on the constructor's prototype they only need to be declared once, regardless of how many array instances are created.

If we log `Array.prototype` in  the console, we can see all of the in-built array methods:

![](https://user-images.githubusercontent.com/3531085/35680151-611ebbd2-0751-11e8-8681-8f26f83c3cd5.png)

You can think of `prototype` as like a shared instruction manual. Rather than everyone owning the manual, any time someone needs to know how to do something, they can look it up in the shared manual. That way you save resources by not having to print out a new manual every time you hire a new employee.

## Creating a `constructor`

In the following example we'll make a `Car` constructor, which will allow us to create car objects. The car objects will have `brand`, `numerOfSeats`, `speed` and `maxSpeed` properties, and `speedUp` and `slowDown` methods.

```js
function Car(brand, numberOfSeats, maxSpeed) {
  this.brand = brand;
  this.numberOfSeats = numberOfSeats;
  this.maxSpeed = maxSpeed;
  this.speed = 0;
}

Car.prototype.speedUp = function(amount) {
  if((this.speed + amount) >= this.maxSpeed) this.speed = this.maxSpeed;
  else this.speed += amount;
  return this.speed;
}

Car.prototype.slowDown = function(amount) {
  if((this.speed - amount) >= 0) this.speed = 0;
  else this.speed -= amount;
  return this.speed;
}

const mustang = new Car('Ford Mustang GT', 2, 164);
const honda = new Car('Honda Civic', 4, 94);
```

>**Note**: Inside the constructor function `this` refers to the instance that will be created.

The two car instances, `mustang` and `honda` have access to the `Car` constructor's prototype, so have the built-in methods `speedUp` and `slowDown`.

### ES6 syntax

Constructor functions are unique to JavaScript, most other languages have _classes_. A class, much like a JavaScript constructor is a blueprint for an object instance. ES6 tries to mimic other programming languages with its own `class` keyword, which can be used to make a constructor function.

If we re-wrote our `Car` constructor function with the new `class` syntax, it would look like this:

```js
class Car {
  constructor(brand, numberOfSeats, maxSpeed) {
    this.brand = brand;
    this.numberOfSeats = numberOfSeats;
    this.maxSpeed = maxSpeed;
    this.speed = 0;
  }

  speedUp(amount) {
    if((this.speed + amount) >= this.maxSpeed) this.speed = this.maxSpeed;
    else this.speed += amount;
    return this.speed;
  }

  slowDown(amount) {
    if((this.speed - amount) >= 0) this.speed = 0;
    else this.speed -= amount;
    return this.speed;
  }
}

const mustang = new Car('Ford Mustang GT', 2, 164);
const honda = new Car('Honda Civic', 4, 94);
```

The `class` syntax is simply a different way of writing a constructor function, however it is far more similar to Ruby or Python or pretty much any other C based language. However, it is important to note that under the hood, a constructor function is being created and the `speedUp` and `slowDown` methods are being attached to the `prototype`.

## Further reading

* [JavaScript Constructors & Prototype](http://tobyho.com/2010/11/22/javascript-constructors-and/)
* [Understanding JavaScript Constructors](https://css-tricks.com/understanding-javascript-constructors/)
* [JavaScript Constructors, Prototypes and the `new` Keyword](https://content.pivotal.io/blog/javascript-constructors-prototypes-and-the-new-keyword)
