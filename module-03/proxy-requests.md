# Proxy Requests

Often when we make AJAX requests we are faced with the following error:

![](https://user-images.githubusercontent.com/3531085/38369027-b3148caa-38de-11e8-9522-48aa0b2ab230.png)

This means that our browser is not allowing us to make the request, because the server we are requesting has not sent an **Access-Control-Allow-Origin** header.

This is unfortunate, but not impossible to work around.

## Making a request from the server

Rather than requesting the data with an AJAX request, we can use Request Promise to make the request on the server-side. This will circumvent the missing Access-Control-Allow-Origin header, because it is the browser that is preventing the request from being made.

### Create a controller

First create a controller to make your proxy request. In this example we'll use the Dark Sky API, so we'll call our controller `darkSky.js`. Inside we'll create a single function to make a request:

```js
const rp = require('request-promise');

function forecast(req, res, next) {
  rp({
    method: 'GET',
    url: 'https://api.darksky.net/forecast/a8a8aba4c0420ee1316b86a0ba2c644d/37.8267,-122.4233',
    json: true
  })
    .then(data => res.json(data))
    .catch(next);
}

module.exports = {
  forecast
}
```

> **Note**: Remember to _NEVER_ hard code API keys in your source code. Add them to your `.zshrc` file as environment variables

> **Note**: the API key used in this example is not valid, you will have to create your own at https://darksky.net/dev

### Hook it up to your router

We can now hook this up to an end point on our own API in `config/routes.js`:

```js
const router = require('express').Router();
const darkSky = require('../controllers/darkSky');

router.route('/forecast')
  .get(darkSky.forecast);

module.exports = router;
```

### Test with Insomnia

You should now be able to make a request with insomnia to your newly created endpoint. In the example above we've used `GET http://localhost:4000/api/forecast`. You should see the data coming through from Dark Sky.
