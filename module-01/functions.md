# Functions

One of the most important parts of any programming language is the ability to separate logic into smaller chunks, and to be able to reuse those chucks.

The a very important principal when writing code to not repeat yourself. In fact code can be described as **DRY** \(Don't Repeat Yourself\) or the opposite **WET** \(We Enjoy Typing, or Write Everything Twice\).

Writing the same logic in multiple places is not only a waste of time but also leads to bugs later on.

Imagine you are writing some functionality to convert pounds and stone into kilograms and vice verser. You need to use it in multiple places in your code. After a few weeks you have the same logic in 18 different files, but you realise that the logic is flawed, your maths is slightly out. You now have to go back through 100s of lines of code in 18 different files to make sure you correct the flaw. Chances are you miss one or two. In a few more weeks you may have slight differences in all 18 instances of that conversion logic. Now you application is full of bugs and the code has gotten out of hand.

> **Note**: True story. I still have nightmares...

Functions are a convenient way to hold small packets of logic that can be reused throughout a codebase. If the conversion logic from the example above was in a function, if the logic needed to be changed, it could be changed within the function, and that change would be reflected instantly throughout the entire code base.

Using functions makes our code simpler, easier to read and more maintainable.

## Writing functions

Functions first need to be declared, then called or _invoked_. The function declaration describes what the function should do, but it does not actually perform that logic until it is called. This makes functions **reusable**.

### Function declaration

![](https://user-images.githubusercontent.com/3531085/35194737-10b8670a-feb0-11e7-9ab5-25a0eb4edd01.png)

Functions have three main parts. The name, in this case `add`, arguments, which allows data to be passed into the function, and a return value, which is what will be given back from the function when it is invoked.

### Calling a function

The function above has been declared, which means it has been loaded into memory, and is ready to be used. In order to actually use it, we need to call it, or invoke it. To do that we need to state the name and parentheses to indicate we want to run the logic:

```js
add(2, 6) // => 8
```

In the example above `2` and `6` are the arguments, and `8` is the return value.

## Different ways of declaring a function

Functions can be declared in several different ways in JavaScript.

### Named function

```js
function add(a, b) {
  return a + b;
}
add(10, 8); // => 18
```

### Anonymous function stored in a variable

```js
const add = function(a, b) {
  return a + b;
};
add(4, 5); // => 9
```

### Arrow function stored in a variable

```js
const add = (a, b) => {
  return a + b;
};
add(); // => "Hello"
```

All the above function declarations are essentially the same, they will all run the same logic and can be called in the same way.  
The differences between them are very subtle, so for now, just think of them as different ways of writing the same thing.

## Arguments

Arguments are variables that only exist within a function, but can be set _when the function is called_. Using the weight conversion example we can write the following function:

```js
function stonesAndPoundsToKilograms(stones, pounds) {
  const totalStones = stones * 14 + pounds;
  const totalKilograms = totalStones * 0.453592;
  return totalKilograms;
}
```

By using arguments we can convert any weight to kilograms.

```js
stonesAndPoundsToKilograms(11, 7); // => 73.02837
stonesAndPoundsToKilograms(15, 0); // => 95.2544
```

## Gotchas

A couple of things to be aware of when writing functions:

### `return`

If you don't use the `return` keyword at in the function it will return `undefined`.

```js
function add(a, b) {
  a + b;
}
add(2, 8); // => undefined
```

What has happened here is that when the `add` function is called, the two numbers are added together, but the result is not returned from the function. Therefore we get `undefined` instead.

### Passing arguments

If you omit an argument when you call a function it will become `undefined`, which can cause unexpected results.

```js
function add(a, b) {
  return a + b;
}
add(3); // => NaN
```

This is because `3 + undefined` is `NaN`.

#### Default Arguments

We can prevent thing from happening by using default arguments:

```js
function add(a=0, b=0) {
  return a + b;
}
add(10); // => 10
```

Now if an argument is omitted a 0 is used in its place.

## Further reading

* [How To Define Functions in JavaScript](https://www.digitalocean.com/community/tutorials/how-to-define-functions-in-javascript)
* [What Are Pure Functions And Why Use Them](https://medium.com/@jamesjefferyuk/javascript-what-are-pure-functions-4d4d5392d49c)
* [JavaScript Arrow Function Introduction](http://wesbos.com/arrow-functions/)
