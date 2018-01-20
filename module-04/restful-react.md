# RESTful React

React makes no assertions on how one should go about building out a complex web application. This makes it extremely flexible, but also daunting at times, since there is very little to go on in terms of best practises.

What follows is an outline of how we teach RESTful with React, but is by no means the definitive approach.

## Folder structure

When complete your folder structure should look something like this:

```sh
├── .babelrc
├── .gitignore
├── config         ---
├── controllers       |
├── db                |
├── server.js         |
├── lib               |
├── models            |---   Express API
├── node_modules      |
├── nodemon.json      |
├── package.json      |
├── public         ---
├── src
│   ├── components
│   │   ├── auth     <----   Register and login components
│   │   ├── cats     <----   RESTful resource components
│   │   ├── common   <----   Sitewide components
│   │   └── static   <----   Static pages like home / about etc
│   ├── index.html   <----   HTML boilerplate
│   ├── app.js       <----   App component with routes
│   ├── lib          <----   Helper classes
│   └── scss         <----   Custom styles
├── webpack.config.js
└── yarn.lock
```

## `app.js`

This is the main application component. It should generally be a classical component, due to limitations with hot reloading.

It should contain the basic structure of the app, and all the app's routes.

Something like this:

```js
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import Navbar from '../components/common/Navbar';

import Home from '../components/static/Home';
import CatsIndex from '../components/cats/CatsIndex';
import CatsNew from '../components/cats/CatsNew';
import CatsShow from '../components/cats/CatsShow';
import CatsEdit from '../components/cats/CatsEdit';
import Login from '../components/auth/Login';
import Register from '../components/auth/Register';

class App extends React.Component {
  render() {
    return (
      <Router>
        <div>
          <Navbar />
          <div className="container">
            <Route exact path="/" component={Home} />

            <Switch>
              <Route exact path="/cats" component={CatsIndex} />
              <Route path="/cats/new" component={CatsNew} />
              <Route path="/cats/:id/edit" component={CatsEdit} />
              <Route path="/cats/:id" component={CatsShow} />
            </Switch>

            <Route path="/login" component={Login} />
            <Route path="/register" component={Register} />

          </div>
        </div>
      </Router>
    );
  }
}

export default App;
```

## `cats` folder

The components you will need for a RESTful resource will vary depending on your choice of UI and how you want your application to work.

Generally I would go with at least the following components:

#### Functional components:

- `<Cat />`: A simple component that renders a single resource. This can be used on both the `INDEX` and `SHOW` routes.
- `<CatForm />`: A form that can be used for both the `NEW` and `EDIT` routes

#### Classical components:

One for each route:

- `<CatIndex />`
- `<CatNew />`
- `<CatShow />`
- `<CatEdit />`

Load in any data you need on each route in the `componentDidMount()` lifecycle hook, so that you can display the data on the page.

## `common` folder

Any components that are considered global. For example:

- Header
- Navbar
- Custom form components (very useful!)
- Footer

## Tips

As your application grows in complexity it is worth taking the time to consider **where** components and helper functions should live in your app. _It doesn't really matter_ as long as you are **consistent!**

If you are working in a team, it is essential you all ensure you are following the same principals in terms of folder structure and naming conventions.

**Remember: if you are not sure, don't be afraid to ask!**
