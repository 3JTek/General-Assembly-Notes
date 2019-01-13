# CSS Frameworks

When we develop apps and websites, we often have a lot to do. There are HTML templates to make, CSS to write and JavaScript functionality to develop. When we work in big teams often these jobs are taken care of by different developers with different specialities.

However, when we are working in smaller teams, or on our own, the amount of work needed to get a simple idea to a point where we can begin to show other people (stakeholders, investors, etc.) can be quite overwhelming.

However, there are plenty of resources out there to help us develop attractive sites quickly, so we can concentrate on what it is that our app or website actually does. This is where CSS frameworks come into play.

Throughout this course we will focus on [Bulma](bulma.io), however, all CSS frameworks follow the same basic principals for installation and usage.

## Why Bulma?

Bulma is a modern CSS framework which uses flexbox and is highly customisable. It is lightweight and has no JavaScript dependencies, which means it can easily be installed in to any project regardless of other 3rd-party products being used.

## How does it work?

Broadly speaking a CSS framework is a collection of styles attached to pre-defined classes. These classes can be assigned to different elements in order to style them.

There is a common visual language throughout the web which, as web developers we often need to replicate. Some examples include:

* Responsive grid
* Responsive navbars and burger menus
* Forms
* Cards
* Dropdowns and revealers
* Tabs
* Breadcrumbs and pagination
* Modals
* Alerts and notifications

A CSS framework has styles for all of these and more. All we need to do is apply the correct style to the correct element.

## Installation

There are normally a few different ways to install a CSS framework, but fundamentally speaking we need to add the framework's CSS file **before** our own. That generally means either downloading the file into our project, or using a Content Delivery Network (CDN).

A CDN simply put is when we use a piece of software hosted on a special highly-optimised server owned by a 3rd-party.

Here's an example using a CDN:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Hello Bulma!</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.6.2/css/bulma.min.css">
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>

  </body>
</html>
```

Notice that our stylesheet must come **after** the framework's!

## Usage

Now that the framework is installed, it's simply a case of finding visual elements from the documentation and adding them into our project. For example, if we wanted a responsive navbar, we can copy the code from the docs and paste it into the `body` of our HTML file like so:

```html
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <img src="https://bulma.io/images/bulma-logo.png" alt="Bulma: a modern CSS framework based on Flexbox" width="112" height="28">
    </a>

    <button class="button navbar-burger">
      <span></span>
      <span></span>
      <span></span>
    </button>
  </div>
</nav>
```

## The Grid

One of the most important and useful parts of a CSS framework is the grid. It allows us to very quickly make complex page layouts that can elegantly change and adapt to different screen sizes.

All grids are made of rows and columns. Columns are elements inside rows, and can be set to certain widths at certain screen sizes.

Here's a simple example:

```html
<div class="columns">
  <div class="column">
    First column
  </div>
  <div class="column">
    Second column
  </div>
  <div class="column">
    Third column
  </div>
  <div class="column">
    Fourth column
  </div>
</div>
```

With this layout, we will have four columns on desktop and tablet, but on mobile the columns will stack to look like four rows.

Let's modify this behaviour slightly:

```html
<div class="columns is-multiline">
  <div class="column is-full-desktop is-half-tablet">
    First column
  </div>
  <div class="column is-one-third-desktop is-half-tablet">
    Second column
  </div>
  <div class="column is-one-third-desktop is-half-tablet">
    Third column
  </div>
  <div class="column is-one-third-desktop is-half-tablet">
    Fourth column
  </div>
</div>
```

By adding the `is-multiline` modifier class, the columns will wrap over more than one line if need be. The responsive classes `is-half-tablet`, `is-one-third-desktop` and `is-full-desktop`, force the columns to resize for those devices.

As you can see it is fairly trivial to set up fairly elaborate designs by simply adding some classes.

## Tips

* When learning a CSS framework it is best to pick one and stick to it until you are very comfortable with it.
* **Do not fight the Framework!** Make sure you apply your CSS framework across your whole app before you attempt to modify it and add your own styles.
* Spend a good amount of time with the documentation, you may find some cool extra features and helpers.

## Further reading

* [Bulma Documentation](https://bulma.io/documentation/overview/start/)
* [A CSS Framework in 6 minutes with Bulma](https://www.sitepoint.com/a-css-framework-in-6-minutes-with-bulma/)
* [Get To Know Bulma](https://scotch.io/bar-talk/get-to-know-bulma-my-current-favorite-css-framework)
* [Top 5 Most Popular CSS Frameworks that You Should Pay Attention to in 2017](https://hackernoon.com/top-5-most-popular-css-frameworks-that-you-should-pay-attention-to-in-2017-344a8b67fba1)
