# Javascript

JavaScript has become an extremely important language. Its ubiquitous in web-capable devices, a pillar of the Web Platform and the base for most web and mobile applications. Thanks to [Node.js](https://nodejs.org/) and [MongoDB](https://www.mongodb.com/) (among others), it's now a permanent fixture on servers and in databases as well.

## A little history

Tim Berners-Lee invented the first web server, the first web browser (called WorldWideWeb), the first web editor (WorldWideWeb worked as a web editor as well), and the first web pages.

However, Tim Berners-Lee (and many others) saw the Web as a tool for scientists and researchers to exchange information. Luckily for us, some folks had different plans for it.

#### Mosaic

A group of students at the University of Illinois Urbana-Champaign created Mosaic, which is the browser that popularised the World Wide Web.

The reasons might be many, but the main one was that they totally ignored Berners-Lee and went ahead and shipped the `<IMG>` tag, which made the web instantly more attractive.

Mosaic quickly became the most popular browser.

![](https://cloud.githubusercontent.com/assets/40461/8239877/a153716a-15f6-11e5-9760-5d02cb984a2e.jpg)

#### Netscape Navigator

[Netscape](https://en.wikipedia.org/wiki/Netscape) took Mosaic’s authors, had them reimplement a browser which they called Navigator, and built a business around it.

The reason why people would pay for Netscape Navigator was that Netscape ignored Berners-Lee even more, and implemented a lot of useless stuff that people wanted, like font faces and other things that we now take for granted.

![](https://user-images.githubusercontent.com/3531085/35187837-96bb10fc-fe22-11e7-8fa3-b854992f29af.png)

#### Microsoft

Netscape was really cool and worked exactly the same on all operating systems. Another great feature they were experimenting with was a web-based system that allowed users to edit files across the network, regardless of what operating system being used.

Microsoft didn't like this, as it undermined their business model.

So, they released Internet Explorer, gave it away for free, and started the infamous [browser wars](https://en.wikipedia.org/wiki/Browser_wars).

![](https://user-images.githubusercontent.com/3531085/35187854-f498d772-fe22-11e7-9830-52e11b1b86fd.png)

### Brenden Eich

[Brendan Eich](https://en.wikipedia.org/wiki/Brendan_Eich) was hired by Netscape to build their new client-side language in 1995. It was first released with Netscape 2, early in 1996.

Brendan Eich wrote the original code for Javascript in just 10 days. There is a lot of rumours about the fact that he was either drunk or high when he wrote it...

Javascript was originally going to be called LiveScript, but was renamed in an attempt to capitalise on the popularity of Sun Microsystem's Java language — despite the two having very little in common. This has been a source of confusion ever since.

#### JScript

Microsoft wanted LiveScript in their browser so they reversed-engineered JavaScript (pretty well, actually), and put it into Internet Explorer.

Because of the "Java" trademark, they had to name it JScript, but it was essentially the same thing.

#### ECMAScript

Because of the different versions available, JavaScript needed to be standardised. Netscape looked for a body that would do just that. The W3C refused to do it, and eventually they ended up at the European ECMA.

> ***Note:*** _The World Wide Web Consortium (W3C) is the main international standards organisation for the World Wide Web_

> ***Note:*** _ECMA International is an industry association founded in 1961, dedicated to the standardisation of information and communication systems._

ECMA did standardise the language, but didn’t fix the obviously awful and confusing "JavaScript" name. The thing is, they didn’t know what to call it. So, they just published it with their working name: ECMAScript.

JavaScript, JScript, and ECMAScript are sometimes thought to be different things, but are simply three different names that mean the same thing: JavaScript.

## A weird scripting Language

Unlike most programming languages, the JavaScript language has no concept of input or output. It is designed to run as a scripting language in a host environment, and it is up to the host environment to provide mechanisms for communicating with the outside world.

The most common host environment is a web-browser, but JavaScript interpreters can also be found in a huge list of other places, including Adobe Acrobat, Adobe Photoshop, SVG images, Yahoo's Widget engine, server-side environments such as [Node.js](http://nodejs.org/), NoSQL databases, embedded computers, complete desktop environments like [GNOME](http://www.gnome.org/) (one of the most popular GUIs for GNU/Linux operating systems), and the list goes on.

## Further reading

* [A Brief History of JavaScript](https://auth0.com/blog/a-brief-history-of-javascript/)
* [JavaScript - Wikipedia](https://en.wikipedia.org/wiki/JavaScript)
* [JavaScript.com](https://www.javascript.com/)
