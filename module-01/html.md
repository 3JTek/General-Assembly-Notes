# HTML

## What is HTML?

HTML stands for Hyper Text Markup Language.

HTML is not a programming language, since it cannot perform any computation. It is simply text, which can be interpreted by a web browser, so that the data can be rendered on the screen.

It was created in 1993 by Tim Berners-Lee as a way to share academic papers across the internet.

### Semantics

HTML was created as a way for the web browser, and search engines, to infer meaning about the contents of a web page. The various bits of content on the page are wrapped in **tags**. Each tag has a specific meaning. Tabular data is wrapped in `table` tags, paragraphs wrapped in `p` tags, and so on.

The important thing to remember is that **HTML is about semantics, not about layout.** Layout and styling is taken care of by a different language: CSS.

### XML

HTML is a subset of XML, which stands for Extendable Markup Language. XML was designed to help computers send information to each other via a network (like the internet).

If we wanted to represent a book in XML it would probably look something like this:

```xml
<Book>
  <Title>The Lord of The Rings: The Fellowship of the Ring</Title>
  <Author>J R R Tolkein</Author>
  <Snippet>
    When Mr. Bilbo Baggins of Bag End announced that he would shortly be celebrating his eleventy-first birthday with a party of special magnificence, there was much talk and excitement in Hobbiton.
  </Snippet>
</Book>
```

The information is wrapped in tags, which indicate the start and end of each piece of information, and gives it some _meaning_.

HTML is very similar:

```html
<h1>The Lord of The Rings: The Fellowship of the Ring</h1> <!-- Title (Heading 1) -->
<h2>Chapter 1: A Long-Expected Party</h2> <!-- Subtitle (Heading 2) -->
<p> <!-- paragraph -->
  When Mr. Bilbo Baggins of Bag End announced that he would shortly be celebrating his eleventy-first birthday with a party of special magnificence, there was much talk and excitement in Hobbiton.
</p>
```

## Rules

All HTML files must conform to the following rules:

1. It must start with a `<!DOCTYPE>` declaration indicating which version of HTML the file should be interpreted as.
1. It must have an `<html>` tag.
1. The `<html>` tag must contain a `<head>` section and a `<body>` section.
  * The `<head>` section contains information for the browser, and is not visible to the user.
  * The `<body>` section contains all the data that will be visible to the user.
1. The appropriate tag should be used for the appropriate data.

## Example

A HTML boilerplate:

```html
<!DOCTYPE html> <!-- this indicates that we are using HTML5 (the latest version of HTML)-->
<html> <!-- indicates the beginning of the html document -->
  <head> <!-- head tag contains information for the browser -->
    <meta charset="utf-8"> <!-- indicated the character set (utf8 allows emoji characters...) -->
    <title>My Webpage</title> <!-- This will appear in the browser's title bar or tab -->
  </head>
  <body> <!-- contains data to be seen by the user -->
    <h1>Hello World!</h1>
  </body>
</html>
```

## Attributes

Some elements can also be given attributes which change the way they render data. The most common attribute is the `class` attribute, but others include `href`, `src`, `id`, `name`, `value` etc.

Here's an example of some tags with attributes:

```html
<nav class="navbar"></nav>
<img src="https://gph.is/28UMzPi" /> <!-- self-closing tag -->
<input type="text" name="name" /> <!-- self-closing tag -->
```

## Common HTML tags

Here is a list of a few of the most common tags you'll be using on the course:

| Tag | Meaning | Block / Inline | Info |
|-----|---------|------|
| `<header>` | Header | Block | The main heading of the page. This usually contains the site's branding and navigation. |
| `<main>` | Main Content | Block | The content of the page. This usually immediately follows the header. |
| `<footer>` | Footer | Block | The footer of the page. This generally holds navigation links and copyright information. |
| `<div>` | Division  | Block | Used to group elements for styling purposes. _Has no semantic meaning._ |
| `<span>` | Span | Inline | Similar to `<div>` |
| `<p>` | Paragraph | Block |  |
| `<h1> <h2> <h3> <h4> <h5> <h6>` | Heading | Block |  |
| `<a>` | Anchor | Inline | Also known as a link |
| `<img>` | Image | Inline |  |
| `<ol>` | Ordered List | Block | Must contain `<li>` |
| `<ul>` | Unordered List | Block | Must contain `<li>` |
| `<li>` | List Item | Block |  |
| `<table>` | Table | Block |  |
| `<section>` | Defines a section of the page | Block |  |
| `<form>` | Denotes a form | Block |  |
| `<input>` | User input field | Inline | Should be inside a form |
| `<button>` | A button | Inline | Will submit a form when inside a form |

## `section`, `article` & `aside`

The three most nuanced HTML tags are `section`, `article` & `aside` and are worth taking a moment to understand:

- **`section`**: A section defines a distinct part of a web page. Imagine a newspaper site, it may have the following different sections on the homepage: top stories, politics, sport etc.
- **`article`**: An article defines a part of a web page that would make sense on its own. An example would be a news story. It could appear on a completely different site and still make sense.
- **`aside`**: This is a piece of relevant information which, if omitted, would not take away from the meaning of the web page. An image or a graph could be considered an aside. Although tempting, **an aside is NOT a sidebar, or side panel**. An aside has nothing to do with positioning. (A side panel would be considered a `section`).

## Further reading

* [HTML Elements reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
* [Why Use Semantic HTML](https://www.thoughtco.com/why-use-semantic-html-3468271)
* [HTML5 Element Flowchart](http://html5doctor.com/downloads/h5d-sectioning-flowchart.pdf)
