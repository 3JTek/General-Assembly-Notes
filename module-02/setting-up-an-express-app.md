# Setting up an Express App

It's important to understand how an Express app is setup and most importantly in what _order_.

This should serve as a reference for setting up an Express app which you should refer to every time you start a new project.

## Step 1: Initialise your project

- Create a project folder
- Inside create an `index.js` file, this is the main file for your app, and should contain most of the configuration
- Initialise the project as a node app with `yarn init` (You will be presented with a set of questions, the default options should suffice)
- Install Express with `yarn add express`

## Step 2: Initial setup

- In the `index.js` file, require Express at the top
  ```js
  const express = require('express');
  ```
- Create the app by invoking the `express` module
  ```js
  const app = express();
  ```
- Set a `PORT` variable to something sensible (like `3000`, `8000` or `8080`)
  ```js
  const PORT = process.env.PORT || 8000;
  ```
- Start the app listening out for incoming connections with a simple `console.log`
  ```js
  app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
  ```

Your `index.js` should look something like this:

```js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 8000;

app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
```

You should now be able to start your app with `node index` or `nodemon` (if you have installed the `nodemon` package), and you should see `Up and running on port 8000` in the terminal.

```
node index
```

## Step 3: Create a `public` folder for static files

- Create a folder called `public`, this is where your client JS, and CSS files will live
- Create an `index.html` file inside with a `<h1>` with some text like `Hello World!` or something similar
- Tell Express to look in that folder for static files:
  ```js
  app.use(express.static(`${__dirname}/public`));
  ```

  >**Note** `__dirname` is the name of the directory that the file is in, so in this case it is the root directory of the project

- Make sure you place that line **above the `app.listen` statement which should always be the last line of the `index.js` file**

If you restart the app now, you should be able to navigate to `http://localhost:8000/` and see `Hello World!` in the browser.

## Step 4: Setting up the view engine

- Create a folder called `views`, this is where your templates will live
  ```sh
  mkdir views
  ```
- Inside `views` folder create a `layout.ejs` file.
  ```sh
  touch veiws/layout.ejs
  ```
- Add the HTML boilerplate and `<%- body %>` tag
  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>TEST</title>
    </head>
    <body>
      <%- body -%>
    </body>
  </html>
  ```
- Create another folder in `views` called `pages` for your main template files
  ```sh
  mkdir views/pages
  ```
- Inside `views/pages` create a `home.ejs` file
  ```sh
  touch views/pages/home.ejs
  ```
- Add some content in `home.ejs`
  ```html
  <h1>Hello World!</h1>
  ```
- Install `ejs` and `express-ejs-layouts`
  ```sh
  yarn add ejs express-ejs-layouts
  ```
- Configure Express to use `ejs`
  ```js
  app.set('view engine', 'ejs');
  ```
- Tell Express to look for your template files in the `views` folder
  ```js
  app.set('views', `${__dirname}/views`);
  ```
- Tell Express to use `express-ejs-layouts`
  ```js
  const expressLayouts = require('express-ejs-layouts');
  ...
  app.use(expressLayouts);
  ```
- Finally add a request listener to serve the `home` template
```js
app.get('/', (req, res) => res.render('pages/home'));
```
- If you restart your server, you should see your `Hello World!` on the homepage. Your `index.js` file should now look like this:
  ```js
  const express = require('express');
  const app = express();
  const PORT = process.env.PORT || 8000;
  const expressLayouts = require('express-ejs-layouts');

  app.set('view engine', 'ejs');
  app.set('views', `${__dirname}/views`);
  app.use(expressLayouts);

  app.get('/', (req, res) => res.render('pages/home'));

  app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
  ```
- Your folder structure should now look something like this:
  ```sh
  .
  ├── index.js
  ├── node_modules
  ├── package.json
  ├── views
  │   ├── layout.ejs
  │   └── pages
  │       └── home.ejs
  └── yarn.lock
  ```
