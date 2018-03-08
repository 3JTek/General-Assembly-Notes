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











