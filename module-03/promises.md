# Promises

JavaScript is an asynchronous language, which means that it often performs tasks in parallel. To do this it makes heavy use of callback functions, which can make our code difficult to understand and manage.

## Callback hell

Let's look at an example where we need to make several database calls, all of which need to provide callbacks:

```js
Book.find((err, books) => { // request the books
  if(err) console.log(err); // handle any errors

  console.log(`${books.length} books found`);

  Films.find((err, films) => { // request the films
    if(err) console.log(err); // handle any errors

    console.log(`${toys.length} toys found`);

    Users.find((err, users) => { // request the users
      if(err) console.log(err); // handle any errors

      console.log(`${users.length} users found`);
    });
  });
});
```

As you can see the code can quickly get out of hand and makes difficult reading, plus we have all these closing curly braces and parentheses at the end.

To help manage all these callbacks, we can use promises instead.

A promise is a special function that creates some helper methods that allows us to manage the order in which asynchronous behaviour.

| **Method** | **Purpose** |
| --- | --- |
| `.then()` | Action to be performed once the async function is completed |
| `.catch()` | Action to be performed if the async function fails |
| `.finally()` | Action to be performed either way |

## Setting up Mongoose to use promises

In order to get Mongoose to use promises we need to provide it a promise library. NodeJS comes with one built in, but at GA we prefer Bluebird, which comes with a few added features. We can connect it to Mongoose in our `index.js` file:

```js
const mongoose = require('mongoose');
const Promise = require('bluebird');

mongoose.Promise = Promise;
```

Now mongoose will return a promise when we make a database request. To get the promise we need to run `.exec()` at the end of the method:

```js
Books
  .find()
  .exec() // returns a promise
```

## Using promises

Let's look at how we can get the data from the database in different ways using promises:

### Series

Firstly let's look at running each database lookup one after the other:

```js
Book
  .find()
  .exec() // request the books
  .then(books => {
    console.log(`${books.length} books found`);
    return Film
      .find()
      .exec(); // request the films
  })
  .then(films => {
    console.log(`${films.length} films found`);
    return Users
      .find()
      .exec(); // request the users
  })
  .then(users => {
    console.log(`${users.length} users found`);
  })
  .catch(err => console.log(err)); // handle any errors
```

Here we have created a _promise chain_. The requests will be made one at a time. If any of the database lookups throw errors, then the `catch` block will immediately be called and log the error.

This is nice because we can DRY up our code. We only have to handle our errors once.

### Parallel

We can also run promises all at the same time, and when they have all returned we can handle the responses together. Bluebird has two methods for this: `.all()` and `.props()`:

#### `.all()`

If we want to handle arrays of promises, we can use `.all()`:

```js
const Promise = require('bluebird');

const promiseArray = [ // create an array of promises
  Book.find().exec(),
  Film.find().exec(),
  User.find().exec();
];

Promise.all(promiseArray)
  .then(data => console.log(data)) // return an array of data. The array will be the same length as promisesArray
  .catch(err => console.log(err)); // catch any errors
```

#### `.props()`

We can also attach our promises to an object and handle them with `.props()`:

```js
const Promise = require('bluebird');

const promiseObject = { // create an object of promises
  books: Book.find().exec(),
  films: Film.find().exec(),
  users: User.find().exec()
};

Promise.props(promiseObject)
  .then(data => console.log(data)) // return an object containing the data.
  .catch(err => console.log(err)); // catch any errors
```

## Creating a promise

So for we have looked at using promises, but what about creating one? Again we can use bluebird to convert a function that usually takes a callback into a promise like so:

```js
const Promise = require('bluebird');

// create a new promise
const promise = new Promise((resolve, reject) => {
  Films.find((err, films) => {
    if(err) return reject(err); // call `.catch` passing in the error
    return resolve(films); // call `.then` passing in the films
  });
});

promise
  .then(films => console.log(films))
  .catch(err => console.log(err));
```

When we create a promise it gives us two functions `resolve` and `reject`. Inside the promise we can run our async function callback as usual. Any errors can be sent into the promise's `.catch` block by calling `reject`, and any data that we want to send in to the `.next` block can be done so with `resolve`.

## Further reading

* [Callback Hell](http://callbackhell.com/)
* [ES6 Promises](http://www.datchley.name/es6-promises/)
* [Bluebird Cheatsheet](https://devhints.io/bluebird)



