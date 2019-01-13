# Array Methods

Arrays are amazingly useful and can be used to solve many challenging problems. Most of the time, we need to _iterate_ over the array using some logic to modify it in some way. We could use a `for` loop, but there are a number of in-build array methods that can do a lot of the heavy lifting for us.

Here are a few of the more common ones:

## `.forEach`

This is basically short hand for using a `for` loop to iterate over an array:

```js
const array = ['mars', 'twix', 'curly wurly']
array.forEach((element, index, array) => {
  console.log(element, index, array)
})
// "mars"  0  ["mars", "twix", "curly wurly"]
// "twix"  1  ["mars", "twix", "curly wurly"]
// "curly wurly"  2  ["mars", "twix", "curly wurly"]
```

This is very useful for displaying the contents of an array.

## `.filter`

This will iterate over an array testing each element as it goes. The elements that fail the test will be filtered out. _This method creates a new array, leaving the original one intact._

```js
const array = ['test tube', 'house', 'car']
array.filter((element, index, array) => {
  return element.length > 3
})
// ["test tube", "house"]

array.filter((element, index, array) => {
  return index > 0
})
// ["house", "car"]
```

Very useful for updating a collection based on user input, like displaying only the trainers of a specific colour on a shoe store website.

## `.reduce`

This will iterate over an array and return a single value from it.

```js
const array = [10, 5, 78, 56]
array.reduce((sum, amount) => {
  return sum + amount
}, 0)
// 149
```

The syntax is as follows:

```js
array.reduce(func, [initialValue])
```

Where `func` is a function to run on each iteration, and `initialValue` is the value to start from.

The function itself is passed two arguments from the array: a variable containing the running value, and the current element of the array.

In the example above the `initalValue` is set to 0. The function is run once per element in the array. The values update as follows:

| **Iteration** | **`sum`** | **`amount`** |
|-----|---|----|
| 1st | 0 | 10 |
| 2nd | 10 | 5 |
| 3rd | 15 | 78 |
| 4th | 93 | 56 |

Finally 93 is added to 56 to give the sum of all the elements in the array - 149

Although a little tricky to get your head round initially, this is a very useful function, particularly for working out the total cost of the contents of a shopping cart for example.

## `.map`

This will perform some function against each element in the array. _This method creates a new array, leaving the original one intact._

```js
const array = [5, 10, 15, 20]
array.map((element, index, array) => {
  return element * 2
})
// [10, 20, 30, 40]
```

This is particularly useful for updating the contents of an array based on some user input, like removing VAT from each product in a shopping cart.

## `.includes`

This will return a boolean based on whether the specified value is in the array:

```js
const array = ['banana', 'hedgehog', 'lightbulb']
array.includes('hedgehog') // true
array.includes('handbag') // false
```

## `.slice`

This will return a subset or chunk of an array. _This method creates a new array, leaving the original one intact._

```js
const array = [100, 'Henry VIII', NaN, 'Sarah']
array.slice(1, 3) // ["Henry VIII", NaN]
```

The syntax is as follows:

```js
array.slice([startIndex, endIndex])
```

In the example above a new array was created with the elements from index 1 (Henry VII), up to, but not including index 3 (Sarah)

This is very useful if you need to remove elements from the start _and_ the end of an array at the same time for example if you need to select a collection of days from a calendar.

## `.indexOf`

This will return the index of an element in an array when given the element:

```js
const array = ['milkshake', 'spoon', 'bee']
array.indexOf('spoon') // 1
array.indexOf('telephone') // -1
```

This can be used in a similar way to `includes`, but with the added information of specifically where in the array the element sits. Can be very powerful when combined with `slice`.

## Further reading

* [Array - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
* [5 Array Methods You Should Be Using Now](https://colintoh.com/blog/5-array-methods-that-you-should-use-today)
* [Most Useful JavaScript Array Functions](http://vegibit.com/most-useful-javascript-array-functions/)
* [Map - Part 2 of Functional Programming in JavaScript](https://www.youtube.com/watch?v=bCqtb-Z5YGQ)
* [Reduce Basics - Part 3 of Functional Programming in JavaScript](https://www.youtube.com/watch?v=Wl98eZpkp-c)
