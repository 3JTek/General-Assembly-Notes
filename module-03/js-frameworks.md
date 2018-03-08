# JavaScript Frameworks

## MVC vs MV*?

Most modern web frameworks allow developers to organise code using variations of a pattern known as MVC (Model-View-Controller). MVC separates the concerns in an application down into three parts:

- **Models** represent the domain-specific knowledge and data in an application. Think of this as being a ‘type’ of data you can model — like a User or Photo.
- **Views** are typically considered the User-interface in an application. They should know about the existence of Models in order to observe them, but don’t directly communicate with them.
- **Controllers** responding to the input (e.g user actions & clicks) in an application and Views can be considered as handling the output. When a Controller updates the state of a model (such as editing the caption on a Photo), it doesn’t directly tell the View. This is what the observing nature of the View and Model relationship is for.

JavaScript ‘MVC’ frameworks that can help us structure our code don’t always strictly follow the above pattern. Some frameworks will include the responsibility of the **Controller inside the View** (e.g Backbone.js) whilst others add their own opinionated components into the mix.

### MV*

For this reason we refer to such frameworks as following the MV* pattern, that is, you’re likely to have a Model and a View but you probably won't have a Controller. You are more than likely to have something else also included.

### MVP/MVVM

There also exist variations of MVC known as MVP (Model-View-Presenter) and MVVM (Model-View ViewModel).

### When do you need a JavaScript MV* Framework?

When building a single-page application using JavaScript, whether it involves a complex user interface or is simply trying to reduce the number of HTTP requests required for new Views, you will likely find yourself needing many of the features that make up an MV* framework like Angular, Ember or Backbone.

## The History behind JS Frameworks

We refer to the current state of new frameworks frequently popping up as **'Yet Another Framework Syndrome**' (or YAFS).

So it’s important to understand the chronology of these libraries and frameworks to make a good choice.

### Vanilla JavaScript

In the beginning, there was just vanilla JavaScript and almost everyone was afraid to use it because of the inconsistent [DOM APIs](https://developer.mozilla.org/en-US/docs/Web/API) across different browsers which resulted in lots of bugs.

### jQuery

Next, jQuery came along and made DOM manipulation and AJAX calls less risky by abstracting away the differences into one consistent API. As developers began to write more and more JavaScript and jQuery to make their applications more innovative their code quickly became messier and difficult to manage.

#### 1st Generation

- [Backbone](http://backbonejs.org/)
- [Spine](http://spinejs.com/)
- [JavaScriptMVC](http://www.javascriptmvc.com/)
- [Knockout](http://knockoutjs.com/)

[Backbone](http://backbonejs.org/) was created by Jeremy Ashkenas, the creator of Coffeescript. It arrived and provided organization to that messy jQuery code and allowed a new generation of applications to be built in the browser by adding a thin layer on top of the familiar jQuery code developers were already writing.  

Numerous Backbone clones emerged to create a first generation of Javascript front-end libraries including [Spine](http://spinejs.com/) and [JavaScriptMVC](http://www.javascriptmvc.com/) but Backbone survived as the favorite.  

However, developers still found it difficult to be productive and longed for features such as data-binding (the process that establishes a connection between the application User Interface and business logic) that they had used in Rich Internet Application (RIA) technologies like Flex and Silverlight.

[Knockout](http://knockoutjs.com/) came along and provided data-binding but didn’t have any other features that developers needed to build single-page applications.

*Note: 1st Generation in terms of concept, not in terms of release date.*

#### 2nd Generation

- [AngularJS](https://angularjs.org/)
- [Ember](http://emberjs.com/)
- [CanJS](http://canjs.com/)
- [Durandal](http://durandaljs.com/)

During this period more robust JS frameworks were also to coming out.  [AngularJS](https://angularjs.org/) emerged with all the needed framework pieces in one box: data binding, routing, templating, persistence as well as a compelling testing story and a more declarative syntax.

### Angular

**Angular was designed at Google** by [Misko Hevery](http://misko.hevery.com/) and Adam Abrons (who later left the project). Hevery was working on a project at Google called Google Feedback. Hevery and 2 other developers wrote 17,000 lines of code over the period of 6 months for Google Feedback. However, as the code size increased, Hevery began to grow frustrated with how difficult it was to test and modify the code the team had written.

So Hevery made the bet with his manager that he could rewrite the entire application using his side project, GetAngular, in two weeks. Hevery lost the bet. Instead of 2 weeks it took him 3 weeks to rewrite the entire application, but he was able to cut the application from 17,000 lines to 1,500 lines.

### Ember

[Ember](http://emberjs.com/), developed by Yehuda Katz, was also built with similar features to AngularJS but a stronger router and more emphasis on URLs and not breaking the back button.

Other notable frameworks from this second generation are [CanJS](http://canjs.com/) and [Durandal](http://durandaljs.com/).  

Durandal showed innovation and promise but recently announced its plans to merge with AngularJS. CanJS is a slimmed down version of JavaScriptMVC from the first generation with more data binding support.  CanJS just doesn’t seem to have the community around it but remains popular within its niche.

### Which will prevail?

It’s interesting to note that jQuery was the one clear winner in the DOM manipulation library debate over [Scriptaculous](https://script.aculo.us/), [MooTools](http://mootools.net/), and many others.

**Will there be a clear winner in the race for the front-end framework title?**

The first generation of JavaScript MVC Frameworks had one notable survivor in Backbone. After seeing AngularJS merge with Durandal, one can’t help but wonder if Ember will be able to thrive in the long term. However, the Ruby community loves Ember.

## Comparison of Javascript Frameworks

Picking your JavaScript framework isn’t like picking between three different-colored T-shirts. In fact, it’s more like choosing between three completely different articles of clothing:

> Each piece helps to cover you, but they all function and look very differently.

### Angular

- Initially released in 2009, AngularJS is the oldest of the three frameworks. Probably as a result, it also has the largest community.
- Angular has a lot of its own terms and jargon. This could be because the framework includes some features no other JavaScript solution does.
- Angular is supported by Google.

### Ember

- Initially released in 2011.
- LivingSocial, Groupon, Zendesk, Discourse, and Square are some of the most well-known applications that have adopted Ember.
- Very fast because the rendering happens in the browser.

### Backbone

- Backbone came out in June 2010, and its community is nearly as large as Angular’s.
- Many popular applications use the Backbone framework, including Twitter, Foursquare, LinkedIn, Soundcloud, Pitchfork, and Pandora.
- Backbone is tiny at just 6.4K.
- Backbone only relies on one Javascript Library.

#### Summary

You can see a comparison of Javascript Frameworks [here](http://en.wikipedia.org/wiki/Comparison_of_JavaScript_frameworks).

Angular.js and Ember.js are robust and feature-rich front-end frameworks that offer more than Backbone.js. They also allow you to develop applications with less code than Backbone.js.

Nevertheless, I still think developers should learn Backbone.js first, particularly because it is easier to learn and provides you with a solid understanding of what a JavaScript framework is and how a JavaScript framework functions.

## Further reading

- [5 Best JavaScript Frameworks in 2017](https://hackernoon.com/5-best-javascript-frameworks-in-2017-7a63b3870282)
- [The Brutal Lifecycle of JavaScript Frameworks](https://stackoverflow.blog/2018/01/11/brutal-lifecycle-javascript-frameworks/)
- [Comparison of JavaScript frameworks](https://en.wikipedia.org/wiki/Comparison_of_JavaScript_frameworks)
