# Functional vs Classical Components

There are two types of component that we can make in a React App: **Functional** and **Classical**.

Generally speaking you should use the **functional** style, unless you need:

- Internal state
- Lifecycle hooks
- AJAX requests or complex logic

## Functional

```js
const MyComponent = (props) => {
  return (
  <h1 className={props.className}>Hello World!</h1>
  )
}
```

- Simple
- Stateless
- Presentational
- Preferred
- Easy to test
- Returns JSX

Functional components are often referred to as **dumb components** because they are purely presentational and should not contain any logic.

They accept `props` from a parent component, and have no internal state.

They are simple to test because they have no logic of their own. They will always generate the same output if the `props` they receive are the same.

## Classical

```js
class MyComponent extends React.Component {
  render() {
    return (
      <h1 className="header">Hello World!</h1>
    )
  }
}
```

- More complex
- Stateful
- Contains Logic
- Has lifecycle hooks
- Harder to test
- Needs a render method with returns JSX

Classical components are also referred to as **smart components**. They typically contain all the logic that the functional components need to function.

Classical components have `lifecycle hooks` which allow for logic to be performed at certain times during the component's life.

The following lifecycle hooks are available:

#### Mounting (rendering)
- `componentWillMount()`
- `render()`
- `componentDidMount()`

#### Updating
- `componentWillReceiveProps()`
- `shouldComponentUpdate()`
- `componentWillUpdate()`
- `render()`
- `componentDidUpdate()`

#### Unmounting (destroying)
- `componentWillUnmount()`

> **Note:** The most common hook is `componentDidMount()`.

Classical components are harder to test because they generally contain complex logic which can be run at different times during their lifecycle. They could also make AJAX requests and handle DOM events like `click` and `submit`.

## A simple example

Ok, let's look at how we can use both classical and functional components together. We're going to take some static data stored in a classical component's `state` and present it using a functional component.

We'll also look at some new ES6 features along the way.

### Adding some data

Open up the starter code. We'll make a start by adding some state to the `App` component:

```js
class App extends React.Component {
  constructor() {
    super();

    this.state = {
      counties: [{
        name: 'Greater London',
        population: 8778500,
        borderedBy: [
          'Essex',
          'Kent',
          'Buckinghamshire',
          'Berkshire',
          'Hertfordshire',
          'Surrey'
        ]
      }, {
        name: 'Essex',
        population: 1802200,
        borderedBy: [
          'Greater London',
          'Hertfordshire',
          'Kent',
          'Suffolk',
          'Cambridgeshire'
        ]
      }, {
        name: 'Hertfordshire',
        population: 1176700,
        borderedBy: [
          'Essex',
          'Greater London',
          'Cambridgeshire',
          'Bedfordshire',
          'Buckinghamshire',
          'Berkshire',
          'Surrey'
        ]
      }]
    };

  }

  render() {
    .
    .
    .
  }
}
```

Ok great, now we have some data to work with we can begin to flesh out the app. We're going to make a dumb `County` component to display the data for each county.

```sh
mkdir src/components && touch src/components/County.js
```
```js
import React from 'react';

const County = () => {
  return (
    <div>
    	County data goes here...
    </div>
  );
}

export default County;
```

Ok, let's import it into `app.js`:

```js
import React from 'react';
import ReactDOM from 'react-dom';

import County from './components/County';

class App extends React.Component {
  constructor() {
    super();

    this.state = {
      counties: [{ ... }, { ... }, { ... }]
    };

  }

  render() {
    return (
      <div>
        {this.state.counties.map((county, i) => <County key={i} {...county} />)}
      </div>
    );
  }
}
```

Woah there, what's `.map()` doing, what's this `key={i}` and what's this `{...county}` nonsense??

### Looping in React

#### `.map()`

The `.map()` method creates a new array from an existing array. It's a bit like `.forEach()` but it returns an array the same size as the existing array with the contents specified in the callback function.

It in this case we have created an array of three `County` components, one for each county object in `this.state`.

So effectively this:

```js
<div>
  {this.state.counties.map((county, i) => <County key={i} {...county} />)}
</div>
```

becomes this:

```js
<div>
  [
    <County key={0} {...county} />,
    <County key={1} {...county} />,
    <County key={2} {...county} />
  ]
</div>
```

You will see this a lot in React development.

#### `key`

The `key` prop is required by React when rendering components in a loop. If the underlying data changes React wants an easy way to know which components need to be changed or removed. Placing a unique identifier (like an index) in the `key` prop will do the job nicely.

In fact if you don't supply a `key`, you'll get the following warning:

```
Warning: Each child in an array or iterator should have a unique "key" prop.
```

#### `{...county}`

This is the ES6 `spread` operator in action. It is a nice little short-cut for doing this:

```js
<County key={2} name={county.name} population={county.population} borderedBy={county.borderedBy} />
```

Of course we could have simply done this:

```js
<County key={2} county={county} />
```

But it's important we get used to using the `spread` operator as it is used extensively by the React community.

### Displaying the data inside the `County` component

We should now see "County data goes here..." three times on the screen. Let's update the `County` component to handle the data being passed in via `props`:

```js
import React from 'react';

const County = (props) => {
  return (
    <div>
    	<h1>{props.name}</h1>
    	<p>Population: <strong>{props.population}</strong></p>
    	<p>Bordered by:</p>
    	<ul>
    	  {props.borderedBy.map((county, i) => <li key={i}>{county}</li>)}
    	</ul>
    </div>
  );
}

export default County;
```

We should now see the information of all three counties displayed on the screen.

Notice that we have used `.map()` again to loop over the `borderedBy` array, which has required us to use the `key` prop on the `<li>` tag.

The data that we passed into the component with our `spread` operator has been attached to the `props` argument. Using ES6 destructuring we can refactor our code slightly:

```js
import React from 'react';

const County = ({ name, population, borderedBy }) => {
  return (
    <div>
    	<h1>{name}</h1>
    	<p>Population: <strong>{population}</strong></p>
    	<p>Bordered by:</p>
    	<ul>
    	  {borderedBy.map((county, i) => <li key={i}>{county}</li>)}
    	</ul>
    </div>
  );
}

export default County;
```

Here we've pulled the properties from the `props` object inside the argument part of the arrow function declaration, creating standalone variables `name`, `population` and `borderedBy`.

## Conclusion

Generally speaking we want to split our applications into smaller components. Classical or _smart_ components are use to hold `state` and business logic. Functional or _dumb_ components should be used to display data only.

Because functional components are more simple and reusable they require less computation, and are easy to test, so you should always favour them over classical components.

However if you need to store data on `state` or need to perform complex logic or AJAX requests, you must use a classical component.


## Further reading

- [State and Lifecycle - React](https://facebook.github.io/react/docs/state-and-lifecycle.html)
- [React.Component - React](https://facebook.github.io/react/docs/react-component.html)
