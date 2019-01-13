# Functional vs Classical Components

There are two types of component that we can make in a React App: **Functional** and **Classical**.

Generally speaking you should use the **functional** style, unless you need:

* Internal state
* Lifecycle hooks
* AJAX requests or complex logic

## Functional

```js
const MyComponent = (props) => {
  return (
    <h1 className={props.className}>Hello World!</h1>
  )
}
```

* Simple
* Stateless
* Presentational
* Preferred
* Easy to test
* Returns JSX

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

* More complex
* Stateful
* Contains Logic
* Has lifecycle hooks
* Harder to test
* Needs a render method which returns JSX

Classical components are also referred to as **smart components**. They typically contain all the logic that the functional components need to function.

Classical components have `lifecycle hooks` which allow for logic to be performed at certain times during the component's life.

The following lifecycle hooks are available:

#### Mounting (rendering)
* `componentWillMount()`
* `render()`
* `componentDidMount()`

#### Updating
* `shouldComponentUpdate()`
* `render()`
* `componentDidUpdate()`

#### Unmounting (destroying)
* `componentWillUnmount()`

> **Note:** The most common hook is `componentDidMount()`.

Classical components are harder to test because they generally contain complex logic which can be run at different times during their lifecycle. They could also make AJAX requests and handle DOM events like `click` and `submit`.

## Which should you use

Generally speaking we want to split our applications into smaller components. Classical or _smart_ components are use to hold `state` and business logic. Functional or _dumb_ components should be used to display data only.

Because functional components are more simple and reusable they require less computation, and are easy to test, so you should always favour them over classical components.

However if you need to store data on `state` or need to perform complex logic or AJAX requests, you must use a classical component.

## Further reading

- [State and Lifecycle - React](https://facebook.github.io/react/docs/state-and-lifecycle.html)
- [React.Component - React](https://facebook.github.io/react/docs/react-component.html)
