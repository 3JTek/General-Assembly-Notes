# React Routing

React doesn't come with a router built-in so its up to you to decide how to route your app.

Your main choices are:

- Roll you own
- Use [React Router](https://reacttraining.com/react-router/)
- Use another 3rd-party router

Generally developers use React Router unless they have a good reason not to. It's well documented and there is plenty of help with it online.

One small caveat though: there have been a number of iterations of React Router. It is currently v4, which is very different to v2 and v3. So make sure you know which version you are using.

## Installation

Install with npm or yarn:

```sh
yarn add react-router-dom
```

>**Note**: React Router has a separate package for React Native: `react-router-native`. Both packages are dependent on `react-router` which is installed automatically.

There are three main components that you will want to install:

- `<BrowserRouter />`: the main parent component
- `<Route />`: An individual route or 'page'
- `<Link />`: A helper component for changing URL
- `<Switch />`: A helper component for managing ambiguous URLs

Import them into your project like so:

```js
import { BrowserRouter, Route, Link, Switch } from 'react-router-dom';
```

## A Simple Example

>**Note**: The `Router` component has to have only one JSX element, so you always need to wrap your routes in a `<div>`.

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter, Route, Link } from 'react-router-dom';

class App extends React.Component {

  render() {
    return (
      <BrowserRouter>
        <div>
          <nav>
            <Link to="/">Home</Link>
            <Link to="/about">About</Link>
          </nav>

          <Route exact path="/" component={Home} />
          <Route path="/about" component={About} />
        </div>
      </BrowserRouter>
    )
  }
}

const Home = () => {
  return (
    <h1>Home Page</h1>
  );
}

const About = () => {
  return (
    <h1>About Page</h1>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

## Further Reading

- [React Router v4 Docs](https://reacttraining.com/react-router/web/example/basic)
- [A Simple React Router v4 Tutorial](https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf)
