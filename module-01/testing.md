# Testing

Testing is the process of making sure your code does what it's supposed to.

## Manual testing

Manual testing, or error-driven development, is just what it sounds like: checking all the code works as expected after you change any source code, including testing your application from your web interface. This is limited by the time you need to test *everything* whenever you change *anything*. The larger the code base gets, the harder it is to check every line and every page every time a change is made.

## Automated testing

Automated testing is achieved by writing code that checks your code. This may involve writing some code that plays through scenarios that address various possible input values and the expected outcomes.

When you write very small tests that check very small sections of classes or models, we call that _unit testing_.

As your code base grows, so does you test coverage. You should get to a situation where you can run your test code at any time, and every single line of your code gets passed through to ensure it's still returning as expected when it was first written.

## TDD

TDD stands for _test-driven development_, also known as red/green development. With TDD, you write the tests first, before writing any code and then write code that makes the test pass.

The test will initially fail - that's the point of the 'red' - and the expectations of the test will drive how you will write your actual code - this is referred to as your implementation - until the test passes, or goes 'green'.

Frequently, TDD is approached with pair programming - two developers working together at one machine. Often, one person writes a test; then, the other writes the implementation, and they alternate throughout the day. In an interview, you might be given some test code and be asked to write the implementation code; or you might be asked to write the tests for some outline functionality to demonstrate your familiarity with this process.

The process is also referred to as _red/green/refactor_. Once the test passes you can review the code you've written and any other parts of the code that's affected to see if it can be refactored at all. No new functionality is added at this stage - the desired outcome is still for the tests to pass, just as they had before, but with more efficient code.


![red-green-refactor](https://media.git.generalassemb.ly/user/15120/files/8ef6cc80-1285-11e9-8b8e-09124d358eab)

## Testing at the end?

Often developers do not write their code TDD, this might be because there isn't time or there isn't enough budget. Developers often "intend" to write tests but they don't don't get around to it.

It's always to write tests as you go. More and more employers look for a good understanding of testing in junior developers.

## Writing tests

There are lots of different tools out there for writing tests, on this course we'll be using two which are very popular in the JavaScript community: Mocha and Chai.

### Mocha

Mocha is a test framework. It is used to run structure and run our tests. It gives us the following helper methods:

* `describe`: Defines a collection of tests
* `it`: Defines a single test
* `before`: A function to run before the tests
* `beforeEach`: A function to run before each test
* `after`: A function to run after the tests
* `afterEach`: A function to run after each test

Mocha will also display the test results, usually red crosses or green ticks, either in the terminal or in browser depending on the setup.

### Chai

Chai is an _assertion library_, it provides helper methods to determine whether a given value is what we expect.

Here are some examples:

```js
const num = 20
expect(num).to.equal(10) // false - test has failed

const arr = ['Elephant', 'Giraffe', 'Helicopter']
expect(arr).to.include('Helicopter') // true - test has passed
expect(arr).to.be.empty // false
expect(arr).to.not.be.empty // true

const car = { brand: 'Ford', model: 'Focus', color: 'blue' }
expect(car.band).to.exist // true
expect(car.model).to.be.null // false
expect(car.speed).to.be.undefined // true
```

As you can see we can use Chai to _assert_ or check that a given variable, property or function return is what we expect it to be. In essence this is how automated testing works.

## Setup

We'll setup Mocha and Chai for use in-browser, using a CDN. The folder will look like this:

```
├── index.html
├── app.js
└── test
    ├── index.html
    └── tests.js
```

The `test` folder will store all of our tests, and an index.html file used to display the test results.

#### HTML file

The `test/index.html` file should look like this:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Mocha Tests</title>
  <link href="https://cdn.rawgit.com/mochajs/mocha/2.2.5/mocha.css" rel="stylesheet" />
  <script src="https://cdn.rawgit.com/chaijs/chai/3.5.0/chai.js"></script>
  <script src="https://cdn.rawgit.com/mochajs/mocha/2.2.5/mocha.js"></script>
</head>
<body>
  <div id="mocha"></div>

  <script src="../app.js"></script>
  <script>
    mocha.setup('bdd')
  </script>
  <script src="tests.js"></script>
  <script>
    mocha.checkLeaks()
    mocha.run()
  </script>
</body>
</html>
```

Here we are loading in Mocha and Chai using their respective CDNs. The `div` with the id of `mocha` is used to display the test results.

After that we include the code that we want to test. This is our main `app.js` file.

The we are running mocha's `setup` method, which initialises the testing framework.

After that we include the actual automated test that we will write.

Finally we call `mocha.checkLeaks()` and `mocha.run()` which actually runs our tests, and outputs the results to the DOM.

## Writing tests

We'll look at how to write a function which converts feet and inches to centimetres, using TDD. **Before attempting to solve the problem** we first write a test, in `test/tests.js`:

```js
/* global describe, it */
/* eslint-disable no-undef */

const expect = chai.expect

describe('Feet and Inches to CM tests', () => {
  it('should return a number', done => {
    expect(ftAndInchesToCm(5, 10)).to.be.a('number')
    done()
  })
})
```

This is a typical test. It is made up of four parts:

* `describe`: A method which takes a string which will be output by mocha. It is simply a description of the tests that are about to be run. It is useful for grouping tests together.
* `it`: Similar to describe, except this describes a single test. `it` generally begins a sentence: "it should be red", "it should be an object", etc.
* `expect`: This is the actual test. Basically we are saying "I expect A to be B".
* `done`: The `done` method tells mocha that this specific test is over, and it's time to move on to the next test. This is particularly useful if we need to test something _asynchronously_, eg a HTTP request or anything that takes a good amount of time.

In the test above we are saying:

"I expect that `ftAndInchesToCm(5, 10)` should return a number"

To run the test we just need to open the `tests/index.html` with Chrome.

We should see the following output:

![failing test](https://media.git.generalassemb.ly/user/15120/files/e6e50180-128c-11e9-872b-2efac77a54af)

Our test is failing. This is good! Our test has told us that `ftAndInchesToCm` is not defined.

Now we can try to make our test pass. **We should write just enough code to make our test pass**. Let's start by defining our `ftAndInchesToCm` function in the `app.js` file:

```js
function ftAndInchesToCm() {
  return 0
}
```

If we refresh the browser we should see that the test has passed.

Great! Let's write another:

```js
describe('Feet and Inches to CM tests', () => {
  it('should return a number', done => {
    expect(ftAndInchesToCm(5, 10)).to.be.a('number')
    done()
  })

  it('should return the correct value', done => {
    expect(ftAndInchesToCm(5, 10)).to.eq(178)
  })
})
```

Again we first need to check that the test fails, then we can write **just enough code to make the test pass**. That might look something like this:

```js
function ftAndInchesToCm() {
  return 178
}
```

Ok, that _does_ pass the test, but it doesn't really solve the problem. However, that's ok. It's important to not _overengineer_ a solution. Perhaps this function only needs to convert 5' 10". If the test has passed, the job is done. If we need our function to handle different situations, we need to write more tests.

Let's add a couple more:

```js
describe('Feet and Inches to CM tests', () => {
  it('should return a number', done => {
    expect(ftAndInchesToCm(5, 10)).to.be.a('number')
    done()
  })

  it('should return the correct value', done => {
    expect(ftAndInchesToCm(5, 10)).to.eq(178)
    expect(ftAndInchesToCm(4, 8)).to.eq(142)
    expect(ftAndInchesToCm(6, 2)).to.eq(188)
  })
})
```

Ok, with tests in place, we can update our function:

```js
function ftAndInchesToCm(feet, inches) {
  return Math.round((feet * 12 + inches) * 2.54)
}
```

And we're done. Not only have we written a function that converts feet and inches to centimetres, but we have a test suite that checks that it works correctly. If we want to refactor it now, we can be confident that it still works as expected if is still passes all the tests when we've finished.

## Summary

Testing can take a little while to get used to, and it does take a little time to understand _what_ to test, and then _how_ to test it.

Remember we are really just mimicking manual testing with code. If you are not sure what you should be testing, think about how you would test it manually. Maybe a console log? What would you expect the console log to display if your code works? Now write that as an automated test.

## Further reading

* [Mocha Documentation](https://mochajs.org/)
* [`expect` - Chai Documentation](https://www.chaijs.com/guide/styles/#expect)
* [Unit Test Your JavaScript Using Mocha and Chai - SitePoint](https://www.sitepoint.com/unit-test-javascript-mocha-chai/)
* [The Absolute Beginner’s Guide to Test Driven Development, with a Practical Example - Medium](https://medium.com/@bethqiang/the-absolute-beginners-guide-to-test-driven-development-with-a-practical-example-c39e73a11631)
