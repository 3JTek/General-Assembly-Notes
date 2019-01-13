# The DOM

The _Document Object Model_, commonly referred to as the DOM, is an interactive representation of HTML as a collection of JavaScript objects.

Each element in the HTML is converted into a node of the DOM (like a branch on a tree), with its own specific properties and methods. A `form` element will have an `onsubmit` method, and a `button` an `onclick` method, for example.

Each element is linked to its parent via properties and methods. If we were to draw a diagram of the DOM, it might look a little something like this:

![](https://cloud.githubusercontent.com/assets/40461/8267269/558bf840-1751-11e5-8127-12c6e5c34041.png)

## Interacting with the DOM

When a browser reads an HTML file, it creates the DOM, converting each element into an object. Once the DOM has been created we can access any element with JavaScript. We can store the element in a variable, and then modify its properties and methods, just like we can with a standard JavaScript object.

The DOM is represented in JavaScript with the `document` keyword. We use special methods of the `document` object to access specific nodes of the DOM.

If we had the following HTML:

```html
<body>
  <h1 id="header" class="header">I am a header</h1>
</body>
```

We can access the `h1` in our JavaScript file in the following ways:

```js
// The following methods ALWAYS return a single object
const header = document.getElementById('header') // targets an element by its id
const header = document.querySelector('#header') // uses CSS selector syntax

// The following methods ALWAYS return an array of objects, so we use [0] to get the first one
const header = document.getElementsByTagName('h1')[0] // targets elements by their tag
const header = document.getElementsByClassName('header')[0] // targets elements by the class
const header = document.querySelectorAll('.header')[0] // uses CSS selector syntax
```

Once we have an element stored in a variable we can modify it, which will instantly update the DOM, and in turn the web page.

If we wanted to change the `h1` content from "I am a header" to "JavaScript Rocks!" for example, we could modify the `textContent` property of the header:

```js
const header = document.querySelector('#header')
header.textContent = 'JavaScript Rocks!'
```

The page is instantly updated.

## Creating a DOM element with JavaScript

We can also create our own DOM elements programatically using JavaScript, and add them into the DOM. If we wanted to create a `h2` underneath our `h1`, we could do it like so:

```js
// get the element you wish to add the new element to
const body = document.querySelector('body')

// create the h2 element
const h2 = document.createElement('H2')

// add some content
h2.textContent = 'I love it!'

// append the element to the parent
body.appendChild(h2)
```

In the Chrome Dev Tools, you should see something like this:

![](https://user-images.githubusercontent.com/3531085/35640472-26206478-06b5-11e8-972a-1e2ac158e647.png)

And the new `h2` element has appeared on the screen.

## Common `properties` and `methods`

Below is a list of the more common properties and methods you will be using on this course:

|  | **Property / Method** | **Description** |
| :--- | :--- | :--- |
|  | `element.children` | Returns a collection of an element's child element \(excluding text and comment nodes\) |
|  | `element.classList` | Returns the class name\(s\) of an element |
|  | `element.classList.add()` | Adds the class name to the element's class attribute |
|  | `element.classList.remove()` | Removes the class name from the element's class attribute |
|  | `element.classList.toggle()` | Toggles the class name of an element's class attribute |
|  | `element.className` | Sets or returns the value of the class attribute of an element |
|  | `element.id` | Sets or returns the value of the id attribute of an element |
|  | `element.offsetHeight` | Returns the height of an element, including padding, border and scrollbar |
|  | `element.offsetWidth` | Returns the width of an element, including padding, border and scrollbar |
| `element.textContent` | Sets or returns the textual content of a node and its | descendants |
|  | `element.innerHTML` | Sets or returns the content of an element |
|  | `element.parentElement` | Returns the parent element node of an element |
|  | `element.style` | Sets or returns the value of the style attribute of an element |
|  | `element.click()` | Simulates a mouse-click on an element |
|  | `element.cloneNode()` | Clones an element |
|  | `element.addEventListener()` | Attaches an event handler to the specified element |
|  | `element.removeEventListener()` | Removes an event handler that has been attached with the addEventListener\(\)\` method |
|  | `element.appendChild()` | Adds a new child node, to an element, as the last child node |
|  | `element.removeChild()` | Removes a child node from an element |
|  | `element.replaceChild()` | Replaces a child node in an element |
|  | `element.insertBefore()` | Inserts a new child node before a specified, existing, child node |
|  | `element.getAttribute()` | Returns the specified attribute value of an element node |
|  | `element.setAttribute()` | Sets or changes the specified attribute, to the specified value |
|  | `element.removeAttribute()` | Removes a specified attribute from an element |
|  | `element.getElementsByClassName()` | Returns a collection of all child elements with the specified class name |
|  | `element.getElementsByTagName()` | Returns a collection of all child elements with the specified tag name |
|  | `element.querySelector()` | Returns the first child element that matches a specified CSS selector\(s\)\` of an element |
|  | `element.querySelectorAll()` | Returns all child elements that matches a specified CSS selector\(s\) of an element |

## Further reading

* [What is the DOM?](https://css-tricks.com/dom/)
* [Introduction to the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
* [How To Understand and Modify the DOM in JavaScript](https://www.digitalocean.com/community/tutorials/introduction-to-the-dom)
