# Angular Models

Angular is an MVC framework. Controllers and views are trivial to make, however making models is a little more involved.

## Using a service as a model

Angular has a concept known as a service, which is a module which only ever loaded once into memory. This is known as a _singleton_ pattern.

This service can then be injected into any controller. Controllers are not singletons, and get created and destroyed frequently throughout the lifecycle of our app. The service however is only loaded once, then is reused wherever it is needed.

Let's have a look at how we would create a model for a `place`. We'll create a file in `src/services/place.js` and add the following code:

```js
Place.$inject = ['$http'];
function Place($http) {

  function find() {
    return $http.get('/api/places');
  }

  function findById(id) {
    return $http.get(`/api/places/${id}`);
  }

  function create(data) {
    return $http.post('/api/places', data);
  }

  function update(place, data) {
    return $http.put(`/api/places/${place._id}`, data);
  }

  function remove(place) {
    return $http.delete(`/api/places/${place._id}`);
  }

  this.find = find;
  this.findById = findById;
  this.update = update;
  this.remove = remove;
}

export default Place;
```

Here we are using Angular's `$http` module to send AJAX requests. We have created 5 methods, once for each route in our API. This way we can keep all of the logic required for interacting with our API in one place.

Let's add it into our app in `src/app.js`:

```js
import Place from './services/place';

angular.module('myAwesomeApp', [])
  .service('Place', Place);
```

We can now inject it into our controllers. We'll use the INDEX controller as an example:

```js
PlacesIndexCtrl.$inject = ['Place'];
function PlacesIndexCtrl(Place) {
  const vm = this;

  Place.find()
    .then(res => vm.place = res.data);
}
```

Rather than injecting `$http` into our controllers and writing logic there to get data from the database, we can keep all of that logic in our model, which will only ever be loaded into memory once.
