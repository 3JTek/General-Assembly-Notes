# Angular Directives

The whole idea behind angular is that it is _data driven_. Each view is connected to some data in a controller. As the underlying data changes the view will automatically respond.

The way that we connect the controller to the view is via angular directives. Some common directives you will use are the following:

| **Directive** | **Purpose** |
|---------------|-------------|
| `ngApp` | Connects a module to a specific DOM node |
| `ngController` | Connects a controller to a specific DOM node |
| `ngRepeat` | Repeats a DOM node based on the length of an array |
| `ngSubmit` | Attaches a method to the submit event of a form |
| `ngClick` | Attaches a method to the click event of a button or link |
| `ngModel` | Connects a form input to some data in the controller |

## Usage

### `ngApp`

`ngApp` is used to connect your angular app to the DOM. It is possibly the most important directive, because without it none of your code will run.

It can be attached to any DOM node, but most commonly it is added to the HTML tag:

```html
<!DOCTYPE html>
<html ng-app="myAwesomeApp">
  <head>
    <title>My Awesome App</title>
  </head>
  <body>
    <h1>My Awesome App</h1>
  </body>
</html>
```

The value passed to `ngApp` (in this case `myAwesomeApp`) has to match the name of the app specifiec in the `angular.module` declaration in your `app.js` file:

```js
import angular from 'angular';

angular.module('myAwesomeApp', []);
```

### `ngController`

`ngController` will link a specific DOM node to a controller. It's important to understand that the controller is _only available_ to the specific DOM node:

```html
<section ng-controller="MainCtrl as main">
  <h1>{{ main.title }}</h1> <!-- main controller is available here -->
</section>

<section>
  <h2>{{ main.subtitle }}</h2> <!-- main controller is NOT available here -->
</section>
```

It's important to note that before a controller can be used in the view, it must first be registered in `app.js`:

```js
import angular from 'angular';
import MainCtrl from './controllers/main';

angular.module('myAwesomeApp', [])
  .controller('MainCtrl', MainCtrl);
```

#### ControllerAs syntax

Whenever we hook a controller up to a DOM node we should give the controller a name. Controllers are _constructor functions_ and are instantiated when they are hooked up to the DOM. Using the controllerAs syntax we can provide a variable name for the instance created. In this way we can refer to the controller when we want to access its properties and methods.

When we create a controller, `this` will refer to the controller. When we use a controller in the view the name provided after the `as` will refer to the controller:

```js
function MainCtrl() {
  this.title = 'My Awesome Angular Project' // `this` is a reference to the controller
}

export default MainCtrl;
```

```html
<section ng-controller="MainCtrl as main"> <!-- `main` is a reference to the controller -->
  <h1>{{ main.title }}</h1>
</section>
```

### `ngRepeat`

`ngRepeat` will repeat a DOM node once for each element in an array. You can think of it like the `.forEach` array method. You can set a variable name for the element you are currently iterating over like so:

```js
function MainCtrl() {
  this.things = ['elephant', 'marshmallow', 'apple', 'wardrobe'];
}

export default MainCtrl;
```

```html
<section ng-controller="MainCtrl as main">
  <ul>
    <!-- `thing` is the element in the array -->
    <li ng-repeat="thing in main.things">{{ thing }}</li>
  </ul>
</section>
```

## `ngSumbit`

`ngSubmit` should be added to a form, and should be provided a method to be called when the form is submitted. Unlike a usual submit handler, _with angular we do not have to prevent the default behaviour_.

```js
function MainCtrl() {

  this.user = {};

  function handleSubmit() {
    console.log('form has submitted...', this.user);
  }

  this.handleSubmit = handleSubmit;
}

export default MainCtrl;
```

```html
<section ng-controller="MainCtrl as main">
  <form ng-submit="main.handleSubmit()">
    <input name="name" ng-model="main.user.name" placeholder="Name" />
    <button>Submit</button>
  </form>
</section>
```

### `ngModel`

One of the more useful directives `ngModel` is used to connect a form input to a controller. Whenever we use `ngModel` we make a two-way binding between the view and the controller.

Whenever we change the data in the controller, it will automatically be updated in the view, and whenever we change the data in the view (ie type in a input field) it will automatically update the data in the controller:

```js
function MainCtrl() {
  // as the user types into the `name` input field it will be added to this object
  this.user = {};
}

export default MainCtrl;
```

```html
<section ng-controller="MainCtrl as main">

  <form>
    <!-- `main.user.name` is the property that will be updated as the user types into this field -->
    <input name="name" ng-model="main.user.name" placeholder="Name" />
    <button>Submit</button>
  </form>

</section>
```

## Further reading

- [Directive Components in ng](https://docs.angularjs.org/api/ng/directive)
- [AngularJS Built in Directives](http://www.techstrikers.com/AngularJS/angularjs-built-in-directives.php)
