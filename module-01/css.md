# CSS

## What is CSS?

CSS stands for Cascading Style Sheets.

HTML is used to give meaning to data on a web page; CSS is used to style the data, to make it look nice.

## Syntax

CSS syntax is very simple:

```css
selector {
  property: value;
}
```

The `selector` is used to match an HTML tag, known as an _element_ on the web page. Some common selectors:

| **Selector** | **Selected Element** |
| --- | --- |
| `section` | Any `section` element |
| `#home` | The element with the `id` of home |
| `.inner` | Any element with the class of `inner` |
| `[href]` | Any element with the attribute `href` |
| `[alt=Doge]` | Any element with the `alt` attribute of `Doge` |

Each element has a set of `properties`, which govern how they appear on the page. Here are some common properties:

| **Property** | **Possible values** | **Effect** |
| :--- | :--- | :--- |
| `border` | `1px solid blue` | A 1px solid blue border |
| `color` | `red` | Text colour set to red |
| `margin-top` | `10px` | Put 10px space between this element and the element above |
| `height` | `calc(100vh - 80px)` | set the height of the element to be the height of the screen - 80px |
| `display` | `none` | hide the element |

There are literally hundreds of properties and values, check out the further reading section for more examples.

## Classes vs IDs

Any element can have a `class` attribute and an `id` attribute. IDs should be unique. Only one element on the page should have a specific ID. Classes can be assigned to multiple elements.

Classes are great for adding the same style to multiple elements at once.

## Inline styles

You can also add styles directly to an element using the `style` attribute like so:

```
<div style="background: grey; color: red; height: 300px"></div>
```

Although it is generally considered bad practise to do so.

## Specificity

CSS is designed to cascade. This means that styles will overwrite existing styles on an element. To decide which style has precedence the browser decides which is more specific.

For example if an ID is used as a selector, it will take precedence over a class. Since an ID is unique the ID selector is more specific than a class, which may be assigned to several elements.

If two style have the same _specificity_, the last one in the CSS file will have precedence.

Consider the following code:

```html
<button>Click</button>
<button id="home" class="red">Click</button>
<button class="red">Click</button>
```

```css
#home {
  background-color: blue;
}
.red {
  background-color: red;
}
button {
  background-color: green;
}
```

The top button will be green, the second button will be blue, because even though the `red` class is after the `home` ID in the stylesheet it is more specific. The last button will be red, again, because the class of `red` is more specific than the button tag.

### Calculating specificity

You can calculate the level of specificity in the following way:

![](https://camo.githubusercontent.com/b92ee8e168d6c67629d84f1211505994ed1419c1/68747470733a2f2f6373732d747269636b732e636f6d2f77702d636f6e74656e742f637373747269636b732d75706c6f6164732f73706563696669636974792d63616c63756c6174696f6e626173652e706e67)

#### Some examples:

![](https://camo.githubusercontent.com/6b081a2cf093cfb2e43b92df882a8c16aaad2f7d/68747470733a2f2f6373732d747269636b732e636f6d2f77702d636f6e74656e742f637373747269636b732d75706c6f6164732f63737373706563696669636974792d63616c632d312e706e67)

Specificity is calculated as **113**

![](https://camo.githubusercontent.com/e758389423bb633fc76c1853f864dd1df6073620/68747470733a2f2f6373732d747269636b732e636f6d2f77702d636f6e74656e742f637373747269636b732d75706c6f6164732f63737373706563696669636974792d63616c632d322e706e67)

Specificity is calculated as **23**

![](https://camo.githubusercontent.com/5eaa9de805f20968233b8664ff56318559b41588/68747470733a2f2f6373732d747269636b732e636f6d2f77702d636f6e74656e742f637373747269636b732d75706c6f6164732f63737373706563696669636974792d63616c632d342e706e67)

Specificity is calculated as **1000**

## Further reading

* [CSS Selector Cheatsheet](https://gist.github.com/smutnyleszek/809a69dd05e1d5f12d01)
* [CSS Diner](https://flukeout.github.io/)
* [CSS-Tricks Beginners Guide](https://css-tricks.com/guides/beginner/)
* [CodePen](https://codepen.io/)
