# Testing an Express API

Automated testing is essential when creating complex applications. On top of manual testing, ie, testing endpoints in Insomnia, we should also write automated tests for our APIs. This way we can very quickly ascertain that our API is working correctly, and that new code that we write has not caused unwanted side-effects to other areas of the codebase.

## Setup

### Database

When testing APIs we are going to make HTTP requests, and then test that the responses are what we expect. To do this, we will need to add and delete data from our database. However, we don't want to delete stuff from our development database. Also, we don't want to have existing data in our database that may affect the tests that we are writing.

For these reasons, we need to create a separate database for testing purposes.

In your `config/environment.js` file, add the following:

```js
const env = process.env.NODE_ENV || 'development'
const dbURI = env === 'production' ? process.env.MONGODB_URI : `${process.env.MONGODB_URI}-${env}`

module.exports = { env, dbURI }
```

> **Note:** The `NODE_ENV` variable is set to `production` on Heroku. We can set it to `test` when testing our app. It will default to `development` otherwise.

In this way we will have two databases, one ending `-development` for developing, and one ending `-test` when testing. Any testing that adds or removes data from the database will no longer affect our development database.

### Exporting our app

With that done we need to export our `app` from `index.js` so that we can import it into our test suite:

```js
app.listen(process.env.PORT, () => console.log(`Running on port ${process.env.PORT}`))

module.exports = app
```


### Creating a helper file

We need to load in a number of packages for our tests to load. Rather than adding them to the top of each test file, we'll create a helper file which will be automatically loaded into every test file.

We'll be using a few packages:

* Mocha - a test runner, it gives us `describe` and `it` methods amongst others
* Chai - an assertion library, it gives us the `expect` method
* Supertest - used to test API requests

Install them with yarn:

```sh
yarn add moch chai supertest --dev
```

All of our tests will be placed in a `test` folder. Name this file `test/spec_helper.js`

```js
process.env.NODE_ENV = 'test'
global.Promise = require('bluebird')

const chai = require('chai')
global.expect = chai.expect

const supertest = require('supertest')
const app = require('../index')
global.api = supertest(app)
```

Here we set the `NODE_ENV` variable to be `test`, so that the API will connect to our test database. We add `Promise`, `should`, `expect` and `api` to the `global` object (like `window`) in the browser, so that they are globally available.

We are importing our app from `index.js` and then using Supertest to create API endpoints that are testable.


### Add a `test` script

In `package.json` we need to add a `test` script that will run our tests:

```json
"scripts": {
  "start": "node index",
  "test": "mocha test/**/*_spec.js --require test/spec_helper --recursive --exit"
}
```

This script is going to use `mocha` to run any file in the test folder that ends `_spec.js`, requiring our `spec_helper.js` file so that the global variables are created. `--recursive` means that mocha will search all folders for `_spec.js` files no matter how deeply nested they are, and `--exit` will stop once the tests have run (rather than waiting for file changes).

## Writing tests

With all that set up, we can now start writing tests. But what should we test?

Let's imagine we want to test an _INDEX_ route using Insomnia. What would we check to make sure that our route is as we expect?

* We would expect the status code should be 200
* We would expect an array
* We would expect the array to contain objects
* We would expect the objects to contain the correct data

This is what we call a test suite. Now that we have our test suite, we can write some code to check these things for us:

We'll create a file for each route, so let's make a `test/cheeses/index_spec.js` file:

```js
/* global api, describe, it, beforeEach, after */

describe('GET /cheeses', () => {
  it('should return a 200 response', (done) => {
    api.get('/cheeses')
      .end((err, res) => {
        expect(res.status).to.eq(200)
        done()
      })
  })
})
```

This is our first test, we expect the status code to be 200. If it is, we should get a green tick, if not we get a red cross.

We can now run the tests with `yarn run test`.

With the next test we want to check for an array in the body of the response:

```js
/* global api, describe, it, before, after */

describe('GET /cheeses', () => {

  ...

  it('should return an array', (done) => {
    api.get('/cheeses')
      .end((err, res) => {
        expect(res.body).to.be.an('array')
        done()
      })
  })
})
```

Again, run the tests with `yarn run test`. If the test passes you should get another green tick.

The next test expects the array to contain some objects, but since we are using our test database it will be empty. We want to populate our database before we run our test suite, then empty it afterwards, so the data does not affect any other tests.

Mocha has `before` and `after` hooks for this very purpose:

```js
/* global api, describe, it, before, after */

describe('GET /cheeses', () => {

  before(done => {
    Cheese.create({
      name: 'Cheddar',
      origin: 'England',
      image: '/images/cheddar.jpg',
      tastingNotes: 'Sharp and tangy'
    })
      .then(() => done())
  })

  after(done => {
    Cheese.remove({})
      .then(() =>  done())
  })
  ...

  it('should return an array of objects', (done) => {
    api.get('/cheeses')
      .end((err, res) => {
        expect(res.body[0]).to.be.an('object')
        done()
      })
  })
})
```

Finally we can test that the data is as expected:

```js
describe('GET /cheeses', () => {
  ...

  it('should return the correct data', (done) => {
    api.get('/cheeses')
      .end((err, res) => {
        expect(res.body[0]._id).to.exist
        expect(res.body[0].name).to.eq('Cheddar')
        expect(res.body[0].origin).to.eq('England')
        expect(res.body[0].image).to.eq('/images/cheddar.jpg')
        expect(res.body[0].tastingNotes).to.eq('Sharp and tangy')
        done()
      })
  })
})
```


## Testing other endpoints

By following the principals outlined above you should be able to test all other endpoints for your API. Remember you are writing tests that would mimic a manual test of the endpoint.

Some tips to follow:

* No test should rely on another test
* No test should rely on data already in the database
* Any data created by a test should be removed at the end of the test
* Isolate tests as much as possible. If you need a token to test an endpoint for example, make the token with `jsonwebtoken` rather than using the LOGIN route, this way if the LOGIN route is broken, it won't break other tests.
