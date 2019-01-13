# Intro to React

## A little history

React was created by Jordan Walke, a developer at Facebook. He was influenced by XHP, an HTML framework for PHP.

It was first used on Facebook in 2011 for displaying the newsfeed. It was later used on Instagram in 2012.

The product was released to the public as an open-source project in 2013, at the JSConf in Florida.

React now has a huge community around the world and lots of resources online. **However it is worth noting that React is new and is being updated frequently.**

React utilises a number of ES6 and ESNext features, which require transpilation with Babel before being compiled by the browser. This means there is a reasonable amount of setup before we can begin to develop with it.

React is also very different to other JS frameworks, which means that the learning curve can be a little steep at first. However once you get over the initial bump you should find it to be very useful and fun!

## How is React different

React describes itself as **A JavaScript library for building user interfaces**.

It is very new and very hot in the industry right now. It has shaken up the way that developers approach frontend web development and has heavily influenced other JS frameworks like Angular and VueJS.

### Components

The main thing that set it apart from other JS frameworks is that it is _component-based_.

This component paradigm makes it easier for developers to reuse useful widgets through their sites. Each component contains all the logic, HTML markup, and styling in one self-contained bundle.

### Unidirectional Data Flow

In React data can only flow in one direction. As the underlying data changes the view will update, but in order for the view layer to affect the data, an action must be triggered. There is no _automatic updating_. This helps to reduce overhead and makes managing data simpler.

A component can manage its own data (known as `state`), or can have data passed to it from another component (known as `props`). When either `state` or `props` changes React will re-render the component which in turn updates the view.

### Virtual DOM

React employs a _Virtual DOM_. It has a copy of the DOM in memory at all times. When the view needs to be updated React will update this Virtual DOM first. This is easy to do, because it is already in memory.

Once the Virtual DOM has been updated React checks to see what the differences are between the Virtual DOM and the actual DOM. React can then just update the DOM nodes that have changed.

This approach is unique to React and is what made it so much quicker at rendering than any of its rivals.

### JSX

`JSX` is a syntax for easily creating HTML elements with React. It is not a language in itself, merely syntactic sugar around React's `createElement` method.

Basically `JSX` can be used to write `HTML` markup directly in our JavaScript source code. It feels a little weird at first, but after a while become second nature.

In JavaScript we can create HTML nodes with the `document.createElement()` method. React has a similar method, which creates an element in the Virtual DOM.

Here's an example of creating a `<h1>` tag with React's `createElement` method:

```js
React.createElement('h1', { className: 'header' }, 'Hello World!')
```

This creates the following HTML in the Virtual DOM:

```html
<h1 class="header">Hello World!</h1>
```

Instead of having to write `React.createElement()`, we can use JSX as a shorthand for the same method:

```jsx
<h1 className="header">Hello World!</h1>
```

It looks just like HTML, but it's not, it's actually JavaScript, and because of that we can't use the property `class`, as that is a reserved word in JavaScript. Instead, we use `className`.

### React Native

Facebook has also released React Native, which allows React developers to easily create native apps for iOS and Android. Although we wont be looking at React Native on the course it is noteworthy as it allows frontend developers a new opportunity for creating products that would previously required a huge investment of time and money. Just another reason why React is changing the way we work in the industry.

### Webpack

>**Note**: **Webpack is not part of React**, but they are often used together.

Because React needs to be transpiled from ES6 to ES5, and JSX to ES5 it has to undergo some sort of automated task. There are many tools that can do this, but the community tends to favour Webpack. It's simple to use and reasonably easy to set up.

Webpack parses the source code and identifies JS, JSX, CSS and assets and handles them appropriately based on their file extension. That allows us to do some nutty things like _import styles into JavaScript files!_

## Putting it all together: A Simple React App

>**Note:** This section requires a webpack config and some npm packages to be installed. Your instructor will be able to help you with this.

### The HTML boilerplate

React needs very minimal HTML to run. Unlike Angular we just need a single `<div>` element with an id:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My First React App</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

You'll notice there is no script tag here. Webpack will inject that for us.

### The main JavaScript file: `app.js`

Webpack is set up to look for the file `src/app.js` as the main JavaScript file that will load up (or bootstrap) our app. Inside we need to tell React which HTML element to inject all of the HTML that we will generate with our app.

#### ES6 syntax

We can write React with ES5, but its easier to use ES6, and since we are using JSX anyway, we have to transpile our code either way.

With ES5 we import files like this:

```js
const React = require('react')
```

With ES6 we do it like this:

```js
import React from 'react'
```

We need to import `react` and `react-dom` whenever we are working with web projects. `react` is the core library, and `react-dom` is for hooking up to a browser's DOM.

> **Note:** If we were doing an iOS or Android project we would use `react-native` instead.

We can then make a simple component called `App`:

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
```

ES6 introduced JavaScript classes. They are almost identical to Ruby classes, and serve the same function. Ultimately they are transpiled to ES5 constructor functions but the syntax is more in keeping with almost every other programming language!

### Connecting the component to the DOM

Finally we need to connect our `App` component to the DOM:

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

### Starting up the app

We can use Webpack's Dev Server to transpile our code and watch our files for changes _on the fly_. To start the process we could type `webpack-dev-server` in the terminal. However that's a bit long-winded and easy to misspell. Instead in the `package.json` we can add a `scripts` section:

```json
"scripts": {
  "start": "webpack-dev-server"
}
```

This means we can simply type `yarn start` or `npm start` to start the `webpack-dev-server` process running. You should now see the app in all its glory on  `http://localhost:8000`. Any modifications you make to the source code, will automatically update in the browser.

## Conclusion

Its seems like a lot of work for very little pay off at the moment, but you'll soon start to see how powerful React is, and how it allows you to very quickly build out complex applications.

The syntax may feel a little uncomfortable as first but actually, the more you come to use it, the more you will see how similar React is to vanilla JavaScript!

React is not suitable for all projects, but it is extremely popular right now and there are plenty of jobs out there for React developers and with its backing from Facebook, it is likely to be supported for a good while.

## Further Reading

- [React Homepage](https://facebook.github.io/react/)
- [Hello World - React](https://facebook.github.io/react/docs/hello-world.html)
- [Setup a React Environment Using webpack and Babel](https://scotch.io/tutorials/setup-a-react-environment-using-webpack-and-babel)
