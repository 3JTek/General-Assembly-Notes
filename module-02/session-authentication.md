# Session Authentication

User registration involves creating a user record for a specific user and hashing their password. Authentication is much more involved. A user has to login with a specific set of credentials, normally email and password, and then must be granted permission to perform certain actions on a web application.

## Logging in

The first step in this process is logging in. To do that we need to create a login form and controller to handle the form. We then need to check that the password supplied matched the hashed password in the database.

But if the password in the database is hashed and hashing is a one-way process, how can we check that the plain text password supplied matches the one stored in the database? We can ask Bcrypt to do that on our behalf. It will take the plain text password and hash it with the exact same algorithm as the one stored in the database, and _then_ compare them. If they match then they must both be the same password.

Again since this is to do with data, we will add this functionality to the model.

## `Schema.methods`

Mongoose schema's have a `methods` object which is almost identical to JavaScript's native `prototype` object. Any functions added to the `methods` object will be shared by all instances of the model. This is a great place to add some functionality to check whether a password is valid:

```js
const mongoose = require('mongoose');
const bcrypt = require('bcrypt');

const schema = new mongoose.Schema({
  username: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }
});

schema
  .virtual('passwordConfirmation')
  .set(function setPasswordConfirmation(passwordConfirmation) {
    this._passwordConfirmation = passwordConfirmation;
  });

schema.pre('validate', function checkPassword(next) {
  if(this.isModified('password') && this._passwordConfirmation !== this.password) this.invalidate('passwordConfirmation', 'does not match');
  next();
});

schema.pre('save', function hashPassword(next) {
  if(this.isModified('password')) {
    this.password = bcrypt.hashSync(this.password, bcrypt.genSaltSync(8));
  }
  next();
});

// compareSync compares a plain text password against the hashed one stored on the user object
schema.methods.validatePassword = function validatePassword(password) {
  return bcrypt.compareSync(password, this.password);
};

module.exports = mongoose.model('User', schema);
```

With this added we can now check a user's password like so:

```js
User.findOne({ email: 'mike.hayden@ga.co' })
  .then(user => {
    user.validatePassword('snufflekins'); // returns true if the user's password is "snufflekins" or false otherwise
  });
```

With this in place we can use it in a simple login controller:

```js
function sessionsCreate(req, res, next) {
  User
    .findOne({ email: req.body.email })
    .then(user => {
      // if the user cannot be found, or did not supply a valid password
      if(!user || !user.validatePassword(req.body.password)) {
        return res.redirect('/login'); // send them back to the login page
      }

      res.redirect('/'); // otherwise send them to the homepage
    })
    .catch(next);
}
```

We have now assessed whether the user exists in our database, and whether they have a valid password. We now need to grant them access to specific parts of our web application.

## Cookies

In order to allow a user access to certain features of our site we need to keep their user record available over page load. As a user moves through our site, we need to "remember" that she has already logged in. To do that we will use a cookie.

A cookie is a small temporary file that is stored by a browser onto a user's computer. This file is used to store information as a user is navigating around a site. Information can vary depending on the site. There are several types of cookie, but the one we are interested in is a _session cookie_.

A session cookie is an encrypted cookie which lasts for the duration of a user session on the browser \(basically until the user closes the browser\). We can store information inside a cookie, and then access it again after the page has be reloaded or the user has moved to another page on our site.

When a user logs in, we can store the user's ID in the session cookie, then on page load, use that ID to retrieve the user's data. This way we know that they are authorised to use certain areas of the site that are restricted to unregistered users.

In order to setup session cookies in our Express app we will use a package called Express Session. As always we can install it with yarn:

```sh
yarn add express-session
```

Once installed we can set it up in our `index.js` file:

```js
const session = require('express-session');

// other config including body-parser and method-override

// setup express-session must go BEFORE the router
app.use(session({
  secret: 'GysHa^72u91sk0P(', // a random key used to encrypt the session cookie
  resave: false,
  saveUninitialized: false
}));

// router and global error hander

app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
```

Now that cookies are enabled we can store data in our session cookie using `req.session`:

```js
function sessionsCreate(req, res, next) {
  User
    .findOne({ email: req.body.email })
    .then(user => {
      if(!user || !user.validatePassword(req.body.password)) {
        return res.redirect('/login');
      }
      req.session.userId = user._id; // store the user's ID in the session cookie
      res.redirect('/'); // send them to the homepage
    })
    .catch(next);
}
```

## Checking for cookies on page load

Now that we have an ID stored in the session cookie, we need to check it is there on page load. To do that we need to write a piece of middleware that will check the session cookie for the user's ID and load the user record from the database if one is found:

```js
const session = require('express-session');

// other config including body-parser and method-override

// setup express-session must go BEFORE the router
app.use(session({
  secret: 'GysHa^72u91sk0P(', // a random key used to encrypt the session cookie
  resave: false,
  saveUninitialized: false
}));

app.use((req, res, next) => {
  // if there is no user ID, then there is nothing to do, move on to the routes
  if(!req.session.userId) return next();

  // otherwise use the ID to find the user in the database
  User
    .findById(req.session.userId)
    .then(user => {

      // if the user hasn't been found (perhaps if they have deleted their account)
      // log them out (ie delete the data in the session)
      if(!user) req.session.regenerate(() => res.redirect('/login'));

      // add some helpers to res.locals to be used in the views
      res.locals.isAuthenticated = true;
      res.locals.currentUser = user;

      // store the user data on `req` to be used inside the controllers
      req.currentUser = user;

      next();
    });
});

// router and global error hander

app.listen(PORT, () => console.log(`Up and running on port ${PORT}`));
```

Basically this functionality finds the logged in user's ID in the session and loads the user's data from the database. This is then attached to `res.locals` and `req` to be used in the views and controllers respectively.

## Restricting access to authorised routes

Now that we know if a user is logged in or not, we can restrict access to certain routes. The most sensitive routes would be `CREATE`, `UPDATE` and `DELETE`, but we would probably not want an unauthorised user accessing the `NEW` and `EDIT` routes either.

To restrict access we can create a middleware function that sits in front of our controller actions in the router. This function will act as a bouncer to check whether an incoming request has been made by an authorised user or not:

```js
const router = require('express').Router();
const cheeses = require('../controllers/cheeses');

function secureRoute(req, res, next) {
  // if the user is not logged in
  if(!req.session.userId) {
    // clear the session cookie and redirect them to the login page
    return req.session.regenerate(() => res.redirect('/login'));
  }
  next();
}

// both the following routes are now restricted and can only be accessed by a logged in user
app.get('/cheeses/new', secureRoute, cheeses.new);
app.post('/cheeses', secureRoute, cheeses.create);
```

In the example above the request first has to pass into the `secureRoute` function before it reaches the controller actions. Inside `secureRoute` the session is checked for a userId. If none is found, the session cookie is cleared, and  the user is redirected to the login form.

## Hiding buttons and links

Finally we might want to hide certain buttons \(like the delete button, and edit link\) from a logged in user. We can use the properties we added to `res.locals` to helps us with this:

```js
<% if(locals.isAuthenticated) { %>
  <a class="button is-primary" href="/cheeses/<%= cheese._id %>/edit">Edit</a>
  <form method="POST" action="/cheeses/<%= cheese._id %>">
    <input type="hidden" name="_method" value="DELETE">
    <button class="button is-danger">Delete</button>
  </form>
<% } %>
```

Now only a logged in user will see the edit link and delete buttons on the SHOW route.

## Logging out

Finally we need a way to log a user out. This can be a very simple controller action which regenerates the session cookie:

```js
function sessionsDelete(req, res) {
  req.session.regenerate(() => res.redirect('/'));
}
```

We can hook this up to a GET request `/logout` in our router:

```js
router.get('/logout', sessions.delete);
```

Then finally add the link in the navbar. Notice that only a logged in user would need to see this link:

```html
<% if(locals.isAuthenticated) { %>
  <a class="navbar-item" href="/logout">Logout</a>
<% } %>
```

## Flash Messages

Now our app is complete with user authentication and some protected routes, why don't we improve the user experience with some informative flash messages? Let's start by installing the package with yarn:

```bash
yarn add express-flash
```

Now let's add it to our `index.js` file. It must come underneath the sessions.

```js
app.use(session({
  secret: process.env.SESSION_SECRET || 'ssh it\'s a secret',
  resave: false,
  saveUninitialized: false
}));

app.use(flash());
```

Let's now add this to our `secureRoute` function. Before redirecting the user to the login page, we can let them know that this is happening because they have tried to access protected content.

```js
function secureRoute(req, res, next) {
  if (!req.session.userId) {
    return req.session.regenerate(() => {
      req.flash('danger', 'You must be logged in.');
      res.redirect('/login');
    });
  }

  return next();
}
```

In order to see our flash messages rendered on our app, we need to update our `layout.ejs` file like so:

```
<main class="container">
  <% for (const type in messages) { %>
    <p class="alert alert-<%= type %>"><%= messages[type] %></p>
  <% } %>

  <%- body %>
</main>
```

We can also add this to our custom middleware for finding the logged in user.

```js
User
  .findById(req.session.userId)
  .then((user) => {
    if(!user) {
      return req.session.regenerate(() => {
        req.flash('danger', 'You must be logged in.');
        res.redirect('/');
      });
    }

    // Re-assign the session id for good measure
    req.session.userId = user._id;

    res.locals.user = user;
    res.locals.isLoggedIn = true;

    return next();
  });
```

We can also use flash messages to welcome users when they register and log in:

```js
function registrationsCreate(req, res) {
  User
    .create(req.body)
    .then((user) => {
      req.flash('info', `Thanks for registering, ${user.username}! Please login.`);
      return res.redirect('/login');
    })
    .catch((err) => {
      if (err.name === 'ValidationError') {
        return res.status(400).render('registrations/new', { message: 'Passwords do not match' });
      }
      res.status(500).end();
    });
}
```

```js
function sessionsCreate(req, res) {
  User
    .findOne({ email: req.body.email })
    .then((user) => {
      if(!user || !user.validatePassword(req.body.password)) {
        req.flash('danger', 'Unknown email/password combination');
        return res.redirect('/login');
      }

      req.session.userId = user.id;

      req.flash('info', `Welcome back, ${user.username}!`);
      res.redirect('/');
    });
}
```

You can choose whatever colors you like for your flash messages using CSS, but the default options refer to some colors defined by Bootstrap. These include 'danger' =&gt; red, 'success' =&gt; green, 'warning' =&gt; yellow. For more colors, [see the docs](https://getbootstrap.com/docs/4.0/utilities/colors/). 

## Further reading

* [Cookies - The Royal Family](https://www.royal.uk/cookies)
* [Express Session](https://github.com/expressjs/session)
* [How do Express.js Sessions Work?](https://nodewebapps.com/2017/06/18/how-do-nodejs-sessions-work/)



