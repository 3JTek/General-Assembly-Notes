# AJAX with React

There are a few different ways of making AJAX requests with React:

- **[Vanilla JavaScript](http://mdn.beonex.com/en/DOM/XMLHttpRequest/Using_XMLHttpRequest.html)**: light but very long-winded!
- **[jQuery](http://api.jquery.com/jquery.ajax/)**: yes you can use React and jQuery together, but you probably shouldn't!
- **[fetch](https://github.github.io/fetch/)**: an AJAX library that is in-built in Chrome and Firefox, which may become a part of HTML spec soon
- **[axios](https://github.com/mzabriskie/axios)**: promise-based AJAX requests similar to jQuery, but without the added bloat of all the other jQuery functionality
- **[SuperAgent](https://visionmedia.github.io/superagent/)** similar to jQuery and Axios but without promises.

Ultimately it's up to you, but its worth noting that React does not have its own AJAX library (unlike Angular with its `$http` module).

The two most popular are `fetch` and `axios`. Here's a quick summary of both of them:

## `fetch`

`fetch()` allows you to make network requests similar to XMLHttpRequest (XHR). The main difference is that the Fetch API uses Promises, which enables a simpler and cleaner API, avoiding callback hell and having to remember the complex API of XMLHttpRequest.

We don't need to install the Fetch API as it is in-built in Chrome and Firefox.

| Pros | Cons |
|:-----|:-----|
| Native to Chrome and Firefox | Clunky syntax |
| No installation | Can only handle strings so requires the developer to use `JSON.stringify` before sending data |
| Will likely become standard in all browsers | Two-stage process to get the JSON payload in the response |
| Uses promises | Requires the API handle `URLencoded` data |


#### Example POST request

```js
fetch('/api/cats', {
  method: 'POST',
  body: JSON.stringify(this.state.cat)
})
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.log(err));
```

## `axios`

Like Fetch, Axios is a promise-based HTTP client that works both in the browser and in a node.js environment. It basically provides a single API for dealing with XMLHttpRequests and node’s http interface.

There are some benefits of using Axios over Fetch. It has a syntax that is slightly simpler - we don't need the first `.then()` block that turns the response into JSON, and we don't need to stringify data before sending it as part of a POST request. Axios is also supported on all browsers, whereas Fetch is not yet standard in all browsers (check out Fetch browser support [here](http://caniuse.com/#feat=fetch)).

| Pros | Cons |
|:-----|:-----|
| Has a familiar jQuery-esq syntax | Requires installation |
| Simple to use | Only requires the API to handle `JSON` |
| Lightweight compared to jQuery |

#### Example POST request

```js
axios.post('/api/cats', this.state.cat)
  .then(res => console.log(res.data))
  .catch(err => console.log(err));
```


## When to use AJAX in a React app

If you want to load data when the component renders, you should use the `componentDidMount()` lifecycle hook:

```js
class Cats extends React.Component {

  constructor() {
    super();
    this.state = { cats: [] };
  }

  componentDidMount() {
    axios.get('/api/cats', this.state.cat)
      .then(data => this.setState({ cats: res.data }))
      .catch(err => console.log(err));
  }
}
```

You can also add an AJAX request to a event handler with `onClick` or `onSubmit`:

```js
class Cats extends React.Component {

  constructor() {
    super();
    this.state = { cats: [] };

    this.handleSubmit.bind(this);
  }

  handleSubmit() {
    axios.post('/api/cats', this.state.cat)
      .then(res => console.log(res.data))
      .catch(err => console.log(err));
  }
}
```

## Further Reading

- [AJAX Requests in React: How and Where to Fetch Data](https://daveceddia.com/ajax-requests-in-react/)
- [Fetch API - David Walsh](https://davidwalsh.name/fetch)
