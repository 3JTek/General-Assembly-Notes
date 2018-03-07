# AJAX

AJAX stands for **Asynchronous Javascript And XML** and is a way for us to make HTTP requests directly inside our JavaScript files.

This is really useful for us because it allows us to pull the data from an API directly into our JavaScript and then render that data into the view with jQuery, Angular or React.

## XML and JSON

Back in the day, APIs used to send data in XML format, which is where the X in AJAX comes from. XML is similar to HTML. A cheese represented in XML might look like this:

```xml
<cheese>
  <name>Gouda</name>
  <origin>Netherlands</origin>
  <strength>4</strength>
</cheese>
```

While this was perfectly fine, but because each property of the cheese required an opening _and_ closing tag, the amount of data being transferred was rather large.

As APIs were being consumed more and more by JavaScript applications a more suitable way of transferring data with an API was born: JSON.

JSON stands for **JavaScript Object Notation** and is a string representation of JavaScript objects and arrays. The same cheese represented in JSON might look like this:

```json
{
  "name": "Gouda",
  "origin": "Netherlands",
  "strength": 4
}
```

As you can see, it looks almost identical to a JavaScript object, except that the keys have double quotes and the string values also have double quotes.

The APIs we build on this course will use JSON exclusively.

## Using AJAX

Depending on the library we are using we can make any request (including GET, POST, PUT, PATCH and DELETE)

### jQuery

jQuery has an `ajax` method, and `get` and `post` helper methods:

```js
$.ajax({
  url: '/cheeses',
  method: 'GET' // could be any HTTP verb
})
  .done(data => console.log(data))
  .fail(err => console.log(err))
  .always(() => console.log('what\'s wrong with .then, .catch & .finally??'));

// same as above
$.get('/cheeses')
  .done(data => console.log(data));
```

We can also send data with the request using the `data` property. However, if we want to send JSON, we need to convert the data into JSON first, using `JSON.stringify`, and set the `dataType` and `contentType` properties:

```js
$.ajax({
  method: 'POST',
  url: '/cheeses',
  data: JSON.stringify({ name: 'Gouda', origin: 'Netherlands', strength: 2 }),
  dataType: 'json',
  contentType: 'application/json'
})
  .done(cheese => console.log(cheese))
  .fail(err => console.log(err));
```

### $http

$http is Angular's build in AJAX library.

$http is almost identical to jQuery, except it's got a few more helper functions:  `put`, `patch` and `delete`:

```js
$http({
  url: '/cheeses',
  method: 'GET' // could be any HTTP verb
})
  .then(res => console.log(res.data)) // data is stored on a `data` property of the response
  .catch(err => console.log(err))
  .finally(() => console.log('ooh much nicer'));

// same as above
$http.get('/cheeses')
  .then(res => console.log(res.data));
```

As with jQuery we can send data with our requests, but we do not need to manually convert it to JSON, `$http` will do that for us:

```js
$http({
  method: 'POST',
  url: '/cheeses',
  data: { name: 'Gouda', origin: 'Netherlands', strength: 2 }
})
  .then(res => console.log(res.data))
  .catch(err => console.log(err));

// same as above
$http.post('/cheeses', { name: 'Gouda', origin: 'Netherlands', strength: 2 })
  .then(res => console.log(res.data));
```

## Axios

Another 3rd-party AJAX library is Axios, which we use with React. The syntax is identical to $http:

```js
axios({
  url: '/cheeses',
  method: 'GET' // could be any HTTP verb
})
  .then(res => console.log(res.data))
  .catch(err => console.log(err))
  .finally(() => console.log('ooh much nicer'));

axios.get('/cheeses')
  .then(res => console.log(res.data));

axios({
  method: 'POST',
  url: '/cheeses',
  data: { name: 'Gouda', origin: 'Netherlands', strength: 2 }
})
  .then(res => console.log(res.data))
  .catch(err => console.log(err));

// same as above
axios.post('/cheeses', { name: 'Gouda', origin: 'Netherlands', strength: 2 })
  .then(res => console.log(res.data));
```

## Further reading

- [AJAX](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX)
- [Understanding AJAX as a Beginner Web Developer](https://www.codementor.io/sheena/ajax-tutorial-web-development-du107rzaq)
- [JavaScript and AJAX tutorial: What is AJAX?](https://www.youtube.com/watch?v=RDo3hBL1rfA)
