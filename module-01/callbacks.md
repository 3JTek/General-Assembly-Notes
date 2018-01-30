# Callbacks

A callback is a function which is passed to another function as an argument. A function that takes another function as an argument is known as a _higher order function_.


## `forEach`

`forEach` is a higher order function, it takes a callback as its first argument:

```js
['elephant', 'kangaroo', 'dolphin'].forEach(function(element, index, array) {
  console.log(element, index, array);
});
```

The first argument of `forEach` is a callback function. It can be re-written like this:

```js
function logElement(element, index, array) {
  console.log(element, index, array);
}

['elephant', 'kangaroo', 'dolphin'].forEach(logElement);
```

Both examples are exactly the same, except the first declares the callback function inside the `forEach` method, and the second declares the callback before passing it as an argument.

## Functions are objects

The reason this is possible is that functions are objects, and can be named, or stored in variables and passed as arguments. It is important to note that the **function declaration** is passed as an argument, not the function invocation.

Let's have a look at what happens when we invoke the function inside the `forEach` method:

```js
function logElement(element, index, array) {
  console.log(element, index, array);
}

['elephant', 'kangaroo', 'dolphin'].forEach(logElement()); // undefined is not a function
```

When we invoke a function JavaScript runs it and the return value is used in its place. `logElement` has not explicit return value, so it returns `undefined` instead. The above is the equivalent of:

```js
['elephant', 'kangaroo', 'dolphin'].forEach(undefined);
```

## Under the hood

But what is going on under the hood with `forEach`? Let's write our own version of `forEach` to better understand:

```js
const animals = ['elephant', 'kangaroo', 'dolphin'];

// overwrite the existing forEach function
animals.forEach = function(callback) {
  // callback refers to the function passed as the first argument
  for(let i=0; i<this.length; i++) {
    // we iterate over animals (`this`) and call the callback function once per iteration, passing the element, index and array
    callback(this[i], i, this);
  }
}
```

As you can see, we iterate over the animals array, (using `this` to refer to it), and call the callback function once per iteration, passing the correct arguments to it. This is where the name comes from: the function is _called back_ by the method it was passed to.

Whenever you see a function passed as an argument to another function, it will be _called back_ at some point by the higher order function.

## Methods as callbacks

It's important to understand that when we pass a method as a callback, it is passed _without_ its object (also known as the context). What this means is that when the callback is invoked inside the higher order function, `this` will refer to the `window` object, not the context.

Let's look at an example:

```js
const logger = {
  log(element) {
    console.log(this, element);
  }
}

logger.log(10); // =>  Object { log: function(element) {} } 10

[1,2,3].forEach(logger.log);
// Window { ... } 1
// Window { ... } 2
// Window { ... } 3
```

When we call `log` from the context of `logger`, `this` points to `logger`, however, when we pass `logger.log` as a callback function, it is called with no context, therefore `this` becomes the window.

This is quite confusing at first, but remember, if you are not sure what `this` is pointing to, you just need to log it.

## Further reading

- [What the Heck is a Callback](https://codeburst.io/javascript-what-the-heck-is-a-callback-aba4da2deced)
- [Callback Hell](http://callbackhell.com/)
- [Callbacks in JavaScript](https://zellwk.com/blog/callbacks/)
