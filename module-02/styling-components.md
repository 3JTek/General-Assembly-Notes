# Styling React Components

## Introduction

Traditionally, we have split web pages up into markup (HTML), styling (CSS), and logic (JavaScript).

Now that we are working with React, we have already seen how markup and logic are appearing in the same file (using JSX). Pete Hunt (one of the early developers working on React) [famously said](https://www.youtube.com/watch?v=x7cQ3mrcKaY&feature=youtu.be&t=2m7s) while defending the initially controversial nature of JSX, _"we’ve long been forced to separate our technologies rather than our concerns"_.

Pete's talk also touches on the following points:

* Often display logic and markup are inevitably tightly **coupled**.
* This also means that display logic and markup are highly **cohesive**.
* The JavaScript code that drives the UI and the markup that displays it to the user are both doing basically the same thing - they are handling user events and are rendering data to the user.
* It's a seperation of technologies that you're using to implement the same concern.

Whilst he is referring to the relationship between markup and logic, we can also add styling to this conversation. The idea of building **reusable** and **composable** components with React has lead to new ways to think about styling. While best practices are still being figured out, some early patterns have begun to emerge.
Let's have a look at some solutions that are currently on the cards.

## Traditional Approaches

## 1. Old School Styling

We have already had a look at how to link a stylesheet into our React applications using the `import` syntax:

```js
import './scss/style.scss';
```

This allows us to do the type of styling that we are familiar with - adding ids and classes, setting up variables and mixins if we're using SCSS, and hoping for the best. In CSS everything is global by default, which means that often we need to be wary of overriding styles and making updates to our CSS that will alter other areas of the app unexpectedly.

This approach is acceptable when starting out, but as your app grows in complexity, it might be a good idea to move towards a more structured solution.

## 2. CSS Methodologies

What happens when your application starts to expand and new concepts get added? Broad CSS selectors are like globals. The problem gets even worse if you have to deal with loading order. If selectors end up in a tie, the last declaration wins, unless there's !important somewhere. It gets complex very fast.

We could battle this problem by making the selectors more specific, using some naming rules, and so on. That just delays the inevitable. As people have battled with this problem for a while, various methodologies have emerged.

There are plenty of methodologies out there aiming to reduce the CSS footprint, organize cooperation among programmers and maintain large CSS codebases.

Particularly, OOCSS (Object-Oriented CSS), SMACSS (Scalable and Modular Approach for CSS), and BEM (Block Element Modifier) are well known. Each of them solves problems of CSS in their own way.

Check out the [BEM docs](http://getbem.com/introduction/) to see what it's all about.

As they state, _"No matter what methodology you choose to use in your projects, you will benefit from the advantages of more structured CSS and UI"_, however maintaining long class names can be arduous.

## React Based Approaches

With React we have some additional alternatives. What if the way we've been thinking about styling has been misguided? CSS is powerful, but it can become an unmaintainable mess without some discipline. There are various approaches for React that allow us to push styling to the component level.

## 1. Inline Styles

Adding inline styles to HTML using the `style` attribute has been far from best practice for a long time, but React developers are known to question best practices (think JSX!). It may feel counter intuitive,
but foundational features of React fall into this same category. Who would have thought you would put your entire view hierarchy into a render function?

There are a few things to note when writing [inline styles](https://zhenyong.github.io/react/tips/inline-styles.html) inside a React component:

* When we code inline styles with React, all of our styles are actually written in Javascript, not CSS
* CSS attributes should be camel case
* Values are normally wrapped in a string
* Having styling at the component level means we can implement logic that alters those styles easily

> **Note:** The codealong is below - these are just examples

```js
import React from 'react'

const Button = () => {
  const styles = {
    backgroundColor: 'dodgerblue',
    color: 'white',
    border: 'none',
    borderRadius: '4px',
    padding: '8px 12px'
  };

  return (
    <button style={styles}>Click</button>
  );
};

export default Button;
```

If you are concerned about cluttering your component file with CSS, you can always move the styles into a seperate file and import them into them, but note that this makes it less straightforward to use logic inside the styles object.

```js
// button-styles.js

export default {
  backgroundColor: 'dodgerblue',
  color: 'white',
  border: 'none',
  borderRadius: '4px',
  padding: '8px 12px'
};
```

```js
// button.js

import React from 'react'

import styles from './button-styles.js';

const Button = () => {
  return (
    <button style={styles}>Click</button>
  );
};

export default Button;
```

The idea would be that each component has their own `-styles.js` file that is imported into the component file, ensuring that there are no global styles, and one component's styles cannot affect another component.

|Pros|Cons|
|---|---|
|No global styles, everything is a local style object|Since everything is in JS, there is no CSS media query syntax|
|Explicit dependecies using the import syntax|No native support for pseudo selectors, such as hover or last-child|
|Easy to share values between components when necessary|No good option for reuable animations such as CSS keyframe animation when we use inline styles|
|Isolation ensures no components styling influences any other component styling|Difficult to perform large, sweeping changes to our codebase|


At the moment it is very common to see inline styles being used in React libraries.

There are packages that you can use to tackle some of the problems that inline styles have, such as keyframe animations, pseduo selectors and media queries. A popular one is [Radium](https://github.com/FormidableLabs/radium).

## 4. CSS Modules

The premise of CSS Modules is simple - each React component gets its own CSS file, which is scoped to that file and component. The magic happens at build time, when local class names – which can be super simple without risking collisions – are mapped to automatically-generated (hashed) ones and exported as a JavaScript object to use within React components.

For example, two stylesheets could use the same `.button` class, but thanks to CSS modules, the class names will be uniquely hashed, meaning that the styles will not interfer with/be overrided by each other.

There is a little bit of setup that we have to do in order to use CSS modules. Inside `webpack.config.js` update the `style-loader` to be the following:

> **Note:** The codealong is below - these are just examples

```js
{ test: /\.scss$/, loader: ['style-loader', 'css-loader?modules&localIdentName=[local]---[hash:base64:5]', 'sass-loader'], exclude: /node_modules/ }
```

Create a `button-styles.scss` file inside the `scss` directory.

```css
.button {
  background-color: dodgerblue;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 12px;
}
```

The Webpack CSS loader then generates a hash for each selector, known in Webpack as the local ident name. This hashed selector becomes unique on the page.

After hashing the class name will look something like this:

```css
.button--99a0f {
  background-color: dodgerblue;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 12px;
}
```

Even if two files use the same selector name, the hashing will make it unique.

Import this file into the `Button.js` component:

```js
import css from './scss/button-styles.scss';
```
As a React component author, when you import the CSS module, what you really import is the CSS exports object. This object contains each of your original selector names as keys in the object. The values are the associated hashed selectors. We can then add this hashed class name to the `Button` component like this:

```js
const Button = () => {
  return (
    <button className={css.button}>Click!</button>
  );
};
```

When specifiying the `className` attribute, you use the exports object key, but the value that is subsituted after build time and at run time will be the hashed selector. Simple, and powerful.

```html
<button class="button--99a0f">Click!</button>
```

|Pros|Cons|
|---|---|
|One step towards modular and reusable components that will not have side effects|Not as human-readable DOM|
|Shorter and more semantic class names|Some Webpack setup to get started|
|Smaller CSS files||

For a lot of React developers, CSS modules are awesome. They are a great mix of the worlds of inline and external CSS. You get much of the local modular _inline styles_ feeling, and you also get to write your styles using the CSS language. This means that we can use pseudo selectors, media queries and keyframe animations as usual without needed to bring in any additional packages.

> **Note:** If you are going to use this method it's worth looking at the [`classnames`](https://www.npmjs.com/package/classnames) package - which is a _"simple javascript utility for conditionally joining classNames together"_.

## 5. Styled Components 💅

[Styled-Components](https://www.styled-components.com/) is a new CSS tool, created by Max Stoiber and Glen Maddern, which helps you organize CSS in your React project. There is a [talk](https://www.youtube.com/watch?v=jaqDA7Btm3c) that Max gave at a React conference about Styled Components that is worth a watch.

The main thing you need to understand about Styled Components is that its name should be taken quite literally. You are no longer styling HTML elements or components based on their class or HTML element: Instead, you’re defining styled components that possesses their own encapsulated styles. Then you’re using these throughout your codebase.

Three goals of styled components:

1. Getting rid of the mapping between styles and components — Most of the time, a dumb component always has its own small style.css file related. So, you need to create two files every time you want to create the dumb component. This seems to be fine at the beginning, however, when your project is getting bigger, you will end-up with a whole bunch of files. Styled-Components allows you to write CSS directly inside your component, which perfectly solved this problem.
2. Building small and reusable components — Small components can easily be reused and tested. By using Styled-Components, you can easily build a small component and extend its capability with props.
3. Reducing the risk of specificity clash — Everyone might have encounter the specificity clash problem before. For example, you just wanted to add a margin to a specific paragraph, but it unintentionally impacts the other paragraphs. You can easily solve this problem by applying a CSS class only once. Styled-Components is actually doing this for us. It automatically generates a unique class name and pass it to our component.

> **Note:** The codealong is below - these are just examples

To use it in your project first install it using npm or yarn:

```sh
yarn add styled-components
```

Create a file called to hold your component, such as `StyledButton.js`:

```js
import styled from 'styled-components';

const StyledButton = styled.button`
  background-color: tomato;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 12px;
`;

export default StyledButton;
```

First we need to import `styled-components`, and then we can use the Styled Components syntax to create a new button with set styles.

Import this button into another file:

```js
import StyledButton from './styledButton';
```

Add it to a `render()` method:

```js
render() {
  return (
    <main>
      <StyledButton>Styled Component</StyledButton>
    </main>
  );
}
```

> **Note:** The CSS rules are automatically vendor prefixed, so you don't have to think about it.

You can nest styled components inside other styled components. Here is the basic example from the docs.

```js
// Create a Title component that'll render an <h1> tag with some styles
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// Create a Wrapper component that'll render a <section> tag with some styles
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// Use Title and Wrapper like any other React component – except they're styled!
render(
  <Wrapper>
    <Title>
      Hello World, this is my first styled component!
    </Title>
  </Wrapper>
);
```

Another nice feature of the Styled Components library is the ability to adapt styles based on props. From the [docs](https://www.styled-components.com/docs/basics#adapting-based-on-props):

```js
const Button = styled.button`
  /* Adapt the colours based on primary prop */
  background: ${props => props.primary ? 'palevioletred' : 'white'};
  color: ${props => props.primary ? 'white' : 'palevioletred'};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

render(
  <div>
    <Button>Normal</Button>
    <Button primary>Primary</Button>
  </div>
);
```

## Conclusion

There are **so many** blog posts out there that document the "best ways" to style React components - you should be doing your own research on these methods before picking one for a project. You might end up using SCSS as usual - this is totally fine!

## Further reading

* [Modular CSS with React](https://medium.com/@pioul/modular-css-with-react-61638ae9ea3e)
* [Practical Guide to React and CSS Modules](https://www.triplet.fi/blog/practical-guide-to-react-and-css-modules/)
* [What to use for React styling?](http://andrewhfarmer.com/how-to-style-react/)
* [What is the best way to style React components these days?](http://devnacho.com/2016/02/24/what-is-the-best-way-to-style-react-components/)
* [Styled Components: Enforcing Best Practices In Component-Based Systems](https://www.smashingmagazine.com/2017/01/styled-components-enforcing-best-practices-component-based-systems/)
* [Why you shouldn't style with JavaScript](http://jamesknelson.com/why-you-shouldnt-style-with-javascript/)
