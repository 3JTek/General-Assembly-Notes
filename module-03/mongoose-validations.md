# Mongoose Validations

Mongoose will validate the data that we use to create a record for us. All we have to do is add the validation to the model:

```js
const mongoose = require('mongoose');

const schema = new mongoose.Schema({
  name: { type: String, required: true, unique: true },
  origin: { type: String, required: true, minlength: 2 },
  strength: { type: Number, required: true, min: 1, max: 5 }
});

module.exports = mongoose.model('Cheese', schema);
```

In the example above we've added some validations:

- `unique`: No two records can have the same value
- `minlength`: The string must be at least this length
- `min`: The number must be at least this value
- `max`: The number cannot exceed this value

There are other useful ones, including `enum` which means that a value must be one of the options provided.

Once the validations have been added to the model, if we try to save some data, and the validations fail we will get an error:

```js
Cheese.create({ name: 'test', origin: 'a', strength: 64 }, (err, cheese) => {
  if(err) console.log(err); // this will log a validation error
  console.log(cheese); // cheese will be null
});
```

## Handling errors

Once a validations have been added, it's important that we handle any form errors. This should be done on both the server side _and_ the client side, with the server side being the most important.

### Server side

The best way to handle errors on the server side is with a global error handler.

An error handler is just like a request handler, except it takes four arguments, and should be the last thing before the `app.listen` statement:

```js
// error handler
app.use((err, req, res, next) => {
  // handle the errors

});

app.listen(port, () => console.log(`Up and running on port ${port}`));
```

The four arguments are:

- `err`: The error to be handled
- `req`: The request object
- `res`: The response object
- `next`: A callback function to be called if we wish to jump out of the error handler

A simple error handler might look like this:

```js
// error handler
app.use((err, req, res, next) => {
  // handle the errors
  if(err.name === 'ValidationError') return res.status(422).json({ error: err });

  return res.status(500).json({ message: 'Internal Server Error' }));
});
```

In this example we are checking if there is a validation error, and if so we send a 422 (Unprocessble Entity) response and an error page, which could display the errors to the user.

Otherwise we send a more generic 500 (Internal Server Error) response with a page which could display the error to the user, or perhaps a cat running into a screen door or something.

In order to ensure that our errors reach the error handler, we need to pass `next` into our controller functions and pass it into the `.catch` block of our promise chain:

```js
function createRoute(req, res, next) {
  Cheese.create(req.body)
    .then(cheese => res.json(cheese))
    .catch(next); // error is passed on to the error handler
}
```

## Further reading

- [Mongoose Validations](http://fiznool.com/blog/2014/04/23/mongoose-validations/)
