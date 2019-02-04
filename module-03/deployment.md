# Deploying a MERN stack app on Heroku

> **Note**: MERN stands for Mongo, Express, React and Node

Heroku is a web hosting platform and deployment pipeline that uses git. It is free in the most part, but does have usage bands, so if your site is receiving a lot of traffic you may be charged.

## Setting up Heroku

1. First sign up to [Heroku](https://heroku.com)

  You should set your language to _Node.js_ and your position to _student_. You do not have to supply a company name.

1. You should now [download the Heroku toolbelt](https://devcenter.heroku.com/articles/heroku-cli#download-and-install), which is a command line tool for deployment. Follow the instructions for installation with _Homebrew_.

1. Once the CLI has installed, you need to log in to heroku in the terminal:

  ```sh
  heroku login
  ```

  A browser window will open, allowing you to log in. Once you have logged in via the browser, you should see a success message **in the terminal**.

1. Navigate to the root of your MERN stack project. This should be a git repo. If not, you need to initialise it with `git init`

1. Ceate a Heroku app with the following command:

  ```sh
  heroku create region=eu project-name
  ```

  > **Note** replace `project-name` with the name of your project. This will become part of the website's URL.

## Setting up your app for deployment

1. Add a `.env` file to store your port number (and any API keys):

  ```sh
  PORT=4000
  ```

  Ensure that your `.env` file is loaded using the `dotenv` package:

  ```js
  require('dotenv').config()
  ```

  Update your port number to use `process.env.PORT` in `index.js`:

  ```js
  app.listen(process.env.PORT, () => console.log(`Running on port ${process.env.PORT}`))
  ```

1. Add a `start` script to your `package.json`, this will be used by Heroku to launch your app:

  ```json
  "scripts": {
    "serve": "webpack-dev-server --mode=development",
    "build": "webpack -p",
    "start": "node index"
  }
  ```

1. Build your React app

  ```sh
  yarn run build
  ```

1. Test your app locally by running `yarn start`, then navigating to `http://localhost:4000`. Fix any bugs or issues.

1. Commit your code

  ```sh
  git add . && git commit -m "Ready for dpeloyment"
  ```

1. Deploy to Heroku

  ```sh
  git push heroku master
  ```

1. Once your code has finished deploying, open your app with `heroku open`

1. If you get an Application Error, check the logs on heroku with `heroku logs --tail`

## Adding a database

If you are using a database for your app, you will also need to create a Mongo database instance on heroku.

1. Add a credit/debit card to your Heroku account. _Don't worry, you will not be charged, unless you have large volumes of traffic to your site._

1. In the terminal create a Mongo instance for your Heroku app:

  ```sh
  heroku addons:create mongolab
  ```

1. Setup and environment variable for your local database in `.env`:

  ```sh
  PORT=4000
  MONGODB_URI="mongodb://localhost/project-name"
  ```

  > **Note:** Replace `project-name` with the name of your database. You must use `MONGODB_URI` for this to work on Heroku

1. Update your codebase to use the new environment variable:

  ```js
  mongoose.connect(proess.env.MONGODB_URI)
  ```
