# `this`

What `this` is refers to in Javascript code, (`this` binding) is a constant source of confusion for the new JavaScript developer.

## How variables work

Before we get into `this` we need to understand exactly what variables (and arguments) are, and how they work.

### Primitive data types

With _primitive_ data types, like strings and numbers, the value is allocated to the variable, a bit like a key/value pair. So in this sense we can say that the variable is like a container. It contains the data:

```js
let a = 10
let b = a

a = 11

console.log(a) // => 11
console.log(b) // => 10
```

Here, because `a` refers to a primitive, when we define `b` it is also allocated the same value.

When we change `a`, it has no effect on `b`.

### Composite data types

Let's look at a similar example with a more complex data type:

```js
let a = { name: 'Mike' }
let b = a

b.name = 'Stephanie'

console.log(a) // => { name: 'Stephanie' }
console.log(b) // => { name: 'Stephanie' }
```

With _composite_ data types, like objects and arrays, the variable behaves like a pointer. It points to where the value is located in memory.

In the example above `a` and `b` are both pointing to the **same object**. We can access the object using either variable, so any change in that object will be seen using either variable.

## `this` is a pointer

`this` is a variable and because `this` **ALWAYS** refers to an object, it will behave like a pointer.

The thing that makes `this` confusing is that what it is pointing to depends on a few different factors.

### Default

Let's start simple, with the default behaviour of `this`:

```js
this === window
// => true

function foo() {
  return this
}

foo()
// => Window
```

By default, `this` points to the `window`. The `window` object is the edge of the universe for JavaScript, it is also how JavaScript interacts with the browser.

### Strict mode

If we add `'use strict'` at the top of our scripts, we tell the browser that we want to use a more strict set of rules when running our JavaScript files. The default behaviour of `this` will now change:

```js
'use strict'

this === window
// => true

function foo() {
  return this
}

foo()
// => undefined
```

In strict mode, `this` in the functional scope is `undefined`. More on that later.

### Implicit binding

When we call a function from the _context_ of an object, it is known as a **method**. When a method is called, `this` points to the `context`.

**Context** is a confusing word here, but very simply it means _the object that the method was called from_.

Let's look at an example:

```js
function foo() {
  return this
}

const me = {
  name: 'Mike',
  foo: foo
}

foo()
// => Window

me.foo()
// => Object { name: 'Mike', foo: [function foo] }

```

When we call `foo` with no context, it returns the `window` (or `undefined` if we use strict mode).

When we call `foo` from the context of `me`, ie. `me.foo()`, `this` points to `me`.

This is known as _implicit_ binding. Where `this` points is _implied_ by the _context_.

A very common example of the use of implicit binding is with JavaScript event listeners:

```js
const btn = document.querySelector('button')
btn.onclick = function() {
  console.log(this)
}
```

In this example `this` is pointing to the button that was clicked.

### Explicit binding

We can explicitly set where `this` is pointing using the methods `call` or `bind`.

#### Using `call`:

```js
function foo() {
  return this
}

const me = {
  name: 'Mike',
  foo: foo
}

me.foo()
// => Object { name: 'Mike', foo: [function foo] }

me.foo.call(window)
// => Window
```

When we use `call`, we set what `this` points to **and** we call, or _invoke_ the function.

#### Using `bind`:

```js
function foo() {
  return this
}

const me = {
  name: 'Mike',
  foo: foo
}

const boundMe = me.foo.bind(window)
boundMe()
// => Window
```

With `bind`, we create a new function where `this` points to whatever we pass as the argument of `bind`.

### With a constructor

When we use `this` in a constructor method it refers to the instance that will be returned from the function:

```js
function Car(brand, numberOfSeats, maxSpeed) {
  this.brand = brand
  this.numberOfSeats = numberOfSeats
  this.maxSpeed = maxSpeed
  this.speed = 0
}

const mustang = new Car('Ford Mustang GT', 2, 164)
```

When the `Car` object is instantiated its properties will have been set using `this`.

## Overview of `this`

- `this` is a variable
- `this` always points to an object
- Where `this` points is set **when a function is called**
- When used in a function, `this` is the `window` (or `undefined` if we are using strict mode)
- When called from the _context_ of an object, `this` points to that object
- `this` can be _explicitly_ set using `call` or `bind`.
- With constructors `this` refers to the instance that will be returned from the function.

## Nobody panic!!

Developing a good understanding of `this` takes time and experience. _Don't panic_. As a junior developer you will not be expected to fully understand the concept. Just remember that if you are not sure what `this` is pointing to, or if `this` behaves unexpectedly, you can always use `console.log` to find out what `this` is.

## Further reading

* [this - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
* [Understanding the `this` Keyword in JavaScript](https://toddmotto.com/understanding-the-this-keyword-in-javascript/)
* [JavaScript `this` Keyword Explained Simply](https://medium.com/@NinjaJavaScript/javascript-this-keyword-explained-simply-e90762d4945d)
