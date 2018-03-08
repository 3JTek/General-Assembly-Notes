# UI-Router

Angular has a large community, and as a result there is a huge amount of 3rd-party libraries and plugins that can be used in our Angular apps.

One of the most commonly used is UI-Router, which is used to allow us to "change pages" inside our Angular app.

In an Angular app we never actually change pages, because the whole idea of using a frontend framework is to not have to make extra HTTP requests. Instead UI-Router simply changes the url, and the removes the existing HTML, and replaces it with new HTML and optionally a new controller.


## Setup

UI-Router can be installed with Yarn:

```sh
yarn add @uirouter/angularjs
```

It should then be imported into `app.js` then added to your module's dependencies:

```js
import angular form 'angular';
import '@uirouter/angularjs';

angular.module('myAwesomeApp', ['ui.router']); // add UI-Router to module's dependencies
```

## Creating states

Now we need to create some `states` (or "pages") for us to be able to navigate to. Generally we would do this in `src/config/router.js`. Each state has a name, followed by an object of settings:

```js
Router.$inject = ['$urlRouterProvider', '$stateProvider'];
function Router($urlRouterProvider, $stateProvider) {
  $stateProvider
    .state('home', { // this state's name is `home`
      url: '/', // when the browser has this URL, the home state will be rendered
      templateUrl: 'views/home.html', // this is the template file that will be loaded
      controller: 'HomeCtrl as home' // this is the controller that will be attached to the template
    })
    .state('about', {
      url: '/about',
      templateUrl: 'views/about.html',
      controller: 'AboutCtrl as about'
    });

  $urlRouterProvider.otherwise('/');
}
```

And register it using `.config` in `app.js`:

```js
import angular from 'angular';
import Router from './config/router';
import HomeCtrl from './controllers/home';
import AboutCtrl from './controllers/about';

angular.module('myAwesomeApp', ['ui.router'])
  .config(Router)
  .controller('HomeCtrl', HomeCtrl)
  .controller('AboutCtrl', AboutCtrl);
```

In the example above we have created two _states_ that our app can be in.

We can be in the `home` state, where the URL would be `/` and the page would be displaying the `home` template connected to the `HomeCtrl` controller.

Or we can be in the `about` state, where the URL would be `/about` and we would see the `about` template connected to the `AboutCtrl` controller.

## `uiView`

We now need to let UI-Router know where on our `index.html` to inject the templates for each state. To do that we will use UI-Router's `uiView` directive:

```html
<!DOCTYPE html>
<html ng-app="myAwesomeApp">
  <head>
    <title>My Awesome App</title>
  </head>
  <body>
    <ui-view></ui-view> <!-- templates will be injected here -->
  </body>
</html>
```

## Template files

Finally we need to create some files in `src/views`. Inside the view files, we _do not_ need to use `ngController` to connect the controller, as UI-Router has done this for us:

```html
<section>
  <!-- `home` has been made available in the state's controller setting in the router -->
  <h1>{{ home.title }}</h1>
</section>
```

## Changing states

There are two ways we can change state. In the view and in the controller.

### In the view

In the view we can use UI-Router's `uiSref` directive on a link:

```html
<section>
  <!-- `home` has been made available in the state's controller setting in the router -->
  <h1>{{ home.title }}</h1>
  <a ui-sref="about">About Page</a>
</section>
```

Note that `uiSref` does not take a URL, but instead the name of the state we want to go to. In the example above the `about` state will be rendered when we click the link.

### In the controller

If we want to redirect a user on form submission or button click in the controller we can use UI-Router's `$state` module:

```js
ContactCtrl.$inject = ['$state'];
function ContactCtrl($state) {

  this.user = {};

  function handleSubmit() {
    console.log('form has submitted...', this.user);
    $state.go('home'); // once the form is submitted render the `home` state
  }

  this.handleSubmit = handleSubmit;
}

export default ContactCtrl;
```

Again, note that we are not suppling the URL we want to go to, but the name of the state we want to render.

## Accessing URL parameters

We may want to use a parameter in our URLs, particularly if we want to create a RESTful Angular app.

A SHOW state for example would probably have a URL containing an ID, like so: `/cheeses/5a8d472e82373c1d76588a7e`. We could access that ID inside the controller like so:

```js
Router.$inject = ['$urlRouterProvider', '$stateProvider'];
function Router($urlRouterProvider, $stateProvider) {
  $stateProvider
    .state('home', {
      url: '/',
      templateUrl: 'views/home.html',
      controller: 'HomeCtrl as home'
    })
    .state('cheesesShow', {
      url: '/cheeses/:id',
      templateUrl: 'views/cheeses/show.html',
      controller: 'CheeseShowCtrl as cheesesShow'
    });

  $urlRouterProvider.otherwise('/');
}
```

Now that we have set up an `id` parameter in the controller, we can access that portion of the URL inside the controller using `$state.params`:

```js
CheeseShowCtrl.$inject = ['$state'];
function CheeseShowCtrl($state) {
  // this would be the ID portion of the URL
  // '5a8d472e82373c1d76588a7e' for example
  console.log($state.params.id)
}
```

This could now be used in conjunction with an AJAX request to GET the relevant data from an API.

## HTML5 mode

By default UI-Router uses hash bangs to allow the URL to change without causing the browser to make further requests to our server. You'll notice the `#!` in front of the state's URL in the browser's address bar.

It is possible to remove these using UI-Router's HTML5 mode, which switches from using hash bangs to HTML5's history API.

This requires modifying the router file, and the server side code.

Firstly update the router as follows:

```js
Router.$inject = ['$locationProvider', '$urlRouterProvider', '$stateProvider'];
function Router($locationProvider, $urlRouterProvider, $stateProvider) {

  $locationProvider.html5Mode({
    enabled: true,
    requireBase: true
  });

  $stateProvider
    .state('home', {
      url: '/',
      templateUrl: 'views/home.html',
      controller: 'HomeCtrl as home'
    })
    .state('cheesesShow', {
      url: '/cheeses/:id',
      templateUrl: 'views/cheeses/show.html',
      controller: 'CheeseShowCtrl as cheesesShow'
    });

  $urlRouterProvider.otherwise('/');
}
```

Now inside the server's main `index.js` file, we need to serve the `index.html` file regardless of which URL is requested by the browser:


```js

app.use('/api', router);

// send the index.html page for any non API related requests
app.get('/*', (req, res) => res.sendFile(`${__dirname}/public/index.html`));

app.listen(port, () => console.log(`Aye aye captain, pulling into port ${port}`));
```

## Further reading

- [UI-Router for AngularJS (1.x)](https://ui-router.github.io/ng1/)
- [UI-Router Quick Reference](https://github.com/angular-ui/ui-router/wiki/quick-reference)
