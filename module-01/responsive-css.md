# Responsive CSS

When HTML and CSS were first conceived, there was only one way to browse the web, on a desktop. Now there are desktops, laptops, tablets and phones of different shapes and sizes.

Rather than designing webpages for specific screen dimensions, we aim to make our designs _respond_ to different devices.

## Breakpoints

In order to do this we establish _breakpoints_ or specific pixel sizes at which our design will change. There are several schools of thought on what these specific points should be, but generally we work to three breakpoints:

- mobile: 320px - 480px
- tablet: 481px - 768px
- desktop: < 768px

![](https://pbs.twimg.com/media/Clnncz8WEAA20Tl.jpg)

Since phones have become increasingly larger some designers prefer these breakpoints:

- mobile: 320px - 768px
- tablet: 769px - 1024px
- desktop: < 1024px

There's also a [great article by David Gilbertson](https://medium.freecodecamp.org/the-100-correct-way-to-do-css-breakpoints-88d6a5ba1862) which advocates:

- mobile: > 600px
- tablet portrait: 600px - 899px
- tablet landscape: 900px - 1199px
- desktop: 1200px

## `@media` statement

In order to respond to these breakpoints we can use the `@media` statement to send specific styles to specific breakpoints. The syntax looks like this:

```css
@media [media] and [condition] {
  /* styles applied to that media if condition is met */
}
```
So the `@media` query checks for two things, the media that is viewing the webpage (could be screen, or print), and the width of the browser window. If those conditions are met, the styles inside the block are applied.

### An example

```css
@media only screen and (max-width: 480px) {
  body { background-color: red; }
}

@media only screen and (max-width: 768px) {
  body { background-color: blue; }
}

@media only screen and (min-width: 769px) {
  body { background-color: green; }
}
```

In the example above, the body will appear red on mobile, blue on tablet and red on desktop.

## Mobile first

When building a responsive website, it's best to start with mobile, then tablet, then desktop. This is because mobile is the most challenging screen size to design for. The buttons, form fields and text has to be rather large, and space is limited, so there are more complex user interface problems to solve.

Once the mobile design has been realised, it's normally a case of simply changing layout and font sizes to suit larger screens.

### The viewport tag

Mobile and tablet devices display web pages by zooming out, so that the desktop design is displayed. This is not what we want, as none of out media queries would be realised.

In order to prevent this default behaviour, we can use a `meta` tag in the `head` of our `html` document:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

That says: don't try to render a zoomed out desktop page, but instead use the width of the device, and set the scale (or zoom level) to 1. Now our breakpoints will come into play.

## Further reading
* [Get Started with Responsive Web Design](http://www.creativebloq.com/responsive-web-design/get-started-5132987)
* [The Most Used Responsive Breakpoints in 2017 Of Mine](https://medium.com/@uiuxlab/the-most-used-responsive-breakpoints-in-2017-of-mine-9588e9bd3a8a)
* [Responsive Web Design Basics](https://developers.google.com/web/fundamentals/design-and-ux/responsive/)
* [The 100% correct way to do CSS breakpoints](https://medium.freecodecamp.org/the-100-correct-way-to-do-css-breakpoints-88d6a5ba1862)
