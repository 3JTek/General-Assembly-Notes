# URL and POST Parameters

Often we need to send data from a client (a browser, for example), to a server so that it can be processed, and potentially saved to a database.

Let's look at a few ways we can do that.

## Params

With express, we can set up parameters for our routes. This is useful if we want to pass a specific ID to the server to find a specific record in a SHOW route for example:

```js
app.get('/films/:id', (res, res) => {
  Film.findById(req.params.id, (err, film) => {
    if(err) console.log(err);
    console.log(film);
  });
});
```

Here we use a special syntax when we set up the route: `/films/:id`. This says to Express that anything after the `/films/` should be treated as a parameter.

Take the following URL as an example: `/films/5a82d0ba2f65c8c6cb4d77f1`. `req.params` would be the following object: `{ id: '5a82d0ba2f65c8c6cb4d77f1' }`.

This is useful because we can now easily access that portion of the URL inside our controllers.

## Query string

We can also pass data to a server in a GET request using the _query string_ portion or the URL. This is normally quite useful when we want to filter a set of results on an INDEX route.

To access the query string in Express we can use `req.query`.

Here's an example using a query string to return a sample of records in a database to an INDEX route.

Take the following URL for example

```
/films?genre=action
```

We can return just the action films with Mongoose like so:

```js
app.get('/films', (req, res) => {
  Film.find(req.query, (err, films) => {
    if(err) console.log(err);
    console.log(films);
  });
});
```

In the above instance `req.query` is the following object: `{ genre: 'action' }`.

## Body

Passing data in the URL is useful, but not always ideal. If we want to log in to a website, we wouldn't want our password appearing in the URL. If we want to send data a little more discreetly we can POST the data.

When we POST data it is sent along with the request but is not visible to the user.

In order to access this kind of data we need to add a package to Express called Body Parser. This should be done in the `index.js` file:

```js
const bodyParser = require('body-parser');

// other settings and middleware

// body-parser MUST come before the router
app.use(bodyParser.json());
app.use(router);

// last line of index.js
app.listen(port, () => console.log(`Up and running on port ${port}`));
```

The `json` method tells Body Parser to looking out for JSON data. It's important that this is setup _before_ the router, because we will want Body Parser to grab the data for us _before_ we make any database interactions.

Now that Body Parser is set up, we can send form data, and get access to if with `req.body`. Let's have a look at how we would use the data from a form to create a new record in our database:

```js
app.post('/films', (res, res) => {
  Film.create(req.body, (err, film) => {
    if(err) console.log(err);
    console.log(film);
  });
});
```

Here all the form data would be added to `req.body` by body parser, so `req.body` might look something like this:

```js
{
  name: 'Die Hard',
  director: 'John McTiernan',
  genre: 'Action'
}
```

## Overview

|             | `req.params`     | `req.query`             | `req.body`   |
|-------------|------------------|-------------------------|--------------|
| **data**    | URL path         | URL querystring         | REQUEST body |
| **example** | `/cheeses/:name` | `/cheeses?origin=Italy` | FORM data    |

## Further reading

- [Use ExpressJS to get URL and POST Parameters](https://scotch.io/tutorials/use-expressjs-to-get-url-and-post-parameters)
- [Get Query Strings and Parameters in Express.js](http://stackabuse.com/get-query-strings-and-parameters-in-express-js/)
- [How bodyParser() works](https://medium.com/@adamzerner/how-bodyparser-works-247897a93b90)
