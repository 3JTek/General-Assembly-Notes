# Data Types and Variables

All data in JavaScript is loosely divided into two type: _primitive_ and _composite_.

As the name implies, primitive data types are the simplest and include:

- [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
- [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
- [`Boolean`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

Composite data types include:

- [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [`Function`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function)
- [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
- [`RegExp`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

There are also a few special data types, including:

- [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null)
- [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)

In order to check which type of data you are working with, you can use the built-in `typeof` function:

```js
console.log(typeof 37) // => "number";
console.log(typeof {}) // => "object";
console.log(typeof function(){}) // => "function";
console.log(typeof undefined) // => "undefined";
```

Beware of a few gotchas:

```js
console.log(typeof []) // => "object";
console.log(typeof null) // => "object";
```

## Numbers

### Arithmetic

All standard arithmetic operators are supported including modulus:

```js
console.log(1 + 3); // => 4
console.log(1 - 3); // => -2
console.log(5 / 2); // => 2.5
console.log(6 * 2); // => 12
console.log(7 % 3); // => 1 (7 ÷ 3 = 2 remainder 1)
```

#### `Math`

The in-built [`Math`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math) object has more advanced functionality including square root, rounding, π, and a random generator:

```js
console.log(Math.sqrt(9)); // => 3
console.log(Math.round(9.5)); // => 10
console.log(Math.ceil(10.2)); // => 11
console.log(Math.floor(6.9)); // => 6
console.log(Math.PI); // => 3.141592653589793;
console.log(Math.random()); // => 0.7198309201452662;
```

### `NaN`

There is a special value [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN) which means "not a number". This will be returned when a math operation cannot be made successfully, for example when trying to multiply a number with a string. You can test for `NaN` with the `isNaN` function:

```js
console.log(7 * 'b'); // => NaN
console.log(isNaN(7 * 'b')); // => true
```

### `Infinity`

Another special value is [`Infinity`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Infinity). You can test for `Infinity` with `isFinite`:

```js
console.log(1 / 0); // => Infinity
console.log(-1 / 0); // => -Infinity
console.log(isFinite(1 / 0)); // => true
```

## Strings

Strings are sequences of characters. They are defined by wrapping text in either single or double quotes. Although either is valid, our linter setup requires that we use single quotes with JavaScript.

### String methods

All strings have certain in-build properties and functions (or _methods_), which allow us to access information about the string or modify it in some way. He's a look at some of the more common ones:

| **Property / Method** | **Behaviour** | **Syntax** |
|:----------------------|:--------------|:-----------|
| `length` | number of characters in the string | `'hello'.length`
| `replace` | replaces a substring with a string | `'hi there'.replace('hi', 'hello')`
| `charAt` | finds the character at the given index | `'javascript'.charAt(1)`
| `toUpperCase` | makes all the characters uppercase | `'stop shouting'.toUpperCase()`
| `toLowerCase` | makes all the characters lowercase | `'I CAN\'T HELP IT!'.toLowerCase()`

### Concatenation

When we join to strings together we _concatenate_ them. We can do this using either the `+` operator or backticks:

```js
const a = 'hello';
const b = 'world';
console.log(a + ' ' + b + '!'); // => hello world!
console.log(`${a} ${b}!`); // => hello world!
```

### Converting strings into numbers

We can also convert a string into a number with `parseInt` and `parseFloat`. `parseInt` takes a numeric string and converts it to an integer (a whole number). You should always provide a base as the second argument:

```js
console.log(parseInt('45', 10)); // => 45 (base 10 is decimal)
console.log(parseInt('1011', 2)); // => 11 (base 2 in binary)
console.log(parseInt('A4', 16)); // => 164 (base 16 is hexadecimal)
```

`parseFloat` converts a string into a float (or decimal number). It does not require a base:

```js
console.log(parseFloat('43.5627')); // => 43.5627
console.log(parseFloat('10')); // => 10
```

We can also use the unary operator (+):

```js
console.log(+'42'); // => 42
```

And finally the `Number` constructor:

```js
Number('101'); // => 101
```
### Concatenating strings and numbers

When we add a number to a string, or a string to a number, the result is always a string:

```js
console.log('10' + 10) // => "1010"
console.log(4 + 9 + '3') // => "133"
```

### Converting numbers into strings

There are a few different ways of converting a number into a string:

```js
console.log((56).toString()); // => "56"
console.log('' + 56); // => "56"
console.log(String(88)); // => "88"
```

## Boolean

One of the most fundamental data types of any programming language is the boolean. A boolean can be one of two values: true or false. All values in JavaScript can be converted into booleans. All of them become true, except for the following:

- `false`
- `0`
- `NaN`
- `''`
- `null`
- `undefined`

Because of this we can refer to any value as being either _truthy_ (it becomes true when converted to a boolean), or _falsy_ (it becomes false when converted to a boolean).

### Flipping a boolean

You can also flip a boolean using the `!` operator (known as a _bang_). Any truthy value becomes false when preceded by a bang, and vice versa:

```js
console.log(!false); // => true
console.log(!10); // => false
console.log(!''); // => true
console.log(!null); // => true
console.log(![]); // => false
```

### Converting a value to a boolean

The most common way of converting any value to a boolean is with a double bang (`!!`). A single bang will convert the value to a boolean, _and_ flip it, the second bang flips it back:

```js
console.log(!!false); // => false
console.log(!!10); // => true
console.log(!!''); // => false
console.log(!!null); // => false
console.log(!![]); // => true
```

## Variables

A variable holds data in memory for when you might need it later on. A variable can be defined in three ways in JavaScript, with the keywords `var`, `let` and `const`.

- **`var`**: this is now considered the weakest way of defining a variable and should generally no longer be used in JavaScript, although it is still valid.
- **`let`**: this should be used when you will need to re-assign the variable later. This is particularly useful in loops.
- **`const`**: this should be used when you do not need to re-assign the variable later. The variable will remain _constant_ throughout the script or code block.

If you are unsure which you should use, use `const`. If you need to modify the variable later on, change it to `let`.

### Block scope

Any code inside a curly brace is considered a _code block_. If we create a variable with `let` or `const` inside a code block it will not be available outside the code block, but variables outside the block will be available inside.

Let's look at an example:

```js
const a = 10;

if(a > 5) {
  // inside a code block
  const b = 20;

  // variables outside the code block are accessible inside
  console.log(a); // => 10

  const a = 20; // a new variable `a` has been created inside the block, which has nothing to do with the external `a`
}

// variables inside the code block are not accessible outside
console.log(a); // => 10
console.log(b); // => Uncaught ReferenceError: b is not defined
```

This concept is called scope. We will be looking at scope in more detail later with functions.

Important to note: if we use `var` it does not adhere to block scope:

```js
var a = 10;

if(a > 5) {
  var b = 20;
  console.log(a); // => 10
  var a = 20;
}
console.log(a); // => 20 (`a` has been overwritten inside the code block)
console.log(b); // => 20
```

## Further reading

- [The Secret Life of JavaScript Primitives](https://javascriptweblog.wordpress.com/2010/09/27/the-secret-life-of-javascript-primitives/)
- [JavaScript ES6+: var, let or const](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75)
