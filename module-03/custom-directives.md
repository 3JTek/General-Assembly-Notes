# Custom Directives

Angular allow us to make our own directives which is where 3rd-party directives like `uiView` or `uiSref` come from.

## When to use custom directives

Whenever you need to access the DOM in Angular, you shouldn't use `document.querySelector('.cheeses')` or worse `$('.cheeses')`, _because we shouldn't use jQuery with Angular_, instead you should create a custom directive.

You can think of a custom directive as a partial with built in logic. They can be particularly useful when you want to integrate another JavaScript library like Google Maps or FileStack into your apps.

## Usage

Let's take a look at how we might make a Google Maps directive for an angular app.

Firstly we need to create a `directives/google-map.js` file. Inside we can begin to flesh out the directive.

Creating a directive is basically creating a function that returns an object of settings which tells Angular how the directive should behave:

There are loads of settings that can be applied to a directive, but the most common ones are as follows:

| **Setting** | **Meaning** | **Options** |
|-------------|-------------|-------------|
| `restrict` | how the directive can be added to the DOM | `EACM` - Element, Attribute, Class, coMment |
| `replace` | Whether the directive should be replaced with the template or have the template injected inside | `true` / `false` |
| `template` | The HTML that should be used | Any HTML |
| `templateURL` | The path to a HTML file that should be used as the template | Any HTML file |
| `link` | Allows access to the underlying DOM element in the JavaScript | A function |

To start with we will restrict our directive to an element, and make sure that the element is replaced with the template provided:

```js
function GoogleMap() {
  return {
    restrict: 'E',
    replace: true,
    template: '<div class="google-map"></div>'
  }
}

export default GoogleMap
```

With that done we can register our directive in `app.js`:

```js
import angular from 'angular';
import googleMap from './directives/google-map';

angular.module('gMapsDemo', [])
  .directive('googleMap', googleMap);
```

Finally we can use our directive in the view:

```html
<!DOCTYPE html>
<html ng-app="gMapsDemo">
  <head>
    <title>Google Maps Directive Demo</title>
  </head>
  <body>
    <google-map></google-map> <!-- using our custom directive -->
  </body>
</html>
```

Because we have set `replace` to `true`, the `<google-map></google-map>` tag will have been replaced by the template, so in the DOM you should now see this:

```html
<body>
  <div class="google-map"></div>
</body>
```

## Adding Google Maps

In order to add google maps to our project we need to add the script in the head of our `index.html`:

```html
<script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script>
```

>**Note**: You will need to create your own API key using [Google's API Console](https://console.developers.google.com)

Now that we have included the google maps script, the google maps API is available on a global `google` object. So we can now access it in our directive:

```js
console.log(google); // you should see an object in the console like this: { maps: {...} }
function GoogleMap() {
  return {
    restrict: 'E',
    replace: true,
    template: '<div class="google-map"></div>'
  }
}

export default GoogleMap
```

## Accessing the DOM

In order to create a google map we need to pass a DOM element as the first argument like so:

```js
new google.map.Maps(DOMElement, mapSettings);
```

We could use `document.querySelector` to do that, but we _definitely shouldn't!_ Angular is doing a huge amount of DOM manipulation for us so we should definitely not do it ourselves. Instead we can use the directive's `link` function to access the DOM element, as the second argument:

```js
function GoogleMap() {
  return {
    restrict: 'E',
    replace: true,
    template: '<div class="google-map"></div>',
    link($scope, $element) {
      console.log($element); // a JQLite DOM element
    }
  }
}
```

`$element` here is wrapped in a lighter version of jQuery, known as JQLite, so if we want to access the actual JavaScript element we can can use `$element[0]`.

Now we can create our map in the `link` method like so:

```js
function GoogleMap() {
  return {
    restrict: 'E',
    replace: true,
    template: '<div class="google-map"></div>',
    link($scope, $element) {
      new google.maps.Map($element[0], {
        zoom: 14,
        center: { lat: 51.5, lng: -0.07 }
      });
    }
  }
}
```

>**Note**: you must give the element you are loading the map into some height with CSS otherwise you will not see it

## Setting the center and zoom in the HTML

We can set the center onto our directive in the HTML like so:

```html
<google-map center="{ lat: 51.5, lng: -0.07 }"></google-map>
```

In order to pass data from the HTML into our directive we can use the `scope` property:

```js
function GoogleMap() {
  return {
    restrict: 'E',
    replace: true,
    template: '<div class="google-map"></div>',
    scope: {
      center: '='
    },
    link($scope, $element) {

      // This is the data in the `center` attribute of the directive
      console.log($scope.center); // { lat: 51.5, lng: -0.07 }

      new google.maps.Map($element[0], {
        zoom: 14,
        center: { lat: 51.5, lng: -0.07 }
      });
    }
  };
}
```

`scope` is an object which has to have a key that matches the attribute from the directive that we want to access in the directive.

It can take one of three symbols as the value:

| **Symbol** | **Meaning** |
|------------|-------------|
| `=` | Objects / arrays / numbers etc, basically any JavaScript data type |
| `@` | Strings only |
| `&` | Functions only |

Since we want to interpret the data in the `center` attribute of our directive in the HTML as JavaScript we should use the `=` in our scope object:

```js
{
  scope: {
    center: '='
  }
}
```

Now that we have the center, we can use it to center our map:

```js
function GoogleMap() {
  return {
    restrict: 'E',
    replace: true,
    template: '<div class="google-map"></div>',
    scope: {
      center: '='
    },
    link($scope, $element) {
      new google.maps.Map($element[0], {
        zoom: 14,
        center: $scope.center
      });
    }
  };
}
```

## Overview

Directives in Angular are very powerful. They are a way for us to not only interact directly with the DOM, but also to be able to parcel up logic and HTML together in a module which can be used anywhere in our app.

**If you ever find yourself using `document.getElementById`, or `document.querySelector` you should be using a custom directive**

## Further reading

- [Creating custom AngularJS directives for beginners](http://adrianmejia.com/blog/2016/04/08/creating-custom-angularjs-directives-for-beginners/)
- [Creating Custom AngularJS Directives Series](https://weblogs.asp.net/dwahlin/creating-custom-angularjs-directives-part-i-the-fundamentals)
