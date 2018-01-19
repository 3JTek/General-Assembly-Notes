# SASS

## What is SASS?

SASS stands for Syntactically Awesome Stylesheets. It is a pre-processor for CSS.

CSS can be a little difficult to work with mainly because of all the selectors we have to write out. SASS attempts to make CSS easier to work with by allowing us to:

- nest selectors
- create functions
- split our styles into smaller files
- write less CSS

## How does it work?

Web browsers cannot read SASS files, they can only read CSS files. We write SASS then we run a command in the terminal that will convert the SASS to a CSS file, so that a browser can interpret it. This is called _compiling_: converting code from one language (SASS) to another (CSS).

## SASS vs SCSS

There are two different syntaxes for writing SASS. The original SASS looks like this:

```sass
  nav
    ul
      margin: 0
      padding: 0
      list-style: none

    li
      display: inline-block

    a
      display: block
      padding: 6px 12px
      text-decoration: none
```

The alternative to SASS is SCSS (Sassy CSS), and it looks like this:

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

As you can see they are almost identical. **On this course we'll be using SCSS.**

## SASS Features

### Nesting

With SCSS we can nest selectors, which means we can write less code:

```scss
footer {
  width: 100%;
  height: 100%;
  background: #fff;
  padding: 1em 0;
  p {
    color: rgba(0,0,0,0.5);
    a { color: #c69; }
  }
  .date { color: #036; }
  ul.social_buttons {
    li {
      a.twitter { color: blue; }
      a.fb { color: green; }
      a.gplus { color: red; }
    }
  }
}
```

When converted to CSS, the above SCSS would look like this:

```css
footer {
  width: 100%;
  height: 100%;
  background: #fff;
  padding: 1em 0;
}

footer p { color: rgba(0,0,0,0.5); }

footer p a { color: #c69; }

footer .date { color: #036; }

footer ul.social_buttons li a.twitter { color: blue; }

footer ul.social_buttons li a.fb { color: green; }

footer ul.social_buttons li a.gplus { color: red; }
```

### Using `&`

You can also use `&` when nesting to represent the parent selector:

```scss
a {
  &.twitter { color: blue; }
  &.fb { color: green; }
  &.gplus { color: red; }
}
```

The example would be equivalent to:

```scss
a.twitter { color: blue; }
a.fb { color: green; }
a.gplus { color: red; }
```

## Variables

Often it can be annoying when working with CSS that you have to remember HEX codes for various colors on the site. With SCSS, we can store property values in variables to reduce mistakes:

```scss
$body-background-color: #f5f5f5;
$text-color: #ccc;
$title-font: "Futura Bold", sans-serif;

body {
  background: $body-background-color;

  h1,h2 {
    font-family: $title-font;
    color: $text-color;
  }
}
```

## Modifying colours

Often we want to lighten or darken the background colour of a button or element on hover. With SCSS we can use the `lighten` and `darken` methods to calculate a new colour based on our original one:

```scss
$brand-color: #6f8;

button {
  background-color: $brand-color;
  &:hover {
    background-color: darken($brand-color, 20%);
  }
}
```

## Compiling SASS/SCSS

The simplest way to compile our SCSS files is by using the `sass` command-line tool. It's a ruby package, so we need to install it with ruby:

```sh
gem install sass
```

Once installed we can use the following command to transpile our SCSS:

```sh
sass path/to/source:path/to/dest
```

If our SCSS file was in `scss/style.scss` and we wanted to transpile it to `css/style.css` the command would look like this:

```sh
sass scss/style.scss:css/style.css
```

We can also watch for changes in our SCSS file with the `--watch` option. This way, when we update the SCSS file, it will automatically be converted to CSS on the fly.

```sh
sass --watch scss/style.scss:css/style.css
```

## Further reading

[SASS Basics](http://sass-lang.com/guide)
[An Absolute Beginners Guide To SASS](http://blog.teamtreehouse.com/the-absolute-beginners-guide-to-sass)
[Getting Started With SASS (with Interactive Examples)](https://scotch.io/tutorials/getting-started-with-sass)
[Advanced SCSS](https://gist.github.com/jareware/4738651)
[SASS Style Guide](https://css-tricks.com/sass-style-guide/)
