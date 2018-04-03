# Authentication with React

We are going to look at how to implement authentication with React, an Express API and JWT tokens.

The API has already been completed - if it receives a `POST` request to `/api/register`, a new user will be created in the database. If the API receives a `POST` request to `/api/login` with correct credentials, a JWT token is sent back as part of the response.

On the front end, we are going to look at how to store that JWT token in local storage, and then send it back as part of the request when attempting to make a protected request.

We will also look at hiding and showing buttons/links depending on whether or not the user is logged in, and redirecting the user if they attempt to access protected routes when not logged in.

## Express API

Before we begin building out the React side of this authenticated app, have a look at the Express API to remind yourself of how JWT authentication works in the back end.

### Controllers

The `foods` controller is a simple RESTful controller - we can **c**reate, **r**ead, **u**pdate and **d**elete foods. Have a look at the `auth` controller, and remind yourself of the `login` and `register` methods.

The `login` method will return an **unauthorized** error if the user does not exist, or if the password is incorrect. If the user has been authorized, a JWT token is created and returned as part of the response.

```js
function login(req, res, next) {
  User
    .findOne({ email: req.body.email })
    .then((user) => {
      if(!user || !user.validatePassword(req.body.password)) return res.status(401).json({ message: 'Unauthorized' });

      const token = jwt.sign({ userId: user.id }, secret, { expiresIn: '1hr' });
      return res.json({ message: `Welcome back ${user.username}`, token });
    })
    .catch(next);
}
```

### Lib

Inside `lib` there is the `secureRoute.js` file, which is required and used inside `config/routes`. For the `foodsCreate`, `foodsUpdate` and `foodsDelete` methods a user must be logged in, so the `secureRoute` method first checks for a header called `Authorization`, and if it can't find one, will return **unauthorized**.

If it can find a header called `Authorization` it will attempt to take the token from from the string (`Bearer GRAB-TOKEN-FROM-HERE`), decode it using the `jwt` package, and then retrieve the user from the database. If the user does not exist, it will return **unauthorized**, else it will call `next()` and allow us to create, update or delete a food.

```js
const Promise = require('bluebird');
const jwt = Promise.promisifyAll(require('jsonwebtoken'));
const { secret } = require('../config/environment');
const User = require('../models/user');

function secureRoute(req, res, next) {
  if(!req.headers.authorization) return res.unauthorized();

  const token = req.headers.authorization.replace('Bearer ', '');

  jwt.verifyAsync(token, secret)
    .then((payload) => {
      return User.findById(payload.userId);
    })
    .then((user) => {
      if(!user) return res.unauthorized();
      req.currentUser = user;
      return next();
    })
    .catch(next);
}

module.exports = secureRoute;
```

## React App

### Register

The register functionality has already been completed. Open the `Register` component and have a look at the `handleSubmit` method.

```js
handleSubmit = (e) => {
  e.preventDefault();
  Axios.post('/api/register', this.state.user)
    .then(() => this.props.history.push('/login'))
    .catch(err => console.log(err));
}
```

When the register form is submitted, a `POST` request is made using `Axios`. Once it is complete, the user is redirected to the login page. Register yourself as a user, so that we can work on the login functionality.

### Login

Open up the `Login` component and have a look at the `handleSubmit` method.

```js
handleSubmit = (e) => {
  e.preventDefault();
  Axios.post('/api/login', this.state.credentials)
    .then(res => this.props.history.push('/'))
    .catch(err => console.log(err));
}
```

We are using `Axios` to make an AJAX request to the API, and sending in the form data (the user's email and password). Inside the `.then()` callback we are redirecting the user to the homepage by pushing `/` into the history. Let's break this function on to two lines, and console log the response.

```js
handleSubmit = (e) => {
  e.preventDefault();
  Axios.post('/api/login', this.state.credentials)
    .then((res) => {
      console.log(res);
      this.props.history.push('/');
    })
    .catch(err => console.log(err));
}
```

When you submit the login form you should see a token come back as part of the response in the Chrome console. Instead of console logging, we want to save this token to local storage.

Have a look at the `Auth` class inside `lib/Auth`. Here we have a class that has some useful methods that we will be using throughout this codealong. The first that we are going to use is `setToken`, which uses the [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Storage) to set an item in local storage.

Import this class into the `Login` component:

```js
import Auth from '../../lib/Auth';
```

Then use the `Auth.setToken()` method and pass in the token that has been returned from the API.

```js
handleSubmit = (e) => {
  e.preventDefault();
  Axios.post('/api/login', this.state.credentials)
    .then((res) => {
      Auth.setToken(res.data.token);
      this.props.history.push('/');
    })
    .catch(err => console.log(err));
}
```

Submit the login form with valid credentials, and check local storage in the 'Application' tab in Chrome. You should see the token stored there.

Great! That wasn't too bad at all right?

### Sending the token in a header

The create, update and delete routes are protected in our API, so we will need to send the token as a header, with the key of `"Authorization"`, and a value of `"Bearer TOKEN-FROM-LOCAL-STORAGE-HERE"`.

#### Creating a food

Inside the `FoodsNew` component import the `Auth` class, and then update the `Axios` request to include a a header.

This time we are going to use the `.getToken()` method, which will retrieve the token from local storage for us.

**Important:** Remember to add a space after `Bearer`.

```js
import Auth from '../../lib/Auth';

...

handleSubmit = (e) => {
  e.preventDefault();

  Axios
    .post('/api/foods', this.state.food, {
      headers: { 'Authorization': 'Bearer ' + Auth.getToken() }
    })
    .then(() => this.props.history.push('/'))
    .catch(err => console.log(err));
}
```

#### Updating a food

We are going to do the same thing inside the `FoodsEdit` component. First import `Auth`, and then add a header to the `Axios` request.

```js
import Auth from '../../lib/Auth';

...

handleSubmit = (e) => {
  e.preventDefault();

  Axios
    .put(`/api/foods/${this.props.match.params.id}`, this.state.food, {
      headers: { 'Authorization': 'Bearer ' + Auth.getToken() }
    })
    .then(res => this.props.history.push(`/foods/${res.data.id}`))
    .catch(err => console.log(err));
}
```

#### Deleting a food

We will need to do the same again when deleting a food. Inside the `FoodsShow` component import `Auth`, and then add a header to the `Axios` request.

```js
import Auth from '../../lib/Auth';

...

deleteFood = () => {
  Axios
    .delete(`/api/foods/${this.props.match.params.id}`, {
      headers: { 'Authorization': 'Bearer ' + Auth.getToken() }
    })
    .then(() => this.props.history.push('/'))
    .catch(err => console.log(err));
}
```

### Hiding Buttons

Now that we have the ability to login, we want to be able to hide and show elements depending on whether or not the user is authenticated. The `Auth` class has an `.isAuthenticated()` method, which will return true if there is a token in local storage.

#### Index

In `FoodsIndex` compomnet import `Auth`, and wrap the **"Add Food"** link in `{ Auth.isAuthenticated() && ...}`.

```js
import React from 'react';
import Axios from 'axios';
import { Link } from 'react-router-dom';
import Auth from '../../lib/Auth';

class FoodsIndex extends React.Component {

  ...

  render() {
    return (
      <div>
        <div className="row">
          <div className="page-banner col-md-12">
            {Auth.isAuthenticated() && <Link to="/foods/new" className="main-button">
              <i className="fa fa-plus" aria-hidden="true"></i>Add Food
            </Link>}
          </div>
          {this.state.foods.map(food => {
            return(
              <div key={food.id} className="image-tile col-md-4 col-sm-6 col-xs-12">
                <Link to={`/foods/${food.id}`}>
                  <img src={food.image} className="img-responsive" />
                </Link>
              </div>
            );
          })}
        </div>
      </div>
    );
  }
}

export default FoodsIndex;
```

You should still be able to see the button if you're logged in. If you delete the token out of local storage the button should disappear. Neat!

#### Show

Let's do the same thing inside the `FoodsShow` component. Wrap the **"Edit"** and **"Delete"** buttons in `{ Auth.isAuthenticated() && ...}`.

```js
class FoodsShow extends React.Component {

  ...

  render() {
    return (
      <div className="row">
        <div className="image-tile col-md-6">
          <img src={this.state.food.image} className="img-responsive" />
        </div>
        <div className="col-md-6">
          <h3>{this.state.food.title}</h3>
          <h4>{this.state.food.category}</h4>
          <BackButton history={this.props.history} />
          {Auth.isAuthenticated() && <Link to={`/foods/${this.state.food.id}/edit`} className="standard-button">
            <i className="fa fa-pencil" aria-hidden="true"></i>Edit
          </Link>}
          {' '}
          {Auth.isAuthenticated() && <button className="main-button" onClick={this.deleteFood}>
            <i className="fa fa-trash" aria-hidden="true"></i>Delete
          </button>}
        </div>
      </div>
    );
  }
}
```

#### Navbar

We only want to see the **"Logout"** button if we are logged in, and the **"Login"** and **"Register"** buttons if we are logged out.

In `utility/navbar` import `Auth` and wrap the links accordingly.

```js
import Auth from '../../lib/Auth';

import React from 'react';
import { Link } from 'react-router-dom';
import Auth from '../../lib/Auth';

const Navbar = () => {

  return(
    <nav>
      {!Auth.isAuthenticated() && <Link to="/login" className="standard-button">Login</Link>}
      {!Auth.isAuthenticated() && <Link to="/register" className="standard-button">Register</Link>}
      {Auth.isAuthenticated() && <a href="#" className="standard-button">Logout</a>}
    </nav>
  );
};

export default Navbar;
```

Test that you can only see the logout button when logged in, and vice versa.

**Important:** Remember to invoke the `.Auth.isAuthenticated()` method, else it will return the function declaration, which will always be truthy.

### Logout

Since we are inside the navbar component, let's add the logout functionality. The `Navbar` is a functional component, but that doesn't mean that we can't add functions to it.

Add the following `logout` function, that takes the event as an argument. We are going to prevent the default behaviour of the link, to stop the page from refreshing.

We can then use the `Auth.logout()` that simply removes the token from local storage.

```js
const Navbar = () => {

  function logout(e) {
    e.preventDefault();
    Auth.logout();
  }

  return (
    ...
  );
};
```

By clicking on the **"Logout"** link you should be logged out, however we want to redirect the user to the homepage. We want to say `props.history.push('/')`, however we only have access to `props.history` if the component has been rendered using the `<Route />` component.

We can mimic this behaviour be wrapping any component we create with `withRouter`, which comes from the `react-router-dom` library. This allows us to acces the history in the component's props.

Add `withRouter` when deconstructing the `react-router-dom`:

```js
import { Link, withRouter } from 'react-router-dom';
```

Then wrap the `Navbar` export with `withRouter`:

```js
export default withRouter(Navbar);
```

We now get `history` on props, so deconstruct props as it is passed into the component:

```js
const Navbar = ({ history }) => {
  ...
}
```

To redirect to the homepage we can now update the `logout` function to be:

```js
function logout(e) {
  e.preventDefault();

  Auth.logout();
  history.push('/');
}
```

And add an `onClick` to the **"Logout"** link:

```js
{Auth.isAuthenticated() && <a href="#" className="standard-button" onClick={logout}>Logout</a>}
```

The entire `Navbar` component should now look like this:

```js
import React from 'react';
import { Link, withRouter } from 'react-router-dom';
import Auth from '../../lib/Auth';

const Navbar = ({ history }) => {

  function logout(e) {
    console.log('clicked');
    e.preventDefault();

    Auth.logout();
    history.push('/');
  }

  return(
    <nav>
      {!Auth.isAuthenticated() && <Link to="/login" className="standard-button">Login</Link>}
      {!Auth.isAuthenticated() && <Link to="/register" className="standard-button">Register</Link>}
      {Auth.isAuthenticated() && <a href="#" className="standard-button" onClick={logout}>Logout</a>}
    </nav>
  );
};

export default withRouter(Navbar);
```

### Protecting routes

Even though we've hidden the **"Add food"** and **"Edit"** buttons, we can still access those routes through their URLs.

We want to protect those routes, and to do that we are going to make a [higher order component](https://reactjs.org/docs/higher-order-components.html). From the docs:

_A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature._

_Concretely, **a higher-order component is a function that takes a component and returns a new component.**_

We are going to create a higher order component which takes a `<Route />` component as a prop, and if the user is logged in, will render the page, and if not will redirect them to the login page.

Make a new file:

```sh
touch src/components/utility/ProtectedRoute.js
```

Create the following functional component:

```js
import React from 'react';
import { withRouter, Route, Redirect } from 'react-router-dom';
import Auth from '../../lib/Auth';

const ProtectedRoute = ({ component: Component }) => {

  render() {

  }
};
```

As well as importing React, Auth, and React Router, we are creating a const called `Component` and making it equal to `this.props.component` (the component that we will pass in).

Add a spread operator `...other` into the deconstruction of props. This will pull any other props into a `const` called `other`.

```js
const ProtectedRoute = ({ component: Component }) => {

  render() {

  }
};
```

In this instance it will be:

```js
const other = { path: this.props.path }
```

**Note:** `other` is just a naming convention.

We need to return something from this component. Let's return a `<Route />` component, which will take the path stored inside the `other` object, and render the protected component if the user is authenticated, and redirect them if not.

Take a look at the documention for React Router [here](https://reacttraining.com/react-router/web/example/auth-workflow).

```js
import React from 'react';
import { withRouter, Route, Redirect } from 'react-router-dom';
import Auth from '../../lib/Auth';

const ProtectedRoute = (props) => {
  const { component: Component, ...other } = props;
  // 1. creating a const called Component and making it equal to the component being passed in
  // 2. using the spread operator to create an object called other
  // 3. const object = { path: "/foods/:id/edit" } or whichever path it was passed

  return (
    <Route {...other} render={props => (
      Auth.isAuthenticated() ? (
        <Component {...props}/>
      ) : (
        <Redirect to="/login"/>
      )
    )}/>
  );
};
```

Export the component at the bottom of the file, wrapped inside `withRouter`.

```js
export default withRouter(ProtectedRoute);
```


> **Note:** A higher order component is sometimes called a _decorator_ - it takes a component and returns a component, adding extra functionality if needed.

Inside the `Routes` component import the `ProtectedRoute` component:

```js
import ProtectedRoute from '../utility/ProtectedRoute';
```

Then replace the `<Route />` component with `<ProtectedRoute />` for the edit and new routes.

```js
const Routes = () => {
  return (
    <Switch>
      <Route path="/login" component={Login} />
      <Route path="/register" component={Register} />
      <Route exact path="/" component={FoodsIndex} />
      <ProtectedRoute path="/foods/new" component={FoodsNew} />
      <ProtectedRoute path="/foods/:id/edit" component={FoodsEdit} />
      <Route path="/foods/:id" component={FoodsShow} />
      <Route component={NoMatch} />
    </Switch>
  );
};
```
