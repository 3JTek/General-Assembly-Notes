# Token Authentication - Part 2: Client-side

When using token authentication on the client side we need to handle the token being sent from the server during login, store it somewhere, then send it to the server in our subsequent AJAX requests.

This is fairly trivial to do with vanilla JavaScript, but for this module, we'll be using a 3rd-party plugin called Satellizer.

## Adding Satellizer

Firstly install with yarn:

```js
yarn add satellizer
```

Then add it to your Angular project in `src/app.js`:

```js
import angular from 'angular';
import 'satellizer';

angular.module('myAwesomeApp', ['satellizer'])
```

We now need to configure Satellizer to point to our authentication endpoints, in `src/config/auth.js`:

```js
Auth.$inject = ['$authProvider'];
function Auth($authProvider) {
  $authProvider.signupUrl = '/api/register';
  $authProvider.loginUrl = '/api/login';
}

export default Auth;
```

We can now add this to our app in `src/app.js` in a `config` block:

```js
import angular from 'angular';
import 'satellizer';

import Auth from './config/auth';

angular.module('myAwesomeApp', ['satellizer'])
  .config(Auth)
```

Now that we have configured Satellizer, we can use it in our `AuthLoginCtrl` like so:

```js
AuthLoginCtrl.$inject = ['$auth', '$state', '$rootScope'];
function AuthLoginCtrl($auth, $state, $rootScope) {
  this.credentials = {};

  function handleSubmit() {
    $auth.login(this.credentials)
      .then(res => $state.go('placesIndex'));
  }

  this.handleSubmit = handleSubmit;
}

export default AuthLoginCtrl;
```

Satellizer is doing a lot of work for us here. It is making a `POST` request to `/api/login` and looking for a `token` in the response. If it finds one, it will store it in local storage, then add it to all subsequent AJAX requests in an Authorization header.

Once you log in you should be able to see a token in the Application > Local Storage tab of Chrome's dev tools:

![](https://user-images.githubusercontent.com/3531085/37478416-14b8057c-2872-11e8-88e8-c03d190d13a0.png)

You should also see an Authorization header with the token in any AJAX request made by `$http` in the Network tab:

![](https://user-images.githubusercontent.com/3531085/37478577-7ee80c30-2872-11e8-9a7c-2628b3745b02.png)

## Preventing unauthenticated users from accessing certain states

Just like in our API we want to ensure that only logged in users are able to see certain states in our Angular app. The NEW and EDIT states should not be accessible to anyone who has not logged in.

In order to do that we will make a `secureState` function in our client-side router:

```js
secureState.$inject = ['$q', '$state', '$auth'];
function secureState($q, $state, $auth) {
  return new $q(resolve => {
    // if the user is authenticated, resolve the promise, allowing UI Router to load the desired state
    if ($auth.isAuthenticated()) return resolve();

    // otherwise load the login state
    $state.go('login');
  });
}
```

The `secureState` returns a promise that resolves _if the user is authenticated_. We are using `$q` here, which is Angular's built in promise library.

`$auth.isAuthenticated` is a method provided by Satellizer that will return true if a user is logged in, and false otherwise.

We can now add this function to any state that we want to protect:

```js
.state('placesNew', {
  url: '/places/new',
  templateUrl: 'views/places/new.html',
  controller: 'PlacesNewCtrl as placesNew',
  resolve: { secureState } // the secureState promise must resolve before we load the `placesNew` state
})
```

## Adding flash messages

Finally if we are going to redirect our users, it's important that they understand why they have been redirected. To do that we will recreate flash messages in Angular.

### `$rootScope`

`$rootScope` is like a global `$scope`, which can be injected into any controller, and can be used to send and receive messages between controllers.

If we want to send a message, we can use `$rootScope.$broadcast`, if we want to receive a message, we use `$rootScope.$on`.

Let's send a message in our `secureState` function if the user is redirected:

```js
secureState.$inject = ['$q', '$state', '$auth', '$rootScope'];
function secureState($q, $state, $auth, $rootScope) {
  return new $q(resolve => {
    if ($auth.isAuthenticated()) return resolve();

    $rootScope.$broadcast('flashMessage', {
      type: 'danger',
      content: 'You must be logged in.'
    });

    $state.go('login');
  });
}
```

We now need to display that message. To do that we will create a `MainCtrl` in `src/controllers/main.js`:

```js
MainCtrl.$inject = ['$rootScope', '$state', '$timeout'];
function MainCtrl($rootScope, $state, $timeout) {
  this.flashMessage = null;

  // when the flashMessage is broadcast, it will be received here
  $rootScope.$on('flashMessage', (e, data) => {
    this.flashMessage = data; // we add the message data to the controller
    $timeout(() => this.flashMessage = null, 2000); // we remove the message after 2 secs
  });
}

export default MainCtrl;
```

Finally we just need to display the message in the main `index.html`:

```html
<div ng-if="main.flashMessage.content" class="notification is-{{ main.flashMessage.type }}">
  {{ main.flashMessage.content }}
</div>
```

## Showing and hiding links

We can also show and hide links and buttons depending on whether the user is logged in using Satellizer's `isAuthenticated` method, by attaching it to the `MainCtrl`:

```js
MainCtrl.$inject = ['$rootScope', '$state', '$timeout', '$auth'];
function MainCtrl($rootScope, $state, $timeout, $auth) {

  this.isAuthenticated = $auth.isAuthenticated;
  this.flashMessage = null;

  // when the flashMessage is broadcast, it will be received here
  $rootScope.$on('flashMessage', (e, data) => {
    this.flashMessage = data; // we add the message data to the controller
    $timeout(() => this.flashMessage = null, 2000); // we remove the message after 2 secs
  });
}

export default MainCtrl;
```

Now we can use it anywhere we like to render a DOM element conditionally with `ngIf`, for example in the navbar:

```html
<a class="navbar-item" ui-sref="placesIndex" ng-if="main.isAuthenticated()">All Places</a>
<a class="navbar-item" ui-sref="placesNew" ng-if="main.isAuthenticated()">New Places</a>
<a class="navbar-item" ui-sref="login" ng-if="!main.isAuthenticated()">Login</a>
<a class="navbar-item" ui-sref="register" ng-if="!main.isAuthenticated()">Register</a>
```

## Logging out

Logging out is as simple as removing the token from local storage, which Satellizer will do for us with its `.logout` method:

```js
MainCtrl.$inject = ['$rootScope', '$state', '$timeout', '$auth'];
function MainCtrl($rootScope, $state, $timeout, $auth) {

  this.isAuthenticated = $auth.isAuthenticated;
  this.flashMessage = null;

  $rootScope.$on('flashMessage', (e, data) => {
    this.flashMessage = data;
    $timeout(() => this.flashMessage = null, 2000);
  });

  function logout() {
    $auth.logout(); // remove the token from local storage
    $state.go('home'); // redirect the user to the homepage
  }

  this.logout = logout;

}

export default MainCtrl;
```

We can now add this into the navbar with a simple `ngClick` directive:

```html
<a class="navbar-item" ng-click="main.logout()" ng-if="main.isAuthenticated()">Logout</a>
```

## Further reading

- [Satellizer Docs](https://github.com/sahat/satellizer)
- [How to use Local Storage with JavaScript](https://www.taniarascia.com/how-to-use-local-storage-with-javascript/)
