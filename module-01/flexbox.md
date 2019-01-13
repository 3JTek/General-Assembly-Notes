# Flexbox

The CSS Flexible Box Layout Module (or flexbox) is a new layout module in CSS3 that was introduced to make it easier to align items.

When using flexbox, we have to change the way we think about laying out elements using CSS. Instead of adding styles to the children of our container, with flexbox we apply styles on the container, which then affect the children.

![Flex Container & Flex Items](https://i.imgur.com/Hipl6hK.jpg)

From the Mozilla documentation, _"The defining aspect of the flex layout is the ability to alter its items' width and/or height to best fit in the available space on any display device. A flex container expands items to fill available free space, or shrinks them to prevent overflow."_

Now that flexbox has [good browser support](http://caniuse.com/#search=flexbox), it is becoming the go-to for front-end developers when constructing layouts, as positioning is simpler and can be achieved with less code. Rather than thinking about `block` and `inline` elements, we can now think about our elements in terms of rows or columns.

## Flex container properties

### `display: flex`

The flex layout is made up of a parent container (the **flex container**) and its immediate children (the **flex items**). In order to use the flexbox layout, all we have to do is add `display: flex` or `display: inline-flex` to the parent element, and its immediate children will become flex items.

The use of `inline-flex` does not make flex items display inline. It makes the **flex container** display inline. That is the only difference between `display: inline-flex` and `display: flex`.

### `justify-content`

The `justify-content` property horizontally aligns items when the items do not use all available space.

The possible values for `justify-content` are:

* `flex-start` (default) - Items are positioned at the beginning of the container.
* `flex-end` - Items are positioned at the end of the container.
* `center` - Items are positioned at the center of the container.
* `space-between` - Items are positioned with space between each item but not the edge of the container.
* `space-around` - Items are positioned with the same amount of space on both sides, giving the appearance of double the amount of space between items than the edge of the container.
* `space-evenly` - Items are positioned with the same amount of space on both sides and the container.

![justify-content](https://i.imgur.com/PLk2wjQ.jpg)
![justify-content](https://i.imgur.com/Kr3RCVv.jpg)

### `align-items`

The `align-items` property vertically aligns items when the items do not use all available space.

The possible values for `align-items` are:

* `stretch` (default) - Items are stretched to fit the container
* `flex-start` - Items are positioned at the top of the container
* `flex-end` - Items are positioned at the bottom of the container
* `center` - Items are positioned at the center of the container (vertically)
* `baseline` - Items are positioned at the baseline of the container

![align-items](https://i.imgur.com/iZHalzk.jpg)

### `flex-wrap`

The `flex-wrap` property specifies whether the flex items should wrap or not, if there is not enough room for them on one flex line.

The possible values for `flex-wrap` are:

* `nowrap` (default) - The flexible items will not wrap
* `wrap` - The flexible items will wrap if necessary
* `wrap-reverse` - The flexible items will wrap, if necessary, in reverse order

![flex-wrap](https://i.imgur.com/uzlGt4A.jpg)

### `flex-direction`

The `flex-direction` property specifies whether the items should be displayed as a row or a column.

The possible values for `flex-direction` are:

* `row` (default) - The items will be laid out in the same direction as the writing-mode (left to right in this country)
* `row-reverse` - The items will be laid out in the opposite direction.
* `column` - If the writing system is horizontal, the flex items will be laid out vertically.
* `column-reverse` - The items will be laid out in the opposite direction.

![flex-direction](https://i.imgur.com/ARirLs6.jpg)

Have a play around with the `flex-direction` property and notice that the column is hugging the left side of its flex container even if `justify-content: center;` is declared. When you rotate the direction of a container, you also rotate the direction of the justify-content property. It now refers to the container’s vertical alignment, not its horizontal alignment. In order to center the column on the page we would have to add `align-items: center;` instead.

> **Note:** There is a `flex-flow` property that is a shorthand property for the `flex-direction` and the `flex-wrap` properties. For example, you could write `flex-wrap: wrap` and `flex-direction: column` in one line as `flex-flow: column wrap`.

## Further reading

* [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
* [Basic Concepts of Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)
* [Interactive Flexbox Cheatsheet](https://yoksel.github.io/flex-cheatsheet/)
* [Flexbox Froggy](http://flexboxfroggy.com/)
