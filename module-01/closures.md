# Closures

When we create a function we create scope. Scope describes the variables that are available to the function when it is executed. The funny thing is that JavaScript functions remember their scope even after they are executed.

When this happens, we refer to it as a _closure_.

![](https://cloud.githubusercontent.com/assets/40461/8271947/1fc976be-1829-11e5-99c1-dd941e2507d6.jpg)

## Creating a closure

Let's take a look at a closure in action:

```js
function makeAdder(x) {
  return function (y) {
    return x + y;
  }
}

const add5 = makeAdder(5);
const add10 = makeAdder(10);

add5(23); // => 28
add10(23); // => 33
```

Here our `makeAdder` function returns an anonymous function. We call `makeAdder` passing an argument `x`. The anonymous function that is returned is stored into a variable, `add5` for example. When we call `add5` passing the argument `y`, the function remembers the variable `x` from it's parent function. In this way we have created a closure.

`add5` and `add10` do not only have their own scope, but they also have access to the parent scope, even though the scope was created when they were created. It's important to note that the variable `x` in the example above is different for `add5` and `add10`, because it was different when the parent scope was created.

## A silly example

There was a code test I saw once where you had to use a closure to make the following function work:

```js
sentence('This')('function')('should')('return')('a')('sentence.')();
// => "This function should return a sentence."
```

Its a funny one, and has no practical application, but it is another good demonstration of the use of a closure. Here's my solution:

```js
function sentence(initialWord) {
  return function addWord(nextWord) {
    if(!nextWord) return initialWord;
    initialWord += ' ' + nextWord;
    return addWord;
  }
}
```

The important thing here is that when you call `sentence` it returns a function, that way you can chain function invocations together. The next thing is to make sure that the user has supplied a value to the function. If not the `initialWord` is returned. If they have supplied a value, it is concatenated to the `initialWord` variable, which builds the sentence, and then the `addWord` function is returned ready to receive another word.

None of this would be possible if the `addWord` function did not remember what the state of `initialWord` was at the time it was called.

## A practical example

So what are the practical applications of a closure. The most common one is protecting variables from direct access.

Let's say we have a counter module. The counter module is very important to our application, and most importantly the counter should only be incremented or decremented by 1. There are a number of developers working on the application, and the counter is going to be used by multiple parts of the codebase.

Firstly let's look at how one might go about making the module:

```js
const counter = {
  value: 0,
  increment() {
    return this.value += 1;
  },
  decrement() {
    return this.value -= 1;
  }
};
```

Not a bad first effort, but the value can be set to anything like so: `counter.value = 100`. This is not ideal. A developer working on another part of the codebase could easily overwrite the value of the counter without realising the implications of their actions.

Instead of making the variable publicly available, we can use a closure to hide the variable:

```js
function counterModule() {
  let value = 0;

  return {
    increment() {
      return value += 1;
    },
    decrement() {
      return value -= 1;
    }
    getValue() {
      return value;
    }
  }
}

const counter = counterModule();
```

Now the value of the counter cannot be accessed by any other part of the program. It can only be modified through the use of the methods `.increment()`, `.decrement()`. Since the value of the counter is no longer directly available it has to be retrieved using the `.getValue()` method.

## Skipping a step with an IIFE

An `IIFE` stands for an _Immediately Invoked Function Expression_. If we want to create a closure, but only need to do it once, we can call our function immediately and store the return into a variable:

```js
const counter = (function() {
  let value = 0;

  return {
    increment() {
      return value += 1;
    },
    decrement() {
      return value -= 1;
    },
    getValue() {
      return value;
    }
  }
}());
```

This is identical to the previous example, but means the counter function does not have to be named, and cannot be used again.

## Further reading

- [Getting Closure](http://markdaggett.com/blog/2013/02/25/getting-closure/)
- [Understanding JavaScript Closures with Ease](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
- [I Love My IIFE](http://gregfranko.com/blog/i-love-my-iife/)
