# jQuery

jQuery is a 3rd-party library that makes tasks like DOM traversal and manipulation, event handling, animation, and Ajax much simpler with an easy-to-use API that works across a multitude of browsers.

## What is a library?

A **library** is just a collection of code, predominantly reusable methods, that serve a particular purpose.

## What does jQuery do?

* jQuery helps us manipulate the DOM, allowing us to perform complex manipulations in less code with less hassle
* jQuery's syntax was developed to mimic CSS selector syntax, making code easier to develop, read, and manage
* The syntax is shorter, and we're lazy!
* jQuery deals with many cross-browser compatibility issues for us

### jQuery is just Javascript

Many people think of jQuery as being different to JavaScript however, it is just a library written in JavaScript containing useful methods and logic.

## Installation

jQuery is a client side library, which means we need to include it in our HTML. To do this, we have two options:

### Using jQuery's server

Directly from [jQuery's website](http://code.jquery.com/):

```html
<script src="http://code.jquery.com/jquery-1.11.1.min.js"></script>
```

### Using a public CDN

```html
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
```

### Downloading a copy into our project:

[CDNJS](http://www.cdnjs.com), [Google Hosted Libraries](https://developers.google.com/speed/libraries/), and the [jQuery site](http://www.jquery.com) will all allow you to download a copy of jQuery to include in your projects.

## Versions

If you've visited the [jQuery website](http://code.jquery.com), you'll see that there are three major versions of the library.

- **1.x** is the most cross-browser-compatible version of the jQuery core.
- **2.x** does not support for IE 6-8, which allowed for performance improvements
- **3.x** is the most up-to-date version of the library with extended features

## `$`

Before we get started with jQuery, we should have a look at the `$`.

The `$` is nothing but an alias for the jQuery library.

If we have a look at the jQuery code on [Google's CDN](https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.js) we can see at the bottom there is the code:

```
window.jQuery = window.$ = jQuery
```

Essentially by using either `$` or `jQuery` we can access the methods in the jQuery library.

## Using jQuery

The main goal of jQuery is to give us shorthand methods for vanilla JavaScript. Let's look at a few of the common ones:

### `DOMContentLoaded`

We can replace the `DOMContentLoaded` event listener in jQuery with:

```js
$(document).ready(() => {
  ...
})
```

And there's even a shorthand for that:

```js
$(() => {
  ...
})
```

### Selecting elements

There are several ways of selecting elements with pure JS:

```js
document.getElementById('heading')
document.getElememtsByClassName('buttons')
document.getElementsByTagName('div')
document.querySelector('#header .list')
document.querySelectorAll('.italic')
```

With jQuery the process has been significantly simplified:

```js
$('#heading')
$('.buttons')
$('div')
$('#header .list')
$('.italic')
```

The syntax is the simplest in jQuery: `$(selector)`. It is the equivalent of `.querySelectorAll` in vanilla JS, but requires hardly any typing whatsoever!

#### Variable assignment and selection

If you use variable assignment when doing a selection, a [`jQuery object`](https://learn.jquery.com/using-jquery-core/jquery-object/) is returned.

**Note**: Normal DOM elements have methods and properties, a jQuery object just adds a few more methods and properties to the selected object.

We prepend `$` to variable names when a variable is going to be a jQuery object to help us remember that the variable contains a jQuery object:

```js
const $paragraphs = $('p')
```

**We ONLY do this for variables that will contain jQuery objects**. If a variable is going to contain a number for example, we would not prepend it with a `$`:

```js
const $paragraphs = $('p') // does contain a jQuery object
let total = 0 // does not contain a jQuery object
```

### Event listeners

Instead of using the `addEventListener` method, with jQuery we simply use the `.on()` method:

```js
.on(nameOfEvent, callback)
```

So an example of an event listener to watch for a `submit` event on a form would be:

```js
$form.on('submit', (e) => {
  e.preventDefault()
  // do something ...
})
```

### Assigning event handlers to multiple elements

With vanilla JavaScript we have to loop over collections of elements and assign event listeners to each of them in turn:

```js
for(let i=0, len=buttons.lengthi<leni++) {
  buttons[i].addEventListener('click', (e) => {
    // do something here...
  })
}
```

However with jQuery you can add an event listener to a collection without the need for a loop at all:

```js
$buttons.on('click', (e) => {
  // do something here...
})
```

### Modifying the content of an element

Vanilla JavaScript has two main ways of updating the content of an element:

```js
element.innerHTML = '<h1>New HTML Content</h1>'
element.textContent = 'New text content, (HTML elements not supported)'
```

The equivalent in jQuery is the much simpler `.html` and `.text` methods.

To change the content, we pass an argument:

```js
$element.html('<h1>New HTML Content</h1>')
$element.text('New text content, (HTML elements not supported)')
```

And to get the current content, we pass no argument:

```js
$element.html() // => '<h1>New HTML Content</h1>'
$element.text() // => 'New text content, (HTML elements no supported)'
```

### Changing CSS

Vanilla JavaScript uses the following syntax to modify the CSS of an element:

```js
element.style.propertyName = value
```

With jQuery there are two different ways:

```js
$element.css(propertyName, value)
$element.css({ propertyName: value })
```

If we wanted to set the background colour of an element, we could do it like so:

```js
$element.css('background-color', 'red')
$element.css({ backgroundColor: 'red' })
```

Notice that when we use an object, we have to make the property names camel case, but when we use a string, we use the normal CSS property name.

If we want to get the value of a CSS property we just need to pass one argument to the method:

```js
$element.css('background-color') // => "red"
```

## Further reading

- [jQuery - Wikipedia](https://en.wikipedia.org/wiki/JQuery)
- [jQuery Documentation](http://api.jquery.com/)
- [You Might Not Need jQuery](http://youmightnotneedjquery.com/)
