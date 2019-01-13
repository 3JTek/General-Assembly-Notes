# DOM Events

When a Web page is rendered, the user can interact with it. Clicking on links and buttons to change pages or to make windows pop up, entering information in forms and view responses based on entries. In these and many other ways, web pages are responsive to actions.

In other words, web pages are _event driven_. They react to events that are initiated by the user, such as mouse clicks or keyboard input.

## Handling events

When a user interacts with a web page the DOM emits an event which we can hook into to trigger some functionality.

### A simple `click` event handler

We can listen for a click event in two ways, either directly in the HTML with inline JavaScript, or in a JavaScript file linked to the HTML document.

Just like CSS it is generally considered bad practice to use inline JavaScript, but we'll show it here for brevity:

```html
<button onclick="console.log('hello')">Click Me</button>
```

```js
const button = document.querySelector('button')
button.onclick = function() {
  console.log('hello')
}
```

When the `click` event is triggered a function is called. This function is known as an _event handler_ or _event listener_.

## The `event`

When an event listener is called it has access to an `event` object which we can interrogate to find out more information about the event. This will tell us which element triggered the event (the _target_), the time when the event was triggered, methods to prevent the default behaviour of the event and more.

The event object gets passed into the event listener as the first argument. Generally we call it `e` or `evt`:

```js
const button = document.querySelector('button')
button.onclick = function(e) {
  console.log('hello')
  console.log(e.target) // => <button>Click Me</button> -- the button that triggered this event
  console.log(e.target.innerHTML) // => Click Me
}
```

## `this`

Inside and event listener, `this` points to the element that triggered the event if we use a standard function. **With an arrow function, `this` will not be the element, so in this case we can use `e.target`.**

```js
const button = document.querySelector('button')
button.onclick = function(e) {
  console.log(this) // => <button>Click Me</button> -- the button that triggered this event
  console.log(this.innerHTML) // => Click Me
}

button.onclick = (e) => {
  console.log(this) // => Window { ... }
  console.log(e.target) // => <button>Click Me</button> -- the button that triggered this event
  console.log(e.target.innerHTML) // => Click Me
}
```

## `addEventListener`

If we want to listen out for a `click` event we can add an event listener to the element's `onclick` property, for a `submit` event we would use the `onsubmit` property, and so on.

If we add two event listeners to the same property, the second would overwrite the first:

```js
const button = document.querySelector('button')
button.onclick = () => console.log('Hey')
button.onclick = () => console.log('Ho') // Only this function would be called
```

More commonly we would use the `addEventListener` method to add event listeners to an element. This way multiple event listeners can listen out for the same event:

```js
const button = document.querySelector('button')
button.addEventListener('click', () => console.log('Hey'))
button.addEventListener('click', () => console.log('Ho')) // Both functions are called
```

## Common events

| Event | Behaviour |
|-------|-----------|
| `click` | Triggered on mouse clicked and then released, or touch |
| `mousedown` | Triggered when the mouse button (or trackpad) is clicked |
| `mouseup` | Triggered when the mouse button (or trackpad) is released |
| `keypress` | Triggered when a key is pressed and then released |
| `keydown` | Triggered when a key is pressed |
| `keyup` | Triggered when a key is released |
| `dblclick` | Triggered on double click |
| `mouseover` | Triggered when the mouse cursor moves over an element |
| `mouseout` | Triggered when the mouse cursor moves out of an element |
| `resize` | Triggered when the browser window is resized. _Only works on the window object._ |
| `scroll` | Triggered when the user scrolls the window. _Only works on the window object._ |
| `focus` | Triggered when an input becomes focussed |
| `blur` | Triggered when an input loses focus |
| `change` | Triggered when the value of an input changes |
| `submit` | Triggered when a form is submitted. _`e.preventDefault()`_ should be used to prevent reload. |

## Event bubbling

If we place a click event listener on a child element and also one on its parent. When we click the child element the child's event listener will fire, but so will the parent's. We describe this behaviour as _event bubbling_. The event bubbles up the DOM tree.

In order to prevent this we can use `e.stopPropagation()` at any point in the DOM tree:

```html
<div id="parent">
  <div id="child">
    <div id="grandchild"></div>
  </div>
</div>
```

```js
const parent = document.querySelector('#parent')
const child = document.querySelector('#child')
const grandchild = document.querySelector('#grandchild')

parent.addEventListener('click', () => {
  console.log('parent click')
})
parent.addEventListener('click', () => {
  e.stopPropagation()
  console.log('child click')
})
parent.addEventListener('click', () => {
  console.log('grandchild click')
})
```

In the example above if we click on the grandchild element, we would see `grandchild click` and `child click` in the console, but the bubbling would stop there and never reach the parent event listener.

## Further reading

* [An Introduction to DOM Events](https://www.smashingmagazine.com/2013/11/an-introduction-to-dom-events/)
* [Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events)
* [Events and the DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Events)
