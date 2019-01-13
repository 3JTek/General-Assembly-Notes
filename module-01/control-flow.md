# Control Flow

A computer program is basically an algorithm (a sequence of instructions that complete a task). That sequence is defined by the _flow_ of the program. We can define the flow of a program using control flow constructs like conditions and loops.

You can express the flow of a program with a flow chart:

![](https://user-images.githubusercontent.com/3531085/35481242-c88007f0-0417-11e8-9fd8-9faddd7f8636.png)

This flow chart represents a simple game where the user has to guess what number the computer has chosen. Each diamond represents a decision to be made. The decision affects the _flow_ of the program.

## Conditionals

Conditionals take the form `if`, `else if` and `else` and can be used to make binary decisions. The syntax looks like this:

```js
if(condition) {
  // do something if condition is met
} else if(condition) {
  // do something else if condition is met
} else {
  // do something else
}
```

A condition is a small equation which results in a boolean, some examples might be:

```js
// if the value of `a` is less than 10
if(a < 10) {
  // do something
}

// if the width divides exactly into 10
if(width % 10 === 0) {
  // do something
}

// if a is truthy
if(a) {
  // do something
}

// if a is falsy
if(!a) {
  // do something
}
```

### Ternary operator

A decision can be made when setting a variable in one line using a _ternary operator_:

```js
const level = xp > 100 ? 'ninja' : 'novice';
```

This would be the equivalent of:

```js
let level;
if(xp > 100) {
  level = 'ninja';
} else {
  level = 'novice';
}
```

### Comparison operators

JavaScript uses standard comparison operators to compare values. A comparison operator always returns a boolean.

| Operator | Meaning |
|----------|---------|
| `>` | greater than |
| `<` | less than |
| `>=` | greater than or equal to |
| `<=` | less than or equal to |
| `==` | equal to |
| `!=` | not equal to |
| `===` | strictly equal to |
| `!==` | not strictly equal to |

In JavaScript there are two equality operators: `==` and `===`. The `==` will check whether the two value are the same **when converted into the same type**. Whereas the `===` will check that the two value are the same and **are the same type**.

```js
console.log(0 == false); // true - when both values are converted to booleans, they are the same.
console.log(0 === false); // false - 0 is a number, false is a boolean.
```

**We should avoid the use of `==`, as it can lead to unexpected results.**

## Boolean logic

### AND

The AND operator: `&&`, requires both values to be `true` to return `true`:

```js
console.log(false && false); // => false;
console.log(false && true); // => false;
console.log(true && false); // => false;
console.log(true && true); // => true;
```

### OR

The OR operator: `||`, requires either value to be `true` to return `true`:

```js
console.log(false || false); // => false;
console.log(false || true); // => true;
console.log(true || false); // => true;
console.log(true || true); // => true;
```

**Boolean operators are lazy.** With `&&`, if the first value is _falsy_, the second value will not be checked, since the result can only be `false`. Similarly, with `||`, if the first value is _truthy_, the second value will not be checked, since the result can only be `true`.

This can allow for some interesting design patterns:

```js
name && console.log('Name has been defined'); // if name is truthy, log "Name has been defined"
let name = name || 'John Doe'; // if name has not already been defined, it will be defined as 'John Doe'
```

## Switch statements

Another way of writing a conditional is with a switch statement:

```js
const a = 4;

switch(a) {
  case 3:
    console.log('close');
    break;
  case 2:
    console.log('warmer');
    break;
  case 1:
    console.log('colder');
    break;
  default:
    console.log('way off');
}
```

This is the equivalent of:

```js
const a = 4;
if(a === 3) {
  console.log('close');
} else if(a === 2) {
  console.log('warmer');
} else if(a === 1) {
  console.log('colder');
} else {
  console.log('way off');
}
```

The switch statement compares the value in parentheses with the value in each `case` block with a strict equality operator (`===`). If the result is `true` that case block is run. The `break` keyword, stops the next case block from being run also.

## Loops

Another way of changing the flow of a program is by looping over the same piece of logic until a condition is met:

### `while`

A `while` loop checks for a condition at the end of each loop. If the condition is _truthy_, it will run the code block again:

```js
let a = 10;
while(a > 5) {
  console.log(`a is now ${a}`);
  a--;
}
// => a is now 10
// => a is now 9
// => a is now 8
// => a is now 7
// => a is now 6
```

### `for`

A for loop is similar, but the syntax is different:

```js
for(let a = 10; a > 5; a--) {
  console.log(`a is now ${a}`);
}
// => a is now 10
// => a is now 9
// => a is now 8
// => a is now 7
// => a is now 6
```

The `for` loop is more self contained. The initial variables, condition and alteration is done created inside the parentheses:

```
for(initial variables; condition; logic to be performed at the end of each loop) {
  // code to be run on every loop
}
```

While both `while` and `for` can be used to perform the same goal, generally speaking, the `while` loop is more flexible, whereas the `for` loop is more specific to looping over arrays or looping a specific number of times.

## Further reading

* [Conditionals](https://www.javascript.com/learn/javascript/conditionals)
* [Ternary Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
* [Comparison Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)
* [The && and || Operator in JavaScript](https://blog.mariusschulz.com/2016/05/25/the-andand-and-operator-in-javascript)
* [How to Use the Switch Statement in JavaScript](https://www.digitalocean.com/community/tutorials/how-to-use-the-switch-statement-in-javascript)
* [Loops and Iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)
