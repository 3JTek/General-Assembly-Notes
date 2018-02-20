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

app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
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
  if(err.name === 'ValidationError') return res.status(422).render('pages/422', { err });

  return res.status(500).render('pages/500', { err });
});
```

In this example we are checking if there is a validation error, and if so we send a 422 (Unprocessble Entity) response and an error page, which could display the errors to the user.

Otherwise we send a more generic 500 (Internal Server Error) response with a page which could display the error to the user, or perhaps a cat running into a screen door or something.

In order to ensure that our errors reach the error handler, we need to pass `next` into our controller functions and pass it into the `.catch` block of our promise chain:

```js
function createRoute(req, res, next) {
  Cheese.create(req.body)
    .then(() => res.redirect('/cheeses'))
    .catch(next); // error is passed on to the error handler
}
```

## Client side

Once the errors have been handled on the server side, its better to not allow the form to even be submitted if we know the valdations will fail. To do that we can use HTML5 form validation attributes:

```html
<form method="POST" action="/cheeses">
  <input type="text" name="name" placeholder="Name" required />
  <input type="text" name="origin" placeholder="Origin" minlength="2" required />
  <input type="number" name="strength" placeholder="Strength" min="1" max="5" required />

  <button>Submit</button>
</form>
```

The syntax is almost identical to the mongoose validations, but now if we try to submit the form with data that does not meet the validations, not only will the browser prevent the form from submitting, but will also display error messages to the user:

![](https://user-images.githubusercontent.com/3531085/36447868-999d4db2-167d-11e8-98e5-ddd7b0516bc6.png)

## jQuery Validation Plugin

Adding client side validations is good because it instantly informs the user as to what has happened, and does not bother the server unless the data is valid. However the error messages that the browser displays cannot be styled and might not fit with the look and feel of our site.

One option we have is the jQuery Validation Plugin which was written by one of the jQuery core team. It's very simple to install and appends the error messages to the DOM so they can be styled.

### Installation

Simply add the jQuery library and the jQuery validate script above your own client side JavaScript file:

```html
<head>
  <meta charset="utf-8">
  <title>Cheesebored</title>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.17.0/jquery.validate.min.js" charset="utf-8"></script>
  <script src="/js/app.js" charset="utf-8"></script>
</head>
```

Inside your client JavaScript file (`app.js`) simply add the following to activate the plugin:

```js
$(() => {
  $('form').validate();
});
```

The error messages should now appear below the form fields and can be styled with CSS:

![](https://user-images.githubusercontent.com/3531085/36448530-dabedf8e-167f-11e8-91b5-7d4b20c14927.png)

## Further reading

- [Mongoose Validations](http://fiznool.com/blog/2014/04/23/mongoose-validations/)
- [Client-Side Form Validation with HTML5](https://www.sitepoint.com/client-side-form-validation-html5/)
- [jQuery Validation Plugin](https://jqueryvalidation.org/)
