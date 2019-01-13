# Routing

Traditionally when we make a request to a new path on a website, it would refer to a folder in a the file structure. For example if we navigated to `example.com/about` that would refer to a file in the server's file structure: `/about/index.html`.

That file would then hold all of the HTML for the about page, including the `head` and links to CSS and JS files.

That would mean that every page on the website would have to have its own folder. This leads to a huge amount of files and folders, often with very small differences.

Let's take Amazon as an example website. If you search for 'Lord of the Rings' you will see entries in the 'books' category, 'films' category and 'games' category amongst others. But the layout of the pages are all the same. It's the same _template_ with different data in it.

Rather than creating a new folder and `index.html` for each page on the site, we can reuse a few templates and simply **change the data that they display**.

In order to do this we need to detect the path that the user has navigated to and serve the relevant template. For example if the user navigates to `/books` or `/films` or `/toys` we can use the `category` template. If the user has navigated to a specific product we can use the `product` template.

In this sense rather than using the URL of the site to line up with the file structure of the server, we are _routing_ the request to the appropriate template. In an Express app we use a specific file whose job it is to perform this routing. It checks the URL being used and decides which template to use.

## Setting up a router

It's important that we keep things tidy when working on an Express app, because as things get more complex, the number of files we have will increase, and it's important we know where everything is.

* Make a `router.js` file in a `config` folder
  ```sh
  mkdir config && touch config/router.js
  ```
* Create an express router at the top of the file
  ```js
  const router = require('express').Router();
  ```
* Add your routes
  ```js
  router.get('/', (req, res) => res.json({ message: 'HOMEPAGE' }));
  router.get('/about', (req, res) => res.json({ message: 'ABOUT' }));
  router.get('/contact', (req, res) => res.json({ message: 'CONTACT' }));
  ```
* Export the router at the bottom of the file
  ```js
  module.exports = router;
  ```
* Require the router in `index.js`
  ```js
  const router = require('./config/router');
  ```
* And tell Express to use it **making sure you place it just above the **`app.listen`** line**

  ```js
  app.use(router); // MUST BE PLACED JUST BEFORE app.listen

  app.listen(PORT, () => console.log(`Up and running on ${PORT}`));
  ```

You should now be able to navigate to `/`, `/about` and `/contact`.

## Adding some data

OK, so now we have added some routes. Let's now inject different data depending on the route.

We'll make a route for `/fantasy` and `/action` and render them using a single template. We'll send in an array of objects to display.

* Create a `data` folder
  ```sh
  mkdir data
  ```
* Add `fantasy.js` and `action.js` files
  ```sh
  touch data/fantasy.js data/action.js
  ```
* Create some data to work with inside each file
  ```js
  module.exports = [{
    title: 'Lord of the Rings',
    director: 'Peter Jackson',
    image: 'https://upload.wikimedia.org/wikipedia/en/8/87/Ringstrilogyposter.jpg'
  }, {
    title: 'Harry Potter',
    director: 'Chris Columbus',
    image: 'https://files.readanybook.com/786122/files/harry-potter.jpg'
  }];
  ```
* Add the data into the `config/router.js` file
  ```js
  const router = require('express').Router();
  const fantasy = require('../data/fantasy');
  const action = require('../data/action');
  ```
* Inject the data into the views
  ```js
  router.get('/fantasy', (req, res) => res.json({ data: fantasy }));
  router.get('/action', (req, res) => res.json({ data: action }));
  ```

You should now be able to navigate to `/action` and `/fantasy` and see different films.

## Further reading

* [Express Routing - The Beginners Guide](http://jilles.me/express-routing-the-beginners-guide/)
* [Learn How To Use the New Router in Express 4](https://scotch.io/tutorials/learn-to-use-the-new-router-in-expressjs-4)
* [Express Routing](https://expressjs.com/en/guide/routing.html)
