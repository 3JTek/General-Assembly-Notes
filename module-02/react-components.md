# React Components

Everything in React is a component. When we think about creating an app in React, we first need to establish which components we need to make.

Components should be reusable. A component might be used stand-alone or as part of another larger component.

Consider this shopping cart. It is made up of 5+ components, which work together to make the app work.

![Everything in React is a component](https://s3-us-west-2.amazonaws.com/techdojo-web/blog/react+table+2.png)

## Creating a component in React

There are a couple of different ways of creating a component in React. We'll look at the most common way, which uses the new ES6 `class` syntax:

```js
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  render() {
    return (
      <h1>Hello World!</h1>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

As you can see a component is simply a class which defines a set of methods and properties that describes how React should handle the component's UI.

A React component must have **at least** a `render` method. If not you will see the following error:

```
Warning: App(...): No `render` method found on the returned component instance: you may have forgotten to define `render`
```

Every component should `render` something to the DOM. A component should contain all the logic that it needs inside of itself. It can then be connected to other components to build up complex applications.

You can think of it like the human body. The lungs know only how to pass oxygen to the blood and carbon dioxide to the oesophagus. The heart knows only how to pump blood around the body. Hooking them together allows the body to deliver oxygen to all the muscles.

## The data layer

Every component in a React app at some point needs to display something to the user in the form of HTML rendered to the DOM. Exactly what they display usually depends on some data.

It might be an array of birds, or a user's data in the form of an object. Whatever it is, a React component handles this data in the form of `state` or `props`.

### `state`

`state` is data that lives inside a component. We can add state to our `App` component like so:

```js
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  constructor() {
    super()

    this.state = { message: 'Hello World!' }
  }

  render() {
    return (
      <h1>{this.state.message}</h1>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

This component is now displaying data that is being stored on the component's `state`.

#### `constructor()` & `super()`

In order to create this state object on our component we need to add it when the component is _instantiated_ or created. Remember that a class in JavaScript is constructor function. When we instruct React to use this component it will first create an instance of it. Something like this:

```js
const app = new App()
```

Although because we use JSX, what we _actually_ write is this:

```js
ReactDOM.render(
  <App />, // new App() is called here in the background
  document.getElementById('root')
)
```

When this happens the `constructor()` method is called, which then creates our state object.

The component `extends` the `React.Component` class. The `React.Component` class contains a lot of extra functionality that allows our class to behave like a component. Calling `super()` will call the `constructor()` function in the parent class, ensuring that any logic there is run.

### `props`

We can pass data into a component using `props`. Let's update our `App` component to allow it to receive data through props:

```js
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  constructor(props) {
    super(props)

    this.state = { message: 'Hello World!' }
  }

  render() {
    return (
      <h1>{this.state.message}</h1>
      <h2>Welcome, {this.props.name}!</h2>
    )
  }
}

ReactDOM.render(
  <App name="Mike" />,
  document.getElementById('root')
)
```

If we look at the browser we'll see an error message:

```
Error in ./src/index.js Syntax error: Adjacent JSX elements must be wrapped in an enclosing tag
```

React is complaining because we have adjacent JSX tags, basically React can only render a single element, with child elements nested inside. Let's wrap everything in a `<header>` element:

```js
class App extends React.Component {
  constructor(props) {
    super(props)

    this.state = { message: 'Hello World!' }
  }

  render() {
    return (
      <header>
        <h1>{this.state.message}</h1>
        <h2>Welcome, {this.props.name}!</h2>
      </header>
    )
  }
}

ReactDOM.render(
  <App name="Mike" />,
  document.getElementById('root')
)
```

We have added a `name` property to the `<App />` component in the `ReactDOM.render` method. It looks like an attribute on an HTML element eg: `<input name="Mike" />` where the attribute is `name`.

When we add an attribute to a React component, we refer to it as a `prop`, and the data can be accessed inside the component via `this.props`.

Notice that we have had to update out `constructor` and `super` methods. The `constructor` method gets passed the props by React. In the background React is converting the attributes on the JSX to an object:

```js
<App name="Mike" />
// identical to
new App({ name: 'Mike' })
```

When either `props` or `state` changes, React will re-render the component, updating the view.

![Changing state or props updates a react component](https://discoversdkcdn.azureedge.net/postscontent/react%20native/how%20it%20works/image5.png)

## Nesting Components

Ok, let's look at how we can now combine components to create a slightly more complex application.

We'll create two new components `Header` and `Footer` and nest them inside our `App` component.

#### Header

```js
import React from 'react'

class Header extends React.Component {
  render() {
    return (
      <header>
        <h1>{this.props.message}</h1>
        <h2>Welcome, {this.props.name}!</h2>
      </header>
    )
  }
}

export default Header
```

We can now import it in our `src/index.js` file and nest it inside `App`:

```js
import React from 'react'
import ReactDOM from 'react-dom'

import Header from './components/Header'

class App extends React.Component {
  constructor(props) {
    super(props)

    this.state = { message: 'Hello World!' }
  }

  render() {
    return (
      <Header name="Mike" message={this.state.message}/>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

See how we have passed the message from our `App` component's `state` to the `Header` component via props.

#### Footer

```js
import React from 'react'

class Footer extends React.Component {  
  render() {
    return (
      <footer>
        <p>Made with &hearts; at GA London</p>
      </footer>
    )
  }
}

export default Footer
```

Again, let's import this in our `src/index.js` file and add it to our `App` component's `render` method:

```js
import React from 'react'
import ReactDOM from 'react-dom'

import Header from './components/Header'
import Footer from './components/Footer'

class App extends React.Component {

  constructor(props) {
    super(props)

    this.state = { message: 'Hello World!' }
  }

  render() {
    <Header name="Mike" message={this.state.message} />
    <Footer />
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

If we look at the browser we'll see an error message:

```
Error in ./src/index.js Syntax error: Adjacent JSX elements must be wrapped in an enclosing tag
```

This is the same error as we saw earlier. React is complaining because we have adjacent JSX tags, so let's wrap everything in a `main` element:

```js
class App extends React.Component {

  constructor(props) {
    super(props)

    this.state = { message: 'Hello World!' }
  }

  render() {
    <main>
      <Header name="Mike" message={this.state.message} />
      <Footer />
    </main>
  }
}
```

#### `children`

Finally we can nest components by placing them in-between the component's opening and closing tags. Let's create a third component which will simply display an image and caption:

```js
import React from 'react'

class Hero extends React.Component {  
  render() {
    return (
      <section className="hero">
        <img src="https://laracasts.com/images/series/squares/do-you-react.jpg" alt="React" />
        <p>A JAVASCRIPT LIBRARY FOR BUILDING USER INTERFACES</p>
      </section>
    )
  }
}

export default Hero
```

Again let's import our component but this time we'll include it inside the `<App />` JSX element:

```js
import React from 'react'
import ReactDOM from 'react-dom'

import Header from './components/Header'
import Footer from './components/Footer'
import Hero from './components/Hero'

class App extends React.Component {
.
.
.
}

ReactDOM.render(
  <App>
    <Hero />
  </App>,
  document.getElementById('root')
)
```

The `Hero` component has actually been passed into the `App` component through `props`, and stored on a prop called `children`. Anything between the opening and closing tags of a component will be passed to the component as `props.children`.

In order to render it we need to add it into the `render()` method of `App`:

```js
render() {
  <main>
    <Header name="Mike" message={this.state.message} />
    {this.props.children}
    <Footer />
  </main>
}
```

It's important to note that it is difficult to pass data from `App` to `Hero` in this way. Although it is possible to pass data from the `state` in `App` to `Hero` via props using this method, it is complex and requires advanced React techniques. Generally speaking `this.props.children` should only be used with components and DOM elements that do not need to receive props themselves.

## Further reading

* [React JS Tutorial - Nesting Components #6](https://www.youtube.com/watch?v=7VOko6eXb8s)
* [A quick intro to React's props.children](https://codeburst.io/a-quick-intro-to-reacts-props-children-cb3d2fce4891)
* [Thinking In React - ReactJS](https://reactjs.org/docs/thinking-in-react.html)
