# Sorting and Filtering

React is a library, not a framework. It attempts to solve the problem of rendering and updating UI components quickly to the DOM. It does not have any support for filtering or sorting out of the box.

JavaScript has two rather useful methods: [`Array.sort`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) and [`Array.filter`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

## `sort()`

The [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) method takes a comparison argument, which needs to return a positive number, a negative number, or `0`, which indicates whether the element being inspected should be moved up, down or kept in the same place inside the array being sorted.

`sort()` takes one argument, a callback function, which provides two arguments, generally referred to as `a` and `b`. They are the two elements being compared.

Without arguments, `sort` will order an array alphabetically, but **not numerically!**

#### A simple example

```js
['zero', 'helicopter', 'jaguar', 'apple']
  .sort() // ['apple', 'helicopter', 'jaguar', 'zero'];

[100, 23, 81, 90]
  .sort((a, b) => a - b) // [23, 81, 90, 100]

[{ a: 100 }, { a: 23 }, { a: 81 }, { a: 90 }]
  .sort((first, second) => second.a - first.a);
// [{ a: 100 }, { a: 90 }, { a: 81 }, { a: 23 }]
```

## `filter()`

The [`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) method creates a new array with all the elements that pass the test provided by the callback function.

#### A simple example

```js
['zero', 'helicopter', 'jaguar', 'apple']
  .filter(word => word.length > 5) // ['helicopter', 'jaguar']

[100, 23, 81, 90]
  .filter(number => number % 2 === 0) // [100, 90]

[{ a: 100 }, { a: 23 }, { a: 81 }, { a: 90 }]
  .filter(object => object.a % 2 === 1)
// [{ a: 23 }, { a: 81 }]
```

<br>

## Issues with `sort()` and `filter()`

Unfortunately the sort method is a little unsuitable for a few reasons:

* It _mutates_ (or modifies) the original array.
* The sort algorithm its _not stable_, ie. if you perform the same sort on the same collection it may order the elements in a slightly different order.

The filter method is better:

* It creates a new array (ie it is _immutable_).
* It is _stable_.

However the syntax can be quite long-winded. Ideally we want something more straight forward and versitile for building a complex web application.

<br>

## `Lodash`

[Lodash](https://lodash.com/docs/4.17.4), often written simply `_`, is a 3rd party utility library which aims to improve some of the native JavaScript functions for dealing with arrays, numbers, objects and strings.

## `_.sortBy()`

Loadsh's [`sortBy()`](https://lodash.com/docs/4.17.4#sortBy) method is similar to `sort()`, except it is _stable_ and _immutable_. It also works well with arrays of objects.

It takes two agruments:

* The array to sort
* The properties to sort by

**However**: It always sorts in ascending order.

#### A simple example

```js
const users = [
  { name: 'joanne', age: 48 },
  { name: 'fred', age: 36 },
  { name: 'joanne', age: 34 },
  { name: 'fred', age: 40 }
];

_.sortBy(users, ['name', 'age']);

/*
const users = [
  { name: 'fred', age: 36 },
  { name: 'fred', age: 48 },
  { name: 'joanne', age: 34 },
  { name: 'joanne', age: 48 }
];
*/
```

## `_.orderBy()`

[`orderBy()`](https://lodash.com/docs/4.17.4#orderBy) is similar, except you can specify the order to sort.

It takes three agruments:

* The array to sort
* The properties to sort by
* The orders to sort each property

#### A simple example

```js
const users = [
  { name: 'joanne', age: 48 },
  { name: 'fred', age: 36 },
  { name: 'joanne', age: 34 },
  { name: 'fred', age: 40 }
];

_.orderBy(users, ['name', 'age'], ['desc', 'asc']);

/*
const users = [
  { name: 'joanne', age: 34 },
  { name: 'joanne', age: 48 },
  { name: 'fred', age: 36 },
  { name: 'fred', age: 48 }
];
*/
```

<br>

## `_.filter()`

[`filter()`](https://lodash.com/docs/4.17.4#filter) is very similar to the native JavaScript method, but you can pass a function, object or an array.

#### A simple example

```js
const users = [
  { name: 'fred', age: 36 },
  { name: 'joanne', age: 34 },
  { name: 'fred', age: 40 },
  { name: 'joanne', age: 48 }
];

_.filter(users, (user) => user.age > 36);
/*
[
  { name: 'fred', age: 40 },
  { name: 'joanne', age: 48 }
];
*/

_.filter(users, { name: 'fred' });
/*
[
  { name: 'fred', age: 36 },
  { name: 'fred', age: 40 }
];
*/


_.filter(users, ['age', 40]);
/*
[
  { name: 'fred', age: 40 }
];
*/
```

<br>

## Partial matches with `RegExp`

So for we have looked at how to filter collections for **exact** matches. But what if we want to make a **partial** match.

Often a user will begin typing in a search box, and we will want to filter the results as they type. To do that we need to use **regular expressions**.

Regular expressions, often referred to as _regex_ are used to match patterns of characters within strings.

They look a little odd. They take the following format:

`/[pattern]/[flags]`

#### Some examples

* `/Mike/` would to match the word "Mike" in a string
* `/[a-z]/` would match any single lower case letter between a-z
* `/[a-z]/i` would match any single letter between a-z. The `i` makes the match case insensitive.

Although they look weird they are infact a shorthand for the constructor function `RegExp`. The same regex using the constructor would look like this:

* `new RegExp('Mike')`
* `new RegExp('[a-z]')`
* `new RegExp('[a-z]', 'i')`

#### RegExp methods

The two main methods you can use with regex are `test` and `match`.

## `test()`

`test()` takes a string as an argument and returns a boolean depending on whether the test passes or not

#### A simple example

```js
/jo/.test('Joanne') // false - the `j` in `Joanne` is uppercase

/jo/i.test('Joanne') // true - the `i` flag makes the test case insensitive

/ja/i.test('Joanne') // false - because `j` and `a` do not sit together in `Joanne`
```

## `match()`

`match()` is a string method that takes a regex as an argument. It returns the matches it finds in an array.

#### A simple example

```js
'Joanne'.match(/jo/) // null - there is no match

'Joanne'.match(/jo/i) // ["Jo", index: 0, input: "Joanne"]
// 'Jo' is the actual match
// `index` is the starting position of the match in the string
// `input` is the whole string that was matched

'Joanne'.match(/nne/) // ["nne", index: 3, input: "Joanne"]
```

## Combining `RegExp` with `_.filter()`

We can use a regex in combination with a filter method to be more inclusive when we filter.

#### A simple example

```js
const users = [
  { name: 'fred', age: 36 },
  { name: 'joanne', age: 48 },
  { name: 'john', age: 19 },
  { name: 'sanjay', age: 32 }
];

// Return all users with names that conatain the letter 'j'
_.filter(users, (user) => /j/.test(user.name));
/*
const users = [
  { name: 'joanne', age: 48 },
  { name: 'john', age: 19 },
  { name: 'sanjay', age: 32 }
];
*/

_.filter(users, (user) => /a/.test(user.name));
/*
const users = [
  { name: 'joanne', age: 48 },
  { name: 'sanjay', age: 32 }
];
*/

// Return all users with names that conatain the letters 'jo' consecutively
_.filter(users, (user) => /jo/.test(user.name));
/*
const users = [
  { name: 'joanne', age: 48 },
  { name: 'john', age: 19 }
];
*/
```

## Conclusion

All of these things are important because they are not specific to a single framework, library or language. Sorting, filtering and matching strings is bread and butter to a web developer, or indeed any kind of programmer.

The more of a handle you have on these things the more capable you are of solving problems and dealing with code tests when you apply for Junior Developer roles.

We will look at using this functionality in a real-world React application next.

## Further reading

- [Filtering Arrays with Array#filter](http://adripofjavascript.com/blog/drips/filtering-arrays-with-array-filter)
- [Quick Tip: How to Sort an Array of Objects in JavaScript](https://www.sitepoint.com/sort-an-array-of-objects-in-javascript/)
- [Regular Expressions - MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Regular_Expressions)
- [Regex Golf](https://alf.nu/RegexGolf)
- [Online Regex Tester](https://regex101.com/)
