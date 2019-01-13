# Box Model & Positioning

## Box Model

All HTML elements can be considered boxes. Even if you see a circle, it's living within a box.

The CSS box model describes this principal - a box wraps around all HTML elements, and it consists of: margins, borders, padding, and the actual content. This model allows us to place a border around elements and space elements in relation to other elements.

With CSS properties and values, it is possible to apply specific styles to each of these elements, and change the way they behave and/or display on the page.

Each box has the following properties:

* `margin`: the space between the element and its parent and siblings
* `border`: a line which surrounds the box
* `padding`: the space between the edge of the box and the content
* `content`: the actual content of the box, this might be some text, or an image for example

You can see each of these properties using Chrome's Dev Tools

![](https://camo.githubusercontent.com/1817e95e5cb23e78154d6a7305d14af470e41928/687474703a2f2f692e696d6775722e636f6d2f615864633777422e6a7067)

![](https://camo.githubusercontent.com/98dfda4818eaa54bd2e7ed3614cc0b172489a2aa/687474703a2f2f692e696d6775722e636f6d2f6f6250626274422e706e67)

### Modifying box model properties

All of the properties listed above can be modified with CSS. An element's margin and padding can either be set with one command or four:

```css
/* with a single command */
div {
  margin: 10px; /* sets 10px margin around the whole element */
  padding: 10px 20px; /* sets 10px padding top and bottom and 20px margin left and right */
  margin: 10px 20px 30px; /* sets 10px top, 20px left and right and 30px bottom */
  padding: 10px 20px 30px 40px; /* sets 10px top, 20px right, 30px bottom and 40px left */
}

/* with four commands */
div {
  margin-top: 10px;
  margin-right: 20px;
  margin-bottom: 30px;
  margin-left: 40px;
}
```

Border is similar, but we have to define the width, style and colour. Here are a few examples:

```css
div {
  border: 1px solid black;
  border-top: 1px solid black;
}

div {
  border-width: 2px;
  border-style: dotted;
  border-color: #ff5;
}

div {
  border-top-width: 5px;
  border-top-style: dashed;
  border-top-color: #d43;
}
```

### `box-sizing` property

All browsers (except legacy versions of Internet Explorer) calculate the dimensions of an element as:

```
content + padding + border
```

So if you have a 200px wide `div` for example, and you add `border: 1px solid black` and `padding: 10px`, your `div` will become 222px wide (200px + 1px left border + 1px right border + 10px left padding + 10px right padding).

This can be quite counter intuitive and can make layout a pain. However we can change the way that the dimensions are calculated using `box-sizing`.

At the top of your CSS file we can use the global selector (\*) to set the `box-sizing` property to `border-box` like so:

```css
* { box-sizing: border-box; }
```

Now if we set the width of a `div` to be 200px and add `border: 1px solid black` and `padding: 10px`, it will remain 200px wide! This makes layout much more intuitive.

## Positioning

### `display` property

An element's `display` property can be set to one of the following:

* `block`: the element takes up the full width of its parent. It will not let other elements sit next to it.
* `inline`: the element sits in the flow of the document, as if it is a word in a sentence. Other inline elements are free to sit next to it. Top and bottom margin settings will be ignored, but left and right margins and any padding will be applied.
* `inline-block`: the element will sit in the flow of the document, but all box-model properties will be applied.
* `none`: the element will not be displayed at all.

### `position` property

It is possible to position an element on the screen with the `position` property.

An element's `position` property can be set to one of the following:

##### `static` (default)

* The element is rendered in the default position based on its size, margin and other elements around it.

##### `relative`

* The element is rendered in the default position.
* Its position can be set using the `left`, `right`, `top` and `bottom` properties.
* Positioning the element does not affect the position of other elements.
* The new position of the element is calculated from the current position of the element.

##### `absolute`

* The element is rendered in the default position.
* Other elements on the page behave as if that element is not there, which may cause the element to sit on top of an other.
* The element's position can be set with `left`, `right`, `top` and `bottom`.
* The new position of the element is calculated from the closest parent which is _not_ statically positioned, _or_ the edge of the screen.

##### `fixed`

* The element is fixed in position when the page is scrolled.
* The element's position can be set with `left`, `right`, `top` and `bottom`.
* The new position of the element is calculated from the edge of the screen.

> **Note**: Understanding how `relative` and `absolute` position affect an element, and how they interact with each other is fundamental to grasping CSS. **It is worth playing with them in isolation to get a good understanding of how they work before moving on to the next section.**

### Examples absolute position

When the _parent element_ of an absolutely positioned element is static, the absolutely positioned element's position is calculated from the edge of the screen. _This is the default behaviour._

![](https://camo.githubusercontent.com/e570e37105edb51dc74ba7cd8daf247f1b2c0e4a/68747470733a2f2f692e696d6775722e636f6d2f3076476350464c2e706e67)

When the _parent element_ is relative, the absolutely positioned element's position is calculated from the edge of the parent element. **This is normally the behaviour you want.**

![](https://camo.githubusercontent.com/1b5a6edb9d3658d214b3ba4924ae3d81b13d352e/68747470733a2f2f692e696d6775722e636f6d2f4c5264376c42792e706e67)

> **Note**: When positioning elements on the screen you should be wary of the `absolute` setting. Make sure you are checking your layout at different screen sizes as you work.

### `float` property

Floating is a relatively simple concept, but it does have its nuances.

The original idea behind `float` was to allow text to flow around elements, similar to a magazine layout:

![](https://cloud.githubusercontent.com/assets/40461/8234489/3b61ef02-15d4-11e5-8864-435fb6e0c3cc.png)

However it was quickly appropriated for creating complex grids for layout.

The `float` property can be set to one of the following:

* **none** (default): the element sits in the flow of the document.
* **left**: the floated element aligns to the left, and allows other elements to fill up the space to its right.
* **right**: the floated element aligns to the right, and allows other elements to fill up the space to its left.

Floating an element has the following side-affects:

1. All elements will float next to floated items, which is not always desired.
2. The height of the element's parent will be affected. It will disregard the height of the element (as if it were no longer there). This can make layout challenging.

#### Clearing a float

To stop an element from flowing around a floated element, you can use the `clear` property.

In the diagram below notice how the `<footer>` is sitting next to the element that has been floated left, as it has not been cleared.

![](https://cloud.githubusercontent.com/assets/40461/8234478/287c1156-15d4-11e5-9901-ba9090a5bf70.png)

#### Restoring the parent's height

To restore the parent element's height it was common to use the [clearfix hack](https://css-tricks.com/snippets/css/clear-fix/).

However it is now also possible to set the parent's `overflow` property to `hidden`, or `auto`.

## Further reading

* [The CSS Box Model](https://css-tricks.com/the-css-box-model/)
* [Box Sizing](https://css-tricks.com/box-sizing/)
* [Absolute, Relative, Fixed Positioning: How Do They Differ?](https://css-tricks.com/absolute-relative-fixed-positioining-how-do-they-differ/)
* [All About Floats](https://css-tricks.com/all-about-floats/)
