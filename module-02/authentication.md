# Authentication with React

We are going to look at how to implement authentication with React when consuming an API that uses _JSON Web Tokens_.

If the API receives a `POST` request to `/api/register`, a new user will be created in the database. If it receives a `POST` request to `/api/login` with correct credentials, a JWT token is sent back as part of the response.

We will store that JWT token in _local storage_, and then send it back as part of any request that is secured, eg. _CREATE_, _UPDATE_ and _DELETE_ routes.

We will also look at hiding and showing buttons/links depending on whether or not the user is logged in, and redirecting the user if they attempt to access protected areas of the site, for example the _NEW_ and _EDIT_ pages which would display forms to create and update resources respectively.

### Register

A typical register form submit handler might look something like this:

```js
handleSubmit = (e) => {
  e.preventDefault()
  axios.post('/api/register', this.state.user)
    .then(() => this.props.history.push('/login'))
    .catch(err => console.log(err))
}
```

When the register form is submitted, a `POST` request is made using `axios`. Once it is complete, the user is redirected to the login page.

### Login

A typical register form submit handler might look something like this:

```js
handleSubmit = (e) => {
  e.preventDefault()
  axios.post('/api/login', this.state.credentials)
    .then(res => this.props.history.push('/'))
    .catch(err => console.log(err))
}
```

We are using `axios` to make an AJAX request to the API, and sending in the form data (the user's email and password). Inside the `.then()` callback we are redirecting the user to the homepage by pushing `/` into the history. Let's break this function on to two lines, and console log the response.

```js
handleSubmit = (e) => {
  e.preventDefault()
  axios.post('/api/login', this.state.credentials)
    .then((res) => {
      console.log(res)
      this.props.history.push('/')
    })
    .catch(err => console.log(err))
}
```

When you submit the login form you should see a token come back as part of the response in the Chrome console. We need to store this token in _local storage_ to be used later.

## `Auth` class

We can create a class which can be used to handle storing and retrieving tokens to and from _local storage_. We can also add extra functionality to validate tokens, and helper methods to determine whether a user is logged in (has a token), or not.

Here is an implementation of such a class:

```js
class Auth {
  static setToken(token) {
    localStorage.setItem('token', token)
  }

  static getToken() {
    return localStorage.getItem('token')
  }

  static logout() {
    localStorage.removeItem('token')
  }

  static getPayload() {
    const token = this.getToken()
    if (!token) return false
    const parts = token.split('.')
    if (parts.length < 3) return false
    return JSON.parse(atob(parts[1]))
  }

  static isAuthenticated() {
    const payload = this.getPayload()
    const now = Math.round(Date.now() / 1000)
    return now < payload.exp
  }
}

export default Auth
```

There are 5 methods here:

* `setToken` will take a token and add it to _local storage_
* `getToken` will retrieve a token from _local storage_
* `logout` will clear _local storage_ of a token, essentially logging a user out
* `getPayload` will return the `payload` portion of the token as an object
* `isAuthenticated` will return `true` is there is a valid token in _local storage_ and `false` otherwise

We can now use the `Auth.setToken()` method to store the token that is returned from the API.

```js
handleSubmit = (e) => {
  e.preventDefault()
  axios.post('/api/login', this.state.credentials)
    .then((res) => {
      Auth.setToken(res.data.token)
      this.props.history.push('/')
    })
    .catch(err => console.log(err))
}
```

### Sending the token in a header

The _CREATE_, _UPDATE_ and _DELETE_ routes are protected in our API, so we will need to send the token as a header, with the key of `Authorization`, and a value of `Bearer TOKEN-FROM-LOCAL-STORAGE-HERE`.

Here's a typical form submit handler for creating a new resource, in this case `cheese`:

```js
handleSubmit = (e) => {
  e.preventDefault()

  axios
    .post('/api/cheeses', this.state.food, {
      headers: { 'Authorization': `Bearer ${Auth.getToken()}` }
    })
    .then(() => this.props.history.push('/'))
    .catch(err => console.log(err))
}
```

The edit form submit handler would be very similar:

```js
handleSubmit = (e) => {
  e.preventDefault()

  axios
    .put(`/api/cheeses/${this.props.match.params.id}`, this.state.cheese, {
      headers: { 'Authorization': `Bearer ${Auth.getToken()}` }
    })
    .then(res => this.props.history.push(`/cheeses/${res.data.id}`))
    .catch(err => console.log(err))
}
```

Ditto the delete button:

```js
deleteCheese = () => {
  axios
    .delete(`/api/cheeses/${this.props.match.params.id}`, {
      headers: { 'Authorization': `Bearer ${Auth.getToken()}` }
    })
    .then(() => this.props.history.push('/'))
    .catch(err => console.log(err))
}
```

### Hiding Buttons

Now that we have the ability to login, we want to be able to hide and show elements depending on whether or not the user is authenticated. The `Auth` class has an `.isAuthenticated()` method, which will return true if there is a token in local storage.

If we want to show a button or link only if the user is logged in, for example in the navbar, we can do it like so:
```js
import Auth from '../../lib/Auth'

import React from 'react'
import { Link } from 'react-router-dom'
import Auth from '../../lib/Auth'

const Navbar = () => {

  return(
    <nav>
      {!Auth.isAuthenticated() && <Link to="/login">Login</Link>}
      {!Auth.isAuthenticated() && <Link to="/register">Register</Link>}
      {Auth.isAuthenticated() && <a href="#">Logout</a>}
    </nav>
  )
}

export default Navbar
```

### Loggin out

Since we are inside the navbar component, let's add the logout functionality. The `Navbar` is a functional component, but that doesn't mean that we can't add functions to it.

We can create a `logout` function, which calls the `Auth.logout` method, and add it to the logout button like so:

```js
const Navbar = () => {

  function logout(e) {
    e.preventDefault()
    Auth.logout()
  }

  return(
    <nav>
      {!Auth.isAuthenticated() && <Link to="/login">Login</Link>}
      {!Auth.isAuthenticated() && <Link to="/register">Register</Link>}
      {Auth.isAuthenticated() && <a href="#" onClick={logout}>Logout</a>}
    </nav>
  )
}
```

When we click the logout button the token is removed from _local storage_, however we want to redirect the user to the homepage, using `props.history.push`. However we only have access to `props.history` if the component has been rendered using the `<Route />` component.

We can mimic this behaviour be wrapping any component we create with `withRouter`, which comes from the `react-router-dom` library. This allows us to access `history` in the component's props.

We can add `withRouter` to the navbar like so:

```js
import { Link, withRouter } from 'react-router-dom'

const Navbar = () => {

  function logout(e) {
    e.preventDefault()
    Auth.logout()
  }

  return(
    <nav>
      {!Auth.isAuthenticated() && <Link to="/login">Login</Link>}
      {!Auth.isAuthenticated() && <Link to="/register">Register</Link>}
      {Auth.isAuthenticated() && <a href="#" onClick={logout}>Logout</a>}
    </nav>
  )
}

export default withRouter(Navbar)
```

We now get `history` on props, we can update the `logout` function like so:

```js
const Navbar = (props) => {

  function logout(e) {
    e.preventDefault()
    Auth.logout()
    props.history.push('/')
  }

  return(
    <nav>
      {!Auth.isAuthenticated() && <Link to="/login">Login</Link>}
      {!Auth.isAuthenticated() && <Link to="/register">Register</Link>}
      {Auth.isAuthenticated() && <a href="#" onClick={logout}>Logout</a>}
    </nav>
  )
}
```

### Protecting routes

Although we can now hide links to _NEW_ and _EDIT_ pages, a savvy user can still just update the URL manually in the address bar. We want to protect those routes, and to do that we are going to make a [higher order component](https://reactjs.org/docs/higher-order-components.html).

From the docs:

> _A higher-order component (HOC) is an advanced technique in React for reusing component logic. HOCs are not part of the React API, per se. They are a pattern that emerges from React’s compositional nature._
>
> _Concretely, **a higher-order component is a function that takes a component and returns a new component.**_

We can create a higher order component which takes a `<Route />` component as a prop, and if the user is logged in, will render the page, and if not will redirect them to the login page.

The component might look something like this:

```js
import React from 'react'
import { withRouter, Route, Redirect } from 'react-router-dom'
import Auth from '../../lib/Auth'

const SecureRoute = ({ component: Component, ...rest }) => {
  // Component is the component passed to `SecureRoute`
  // rest is any other props, like `path` for example
  if(Auth.isAuthenticated()) return <Route {...rest} component={Component} />
  return <Redirect to="/login" />
}
```

The `SecureRoute` checks to see if the user is logged in. If so it renders a standard `Route` component, passing the original component and any other props. Otherwise it redirects the user to the login route.

We can now use the `SecureRoute` component just like a `Route` in our `router.js` file:

```
<BrowserRouter>
  <div>
    <Navbar />
    <Switch>
      <Route path="/login" component={Login} />
      <Route path="/register" component={Register} />
      <Route exact path="/" component={CheesesIndex} />
      <SecureRoute path="/cheeses/new" component={CheesesNew} />
      <SecureRoute path="/cheeses/:id/edit" component={CheesesEdit} />
      <Route path="/cheeses/:id" component={CheesesShow} />
      <Route component={NoMatch} />
    </Switch>
  </div>
</BrowserRouter>
```

> **Note:** Check out the documentation for React Router [here](https://reacttraining.com/react-router/web/example/auth-workflow).

> **Note:** A higher order component is sometimes called a _decorator_ - it takes a component and returns a component, adding extra functionality if needed.
