# Method Override

REST is great when building out web applications because it gives us very clear template to structure our code. There is one small problem however, in that we cannot actually send `PUT`, `PATCH` or `DELETE` requests from a browser.

For whatever reason a web browser can only send a `GET` and `POST` requests. When we enter an address is the address bar, we are making a `GET` request. When we submit a form with the `method` attribute set to `POST`, we are making a `POST` request.

In the example below, when we submit the form it will make a `POST` request to `/cheeses`:

```html
<form method="POST" action="/cheeses">
  <input type="text" name="name" placeholder="name" />
  <input type="text" name="origin" placeholder="origin" />
  <input type="text" name="image" placeholder="image" />
  <textarea name="tastingNotes" placeholder="Tasting notes"></textarea>

  <button>Submit</button>
</form>
```

Unfortunately we cannot set the form's `method` attribute to `PUT`, `PATCH` or `DELETE`. Or at least we can, but it will be ignored.

To get around this we can use a package called Method Override. What this does is converts a `POST` request to `PATCH`, `PUT` or `DELETE` once the request has been received by the Express app.

## Setup

It's quite straightforward to setup by following the [installation instructions](https://github.com/expressjs/method-override#custom-logic) online.

It depends on Body Parser, so must appear below it in `index.js`, and _before_ the router:

```js
const PORT = process.env.PORT || 8000;
const router = require('./config/router');

const bodyParser = require('body-parser');
const methodOverride = require('method-override');

// other settings

// setup body-parser
app.use(bodyParser.urlencoded({ extended: true }));

// setup method-override
app.use(methodOverride(function (req, res) {
  if (req.body && typeof req.body === 'object' && '_method' in req.body) {
    // look in urlencoded POST bodies and delete it
    const method = req.body._method
    delete req.body._method
    return method
  }
}));

// setup router
app.use(router);

// listen for traffic
app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
```

Don't worry about understand _how_ this works, just make sure you understand _what_ it does. You can always grab the setup from the documentation or a previous project.

## Usage

With Method Override set up we can now enable `PUT`, `PATCH` and `DELETE` methods using forms.

In the example below, when we submit the form it will send a `PUT` request to `/cheeses/5a8c0d2b1ee97714dafa6ce3`:

```html
<form method="POST" action="/cheeses/5a8c0d2b1ee97714dafa6ce3">
  <input type="hidden" name="_method" value="PUT" />

  <input type="text" name="name" placeholder="name" />
  <input type="text" name="origin" placeholder="origin" />
  <input type="text" name="image" placeholder="image" />
  <textarea name="tastingNotes" placeholder="Tasting notes"></textarea>

  <button>Submit</button>
</form>
```

In order for Method Override to work, we need to add a hidden field with the name of `_method` to our form, and the value set to the method that you actually want to use.

Now we are free to make any request type from our forms, and we are able to stick to our REST paradigm.
