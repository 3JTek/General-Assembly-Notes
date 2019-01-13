# Intro to Flask

Flask is a framework for creating HTTP request handlers (ie a server). It is similar to Express, except it's written in Python.

## Setup

The classic Hello World! server setup is about five lines of code for a Flask server.

First you'll need to create a project directory and install Flask with `pipenv`:

```sh
mkdir flask-hello-world
cd flask-hello-world
pipenv install flask
```

Now create the main file (similar to `index.js` with Express), and import Flask. Flask expects this file to be called `app.py`. We can configure Flask to use any filename, but for now, we'll stick with convention:

```py
# app.py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
  return 'Hello World!', 200
```

Launch the server with the following command:

```sh
pipenv run flask run
```

You should see the following terminal output:

```sh
* Environment: production
 WARNING: Do not use the development server in a production environment.
 Use a production WSGI server instead.
* Debug mode: off
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

If you navigate to localhost:5000, you should see your Hello World! message.

## Configuring your Flask app

### Development mode

The first thing we need to do is put Flask into development mode. The major benefit of this is that it will automatically restart when changes are made to the code base (just like `nodemon`).

To do this we will need to set up an environment variable. With `pipenv` this is fairly trivial. Create a file called `.env` then add the following:

```sh
FLASK_ENV=development
```

Now stop and restart your server:

<kdb>CTRL</kdb> + <kbd>C</kbd>
```sh
pipenv run flask run
```

You should see the following:

```sh
* Tip: There are .env files present. Do "pip install python-dotenv" to use them.
* Environment: development
* Debug mode: on
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
* Restarting with stat
* Tip: There are .env files present. Do "pip install python-dotenv" to use them.
* Debugger is active!
* Debugger PIN: 286-576-526
```

You should notice the "tip" regarding the `.env` file we have just created. Since `pipenv` is already loading this file for us **we do not need to install `python-dotenv`**. To supress this message add the following to the `.env` file:

```sh
FLASK_ENV=development
FLASK_SKIP_DOTENV=1
```

Restart your server, the message should should see the following:

```sh
* Environment: development
* Debug mode: on
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
* Restarting with stat
* Debugger is active!
* Debugger PIN: 286-576-526
```

### Changing the port number

While there is nothing wrong with port `5000` for your Flask app, let's change the port to `4000` to maintain consistency with Express. To do this we need to pass a flag to the `flask run` command:

Stop your server, but this time start it with the following command:

```sh
pipenv run flask run --port=4000
```

This may seem like a lot to type each time you want to start your app, but remember that you can always use the up key on your keyboard to auto populate the last command at your prompt.

### Changing the main file name

`app.py` is perfectly adequate as the main file for a flask app, but for consistencies sake, let's rename it to `main.py` since this is more common in the Python community.

To make Flask aware of the change, we'll update our `.env` file like so:

```sh
FLASK_ENV=development
FLASK_SKIP_DOTENV=1
FLASK_APP=main.py
```

Restart and test your server, everything should be working as usual.

## Further reading

* [Quickstart - Flask](http://flask.pocoo.org/docs/1.0/quickstart/)
* [Official Docs - Flask](http://flask.pocoo.org/)
