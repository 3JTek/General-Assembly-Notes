# Scope

All variables exist in a certain scope. When we create a variable at the top of a script, it is created in _global_ scope. If we use a `let` or `const` in a code block, it exists in _block_ scope, and if we create a variable in a function, it exists in _functional_ scope.

Variables inside scope are not accessible in the parent's scope, however variables in the parent scope _are_ accessible in the child scope. You can think of scope like a room with two-way glass, you can see out of the room (to the parent's scope), but you can't see in it.

### Block scope

Any code inside curly braces is considered a block. If we declare a variable with `const` or `let` it will only be available inside the block. `var` on the other hand does not recognise block scope:

```js
const a = 10
if(a > 5) {
  let b = 20
  const c = 30
  var d = 40
  console.log(a) // => 10 (variables in the parent scope are accessible)
}

console.log(a) // => 10
console.log(b) // => Uncaught ReferenceError: b is not defined
console.log(c) // => Uncaught ReferenceError: c is not defined
console.log(d) // => 40
```

### Functional scope

All variables declared inside a function adhere to functional scope, and are only available inside the function:

```js
const a = 10

function foo() {
  let b = 20
  const c = 30
  var d = 40
  console.log(a) // => 10
}

foo()

console.log(a) // => 10
console.log(b) // => Uncaught ReferenceError: b is not defined
console.log(c) // => Uncaught ReferenceError: c is not defined
console.log(d) // => Uncaught ReferenceError: d is not defined
```

### Scope inside scope

It is possible to create scope inside scope. You can make functions inside functions and block scope anywhere:

```js
const foo = () => {
  const a = 10

  const bar = function() {
    let b = 20

    function bat() {
      var c = 30

      if(a > 5) {
        let d = 40
        console.log(c) // => 30 (variables in the parent scope are accessible)
      }

      console.log(d) // => Uncaught ReferenceError: d is not defined (inside if statement's block scope)
    }

    bat()

    console.log(c) // => Uncaught ReferenceError: c is not defined (inside bat's functional scope)
  }

  bar()

  console.log(b) // => Uncaught ReferenceError: b is not defined (inside bar's functional scope)
}

foo()

console.log(a) // => Uncaught ReferenceError: a is not defined (inside foo's functional scope)
console.log(d) // => Uncaught ReferenceError: d is not defined (you get the idea...)
```

### Why does this happen?

Well, it's all down to how JavaScript interprets the code that we write. When we write JavaScript, at some point it needs to be processed by our computer's CPU (Central Processing Unit). The CPU does not understand JavaScript source code, it needs _machine code_ (think 0s and 1s), so when we run our JavaScript file (the source code), it first needs to be compiled (converted into a different language).

Compiled languages need to be manually compiled before they are run (C and C++ are examples of this), but other languages, known as _interpreted languages_ are compiled automatically at runtime. JavaScript, Ruby, Java and PHP are all interpreted languages.

#### Compilation

Compilation requires three main steps:

1. **Lexical analysis**: The source code is scanned, one character at a time looking for language constructs like variables, functions, loops etc. If there are syntax errors they will be detected here.
1. **Scope created**: The compiler defines scope and ascertains which variables are accessible to which part of the program, making the code as efficient as possible.
1. **Machine code created**: The final stage is the creation of the machine code in memory.

Once the complier is finished the machine code is run automatically. The whole thing takes less than a second.

## Further reading

* [Understanding Scope in JavaScript](https://scotch.io/tutorials/understanding-scope-in-javascript)
* [Demystifying JavaScript Variable Scope and Hoisting](https://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/)
* [Understanding JavaScript Scope](https://hackernoon.com/understanding-javascript-scope-1d4a74adcdf5)
