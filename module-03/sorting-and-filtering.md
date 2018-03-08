# Filtering

Angular has nice some in-built filters that we can implement in our apps. It's important to know that what Angular defines as a filter is not exactly what you'd expect. In Angular terms a filter is used to modify data, so you can use it to change the case of some text for example.

What we'd expect a normal filter to be is Angular's `filterFilter`, but let's start with some simpler ones first.

## Simple Filters

Let's imagine we have a simple RESTful app displaying some restaurant data. The restaurant names in our database may be lowercase, but we could use Angular's uppercase filter to display them in capitals on our page.

```js
<h2>{{ restaurant.name | uppercase }}</h2>
```

That `|` character is a pipe. We are saying pipe the data on the left `restaurant.name` into the `uppercase` filter, which makes the string uppercase.

>**Note:** This can also be done with CSS using `text-transform: uppercase;`

We can also chain filters together. Let's limit the length of the coffee bean name with the `limitTo` filter:

```html
<h2>{{ restaurant.name | uppercase | limitTo: 2 }}</h2>
```

Some filters can take more than one argument. For example, if we want to show not the first two letters of our restaurant name, but the third and fourth, we can add a second argument (offset) to our `limitTo` filter:

```html
<h2>{{ restaurant.name | uppercase | limitTo: 2: 2 }}</h2>
```

Let's try another filter. If we have a restaurant avg. price per head - a number in our database - we can easily turn this into a monetary value with the currency filter:

```html
<p>{{ restaurant.avgPrice | currency }}</p>
```

Now it adds a `$` and zeros to our price to two decimal places. Dollars is the default currency, but we can make it a different currency like so:

```html
<p>{{ restaurant.avgPrice | currency: '£' }}</p>
```

Angular has lots of these helpful filters, take a moment to have a look through the [documentation](https://docs.angularjs.org/api/ng/filter).

## filterFilter

Now on to filtering in the full sense of the word, we're going to use Angular's built-in `filter` filter.

We can apply this filter directly to the `ng-repeat` on our index page and hook it up to a search bar:

```html
<input type="text" class="input" ng-model="restaurantsIndex.q" placeholder="Search...">

<div ng-repeat="restaurant in restaurantsIndex.all | filter: restaurantsIndex.q">
  .
  .
  .
</div>
```

Now as we type in the search box the items are filtered _**as we type!**_ That's pretty amazing.

However, you may notice that this is filtering by _all_ the properties of our data e.g. name, address, rating. This isn't ideal, as users may find it confusing. Let's make it filter by just the name:

```html
<div ng-repeat="restaurant in restaurantsIndex.all | filter: { name: restaurantsIndex.q }">
  .
  .
  .
</div>
```

## Filtering in the Controller

Rather than filtering the data in the view, we can do more if we move our filtering logic to the controller.

Rather than looping over `all` our restaurants, we can filter the data first, then loop over a new `filtered` array.

```js
RestaurantsIndexCtrl.$inject = ['Restaurant', 'filterFilter'];
function RestaurantsIndexCtrl(Restaurant, filterFilter) {
  const vm = this;
  vm.all = [];
  
  Restaurant.find()
    .then(res => vm.all = res.data)
    .then(filterRestaurants);

  function filterRestaurants() {
    vm.filtered = filterFilter(vm.all, vm.q);
  }
}
```

We've injected the `filterFilter` module (as apposed to `uppercaseFilter` or `currencyFilter`) into our controller and created a `filterRestaurants` function which filters `all` based on the search term, and creates a new array called `filtered`.

We must also attach the value of `this` to `vm` so that it continues to refer to the controller when used in the `filterRestaurants` function.

Let's use it in the view:

```html
<div ng-repeat="restaurant in restaurantsIndex.filtered">
  .
  .
  .
</div>
```

Great, but nothing happens when we type in the search box. That's because we're only running the `filterRestaurant` function once when the page loads. We need to run it when the `q` property updates.

## $scope.$watch

First, we need to inject `$scope` into our controller.

Now we can use `$scope.$watch` to literally watch a property, and if it changes, run a function.

```js
$scope.$watch(() => vm.q, filterRestaurants);
```

Let's try our search bar again.

## Combining Filters

We can add multiple form fields to further filter our data. For example, users could search for restaurants by selecting a rating, then filtering the results further by searching for a name.

You could use something like this `select`:

```html
<select name="rating" ng-model="restaurantsIndex.rating">
  <option value="">All</option>
  <option value="1">⭐️</option>
  <option value="2">⭐️⭐️</option>
  <option value="3">⭐️⭐️⭐️</option>
  <option value="4">⭐️⭐️⭐️⭐️</option>
  <option value="5">⭐️⭐️⭐️⭐️⭐️</option>
</select>
```

Now let's update our `filterRestaurants` function:

```js
function filterRestaurants() {
  const params = {};
  if(vm.q) params.name = vm.q;
  if(vm.rating) params.rating = vm.rating;

  vm.filtered = filterFilter(vm.all, params);
```

Now we are saying that we will filter by either name or rating, or both. The if statements mean that the rating and/name filter will only be applied once a value is truthy - i.e. once somebody has typed in the search bar or selected a rating.

## Exact Match Filtering

So far we have been using Angular's `filterFilter` to see if our data _contains_ our query, not to see if there is an exact match. However, this is possible by passing a third argument into the function:

```js
vm.filtered = filterFilter(vm.all, params, true)
```

The `true` passed in here enforces a strict match of the params when filtering. By default, this is set to `false` so we don't normally need to pass it in.

## Further Reading

* [Angular docs](https://docs.angularjs.org/api/ng/filter)





