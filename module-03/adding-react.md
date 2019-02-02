# Adding a React Frontend to an Express API

Traditionally servers served HTML and CSS files directly from the file system. These files are known as _static_ files, since they are sent _as it_, from the server to be processed exclusively by the client.

**Static files** include:

* HTML
* CSS
* Client-side JavaScript
* Images
* Sounds
* Video files

**Non-static files** include:

* Server-side JavaScript
* SASS / SCSS
* JSON created from database requests
* HTML generated on the server-side through templating engines like [EJS](https://ejs.co/) or [Jade](http://jade-lang.com/)

We can turn Express into a static file server with just a few lines of code:

```js
const express = require('express')
const app = express()

app.use(express.static(`${__dirname}/public`))

app.listen(4000, () => console.log('Static server on port 4000'))
```

> **Note** `__dirname` is a built-in Node.js variable which holds the _absolute path_ of the current directory.

Express is now configured to send static files from a `public` folder in the root of the project. If we were to create a file called `style.css` in `public` we could view it in browser by navigating to `localhost:4000/style.css`.

In this way, the `public` folder can now be considered the `root` of the domain.

## Creating static files with Webpack

When using Webpack on a client-side JavaScript project, we often use the Webpack Dev Server while developing the client-side codebase. This is useful to us because Webpack Dev Server will restart the browser when we make changes in our codebase.

Webpack Dev Server will serve files from the `src` folder directly to the browser, compiling the code _on the fly_. Webpack uses port `8000` for this.

`nodemon` is doing exactly the same thing for us on the server-side. We set up our server-side code to run on port `4000`.

This is great for developing because _any change_ whether on the server or client side will automatically be detected making for a smooth, efficient development workflow.

However, when it comes time to _deploy_ our code to the internet, we want to serve the client-side code statically from the server. This means we only need one domain to run the whole application.

Webpack can compile our client-side app into static files for us using the `webpack -p` command. This will generate static files, like `index.html` and `bundle.js` and store them in a folder specified in the `webpack.config.js` file. This is usually called `dist`, `dest` or `public`.

Rather than compiling _on the fly_, this is a one time process which makes a _hard copy_ of the client-side app at that moment in time.

We can now serve the front-end app, as static files using the following line of code in our server-side `index.js` file:

```js
app.use(express.static(`${__dirname}/public`))
```

> **Note:** This line should come **above** any other routes including API endpoints, error handlers etc.

## Handling AJAX requests

During development, when running our client-side app on port `8000` with Webpack Dev Server, and our server on port `4000` we will get an issue when making AJAX requests.

An AJAX requests coming from `localhost:8000` to `localhost:4000` is considers as a _cross origin_ request. The request is being made from one domain (or origin) to another. Modern web browsers like Chrome see this as a potential security risk and will block the request.

![cors error](https://media.git.generalassemb.ly/user/15120/files/bfe9e600-26f9-11e9-952d-bfee9c64180c)

This is only an issue during development however, since once the front-end app is complete, we will be serving it from the same domain as the server, `localhost:4000` in this case.

The simplest way to work around this is to _proxy_ requests from `localhost:8000` to `localhost:4000`, using a setting in `webpack.config.js`:

```js
devServer: {
  contentBase: path.resolve('src'),
  hot: true,
  open: true,
  port: 8000,
  watchContentBase: true,
  proxy: {
    '/api': {
      target: 'http://localhost:4000',
      secure: false
    }
  }
}
```

Note the `proxy` section added to the `devServer` settings. Rather than proxying _all_ requests, which would cause problems with frontend routing, we are electing to only proxy requests to endpoints beginning with `/api` in this case.

On the client-side, we can now make requests to the server-side like so:

```js
axios.get('/api/cheeses')
```

rather than the more explicit:

```js
axios.get('http://localhost:4000/cheeses')
```

However, we do need to prefix all server-side API endpoints with `/api`, which we can do like so:

```js
app.get('/api/cheeses', (req, res) => {
  ...
})
```

Or alternatively if using a router, we can prefix all endpoints in one go like so:

```js
app.use('/api', routes)
```

## Overview

Here is an overview of our development setup

|    | Server-side | Client-side |
|:---|:------------|:------------|
| **port** | `4000` | `8000` |
| **server** | `node` | `webpack-dev-server` |
| **start command*** | `nodemon` | `yarn run serve` |

## Further reading

* [How to Configure a Webpack Proxy](https://sdk.gooddata.com/gooddata-ui/docs/4.1.1/ht_configure_webpack_proxy.html)
* [Setting up React & Express, with Hot Module Replacement](https://blog.campvanilla.com/react-express-hot-module-reloading-with-webpack-dev-server-5c9c67dcbb5e)
