# CSS Animations

Animation used to be the realm of JavaScript. However, in modern browsers, we can animate elements using CSS.

The `@keyframes` block and `animation` properties allow us to specify what gets animated and when.

## Animations in CSS

Animations can run 1 or more times or loop infinitely. It's also possible to add multiple animations to the same element.

Animations can be triggered in CSS as soon as the page loads, after a delay, or via some kind of state based change like `:hover`, `:focus`, or `:active`.

CSS animations can also be started and stopped in JavaScript by adding and removing classes.

## Keyframes

In order to animate an element (or selection of elements), we need to specify a series of keyframes.

The most basic form of keyframe animation goes _from_ one set of styles _to_ another set of styles, over a certain amount of time.

During the animation, the styles between keyframes are automatically calculated by the browser - a process known as _tweening_.

Each keyframe is defined as a block of CSS properties that will be applied to any element at that stage in the animation.

```css
@keyframes moveLeft {
  from { left: 0; }
  to   { left: 500px; }
}
```

The above animation is called `moveLeft`. When applied to an element, it will animate from 0 - 500px from the left.

Additional keyframes can be specified using a percentages syntax. If the animation duration were 10 seconds, over the first 7.5 seconds, the `font-size` of the element would grow to `100px` and then over the next 2.5 seconds, it would shrink back down to `10px`. You can specify as many properties as you like for each keyframe, and as many keyframes as you like in a `@keyframes` statement.

```css
@keyframes grow {
  0%   { font-size: 20px; }
  75%  { font-size: 100px; }
  100% { font-size: 10px; }
}
```

## Animation

When the `@keyframes` have been defined, they are ready to be used in conjunction with the `animation-name` property.

There are a series of `animation` properties to configure your animation as needed:

* `animation-name` specifies `@keyframes` block to use
* `animation-duration` specified the time the animation lasts
* `animation-delay` specifies any delay before the animation starts
* `animation-iteration-count` specifies the number of times to repeat
* `animation-direction` specifies the direction; animations can play forwards (default), in reverse or alternate back and forth
* `animation-play-state` allows the animation to be paused and resumed
* `animation-timing-function` determines an acceleration curve of how the animation plays between keyframes
* `animation-fill-mode` determines how styles are applied before and after the animation

These 8 properties can be combined into a shorthand `animation` property as follows:

```css
.box {
  animation: name duration delay count direction play-state timing fill-mode;
}
```

The only required properties for an animation to be visible at least once are: `animation-name` and `animation-duration`.

## Bouncing ball animation

Let's look at a practical example.

We can create a ball using an equal `width` and `height` box with `border-radius` set to `100%`. We can make the ball bounce up and down by absolutely positioning it and animating the `top` or `bottom` values over time.

We can give the ball a bit more realism by squashing it at the bottom of the animation before having it travel back up, at a slightly slower speed.

```css
.ball {
  position:absolute;
  width:100px;
  height:100px;
  border-radius:100%;
  animation:bounce 3s linear infinite;
}
@keyframes bounce {
  0% { bottom:100%; }
  25% {
    bottom:0;
    width:100px;
    height:100px;
  }
  30% {
    bottom:0;
    height:50px;
    width:110px;
  }
  35% {
    bottom:0;
    width:100px;
    height:100px;
  }
  70% { bottom:100%; }
  100% { bottom:100%; }
}
```

We can make the ball move across the screen by adding a second animation that animates the `left` property of the ball. These can be comma separated so they are both applied to the same element.

To make it appear as though the ball bounces slowly across the screen, we can increase the duration of this second animation.

```css
@keyframes moveLeft {
  from { left:0; }
  to   { left:100%; }
}
.ball {
  animation: bounce 3s linear, moveLeft 12s linear infinite;
}
```

## Browser support

CSS animations are not supported in IE9 or below or Opera Mini.

## CSS Transitions

Transitions are another way to handle animating changes from one state to another, but they don't allow for the same degree of flexibility as animations. Rather than keyframes, you can simply specify the duration of the change, the timing function (e.g. linear, easeIn...) and the delay.

```css
div {
  height: 100px;
  width: 100px;
  background: red;
  transition-property: background;
  transition-duration: 3s;
  transition-timing-function: linear;
  transition-delay: 1s;
}

div:hover {
  background: blue;
}
```

Transitions are for creating a smooth transition from one state to another, and animations for more complex series of movements.

It's possible to set transitions on multiple properties at once.

```css
div {  
  height: 100px;
  width: 100px;
  background: red;
  transition-property: background, height, width;
  transition-duration: 3s, 5s, 5s;
  transition-timing-function: linear;
  transition-delay: 1s;
}

div:hover {
  background: blue;
  height: 200px;
  width: 200px;
}
```

## Further reading

* [Animation](https://css-tricks.com/almanac/properties/a/animation/)
* [CSS Animation for Beginners](https://robots.thoughtbot.com/css-animation-for-beginners)
* [15 Inspiring Examples of CSS Animation on CodePen](https://webdesign.tutsplus.com/articles/15-inspiring-examples-of-css-animation-on-codepen--cms-23937)
* [Transitions Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions)
* [Transition vs. Animation](https://cssanimation.rocks/transition-vs-animation/)
* [CSS Animation Support - caniuse.com](https://caniuse.com/#search=animation)
