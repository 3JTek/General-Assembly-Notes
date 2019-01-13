# Setting up an Express App

It's important to understand how an Express app is setup and most importantly in what _order_.

This should serve as a reference for setting up an Express app which you should refer to every time you start a new project.

## Step 1: Initialise your project

1. Create a project folder
1. Inside create an `index.js` file, this is the main file for your app, and should contain most of the configuration
1. Initialise the project as a node app with `yarn init` \(You will be presented with a set of questions, the default options should suffice\)
1. Install Express with `yarn add express`

## Step 2: Initial setup

1. In the `index.js` file, require Express at the top
  ```js
  const express = require('express')
  ```
1. Create the app by invoking the `express` module
  ```js
  const app = express()
  ```
1. Set a `PORT` variable to something sensible \(like `3000`, `8000` or `8080`\)
  ```js
  const PORT = process.env.PORT || 8000
  ```
1. Start the app listening out for incoming connections with a simple `console.log`
  ```js
  app.listen(PORT, () => console.log(`Up and running on port ${PORT}`))
  ```

Your `index.js` should look something like this:

```js
const express = require('express')
const app = express()
const PORT = process.env.PORT || 8000

app.listen(PORT, () => console.log(`Up and running on port ${PORT}`))
```

You should now be able to start your app with `nodemon`, and you should see `Up and running on port 8000` in the terminal.

> **Note:** make sure you have installed the `nodemon` package with Yarn: `yarn global add nodemon`

## Hooking up the database

1. Install `mongoose` with `yarn add mongoose`
1. Require it in your `index.js` file
1. Connect `mongoose` to your database instance with `mongoose.connect`

Your `index.js` file should look something like this:

```js
const express = require('express')
const mongoose = require('mongoose')
const app = express()

const port = process.env.PORT || 8000
const dbURI = process.env.MONGODB_URI || 'mongodb://localhost:27017/database-name'

mongoose.connect(dbURI)

app.listen(port, () => console.log(`Up and running on port ${PORT}`))
```

> Note if you get the following warning, it is because your mongodb is not running. Start it with `mongod` in a new terminal tab.
>
> `Unhandled rejection MongoNetworkError: failed to connect to server [localhost:27017] on first connect [MongoNetworkError: connect ECONNREFUSED 127.0.0.1:27017]`

## Create your environment file

You can now move some config variables to a dedicated environment file.

1. Create the `config` folder and `environment.js` file inside
1. Add and export the `port` and `dbURI` variables inside like so:
  ```js
  const port = process.env.PORT || 8000
  const dbURI = process.env.MONGODB_URI || 'mongodb://localhost:27017/database-name'

  module.exports = { port, dbURI }
  ```
1. Import the variables from `config/environment.js` in your `index.js` file

Your `index.js` file should now look something like this:

```js
const express = require('express')
const mongoose = require('mongoose')
const app = express()

const { port, dbURI } = require('./config/environment')

mongoose.connect(dbURI)

app.listen(port, () => console.log(`Up and running on port ${PORT}`))
```

## Add a RESTful route

> **Note:** This step requires making a model, controller and router for a resource. In this example we shall be using pizza as our resource

1. Create your model. You will need to decide what properties it should have. **Models should always be referred to in the singular**, and should be placed in a dedicated file in a `models` folder.
  ```js
  const mongoose = require('mongoose')
  const pizzaSchema = new mongoose.Model({
    name: String,
    image: String,
    toppings: [ String ],
    price: Number
  })

  module.exports = mongoose.model('Pizza', pizzaSchema)
  ```
1. Create your controller. **Controllers should always be referred to in the plural**, and should be placed in a dedicated file in a `controllers` folder.
  ```js
  const pizza = require('../models/pizza')

  function indexRoute(req, res, next) {
    Pizza
      .find()
      .exec()
      .then(pizzas => res.json(pizzas))
      .catch(next)
  }

  module.exports = {
    index: indexRoute
  }
  ```
1. Create you router in the `config` folder:
  ```js
  const router = require('express').Router()
  const pizzas = require('../controllers/pizzas')

  router.route('/pizzas')
    .get(pizzas.index)

  module.exports = router
  ```
1. Hook up your router to in `index.js`:
  ```js
  const express = require('express')
  const mongoose = require('mongoose')
  const app = express()

  const router = require('./config/router')
  const { port, dbURI } = require('./config/environment')

  mongoose.connect(dbURI)

  app.use('/api', router)

  app.listen(port, () => console.log(`Up and running on port ${PORT}`))
  ```

> **Note:** You should now test your RESTful route using insomnia.

## Adding a seeds file

In order to ensure that you are able to get the data you want from the route, you should add some data to the database using a seeds file. The seeds file should be located in `db/seeds.js`:

```js
const mongoose = require('mongoose')
mongoose.Promise = require('bluebird')
const { dbURI } = require('../config/environment')
const Pizza = require('../models/pizza')

mongoose.connect(dbURI, (err, db) => {
  db.dropDatabase()

  Pizza.create([{
    name: 'Hawaiian',
    image: 'https://busites-www.s3.amazonaws.com/blog-margaritaville/2018/06/ThinkstockPhotos-515005958-694x533.jpg',
    price: 8,
    toppings: ['ham', 'pineapple']
  }, {
    name: 'Formaggi',
    image: 'https://ohmydish.com/wp-content/uploads/2017/03/Quattro-formaggi-pizza.jpg',
    price: 8,
    toppings: ['mozzarella', 'parmesan', 'gorgonzola', 'ricotta']
  }])
    .then(pizzas => console.log(`${pizzas.length} pizzas created`))
    .catch(err => console.log(err))
    .finally(() => mongoose.connection.close())
})
```

Once you have created the file, run it with `node db/seeds`

Test your INDEX route with Insomnia, you should see the data appear.

## Next steps

1. Add the rest of your RESTful routes one at a time, testing as you go with Insomnia.
1. Add validations in your model and an error handler
1. Add a user model, authentication and secure route
1. Create an amazing application, change the world! ✌️

## Further reading

* [Express](https://expressjs.com/)
* [REST](https://www.codecademy.com/articles/what-is-rest)
* [MVC](https://www.codecademy.com/articles/mvc)
