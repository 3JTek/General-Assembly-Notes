# Arrays

Arrays are a fundamental part of any programming language. They are essentially used to store collections of other data types _in a specific order_.

Consider a shopping list:

1. Toothpaste
1. Ham
1. Mayonnaise
1. Washing liquid
1. Cucumber

This is a collection of objects in a specific order. It can be expressed as an array in JavaScript like so:

```js
const shoppingList = [
  'Toothpaste',
  'Ham',
  'Mayonnaise',
  'Washing liquid',
  'Cucumber'
]
```

If we were to log this array out to the console, it would look like this:

![](https://user-images.githubusercontent.com/3531085/35188946-75724cd8-fe38-11e7-9c62-d538fa4dbbad.png)

Each item of the list is accompanied by a number, beginning at zero, known as an _index_. The index is used to indicate the order or position that each item or _element_ has in the array.

The index can be used to access data from the array.

```js
shoppingList[0] // => "Toothpaste"
shoppingList[2] // => "Mayonnaise"
shoppingList[5] // => undefined
```

## An array is an object

In fact all complex data types are objects, they're just different types or subsets of the object data type.

With an object we have a key/value pair, where the key is generally a string. An array is very similar, except the keys are integers, and the array has specialised in-build functionality for iterating over its contents in different ways.

They are great for storing lists, or collections, and can be sorted and filtered to make interesting applications.

## Creating an array

There are two main ways to create an array in JavaScript:

1. Array literal method:
  ```js
  const array = ['bath', 25, false]
  ```
1. Constructor method:
  ```js
  const array = new Array()
  array[0] = 'bath'
  array[1] = 25
  array[2] = false
  ```

Both examples produce the same result, but the array literal method is generally cleaner and simpler to work with, since you can use it to create an array _with contents_.

## Accessing data

To access a specific element, you can use that element's index to access the data:

```js
array[0] // => "bath"
array[1] // => 25
```

You can also use a loop to access each element one at a time. The `.length` property of the array will give you the number of elements in the array. Since the last element is always one less than the length, that can be used to stop the loop.

#### With `for`
```js
for(let i=0 i<array.length i++) {
  console.log(array[i])
}
// => "bath"
// => 25
// => false
```

#### With `while`
```js
let i = 0
while(i < array.length) {
  console.log(array[i])
  i++
}
// => "bath"
// => 25
// => false

let length = array.length
while(length--) {
  console.log(array[length])
}
// => false
// => 25
// => "bath"
```

#### With `forEach`
```js
array.forEach(element => console.log(element))
// => "bath"
// => 25
// => false
```

## Modifying data

Using the index, it is possible to overwrite the data at a specific element:

```js
array[0] = 'kitchen' // => ["kitchen", 25, false]
```

Similarly in a loop:

```js
for(let i=0 i<array.length i++) {
  array[i] = "Aardvark"
}
// => ["Aardvark", "Aardvark", "Aardvark"]
```

## Adding elements

Data can be added with the array method `push` to add elements to the end of the array:

```js
array.push('Candle') // => ["Aardvark", "Aardvark", "Aardvark", "Candle"]
```

With `unshift` to add elements to the start of the array:

```js
array.unshift(NaN) // => [NaN, "Aardvark", "Aardvark", "Aardvark", "Candle"]
```

## Removing elements

Data can be removed from the end of the array with `pop`:

```js
array.pop() // => [NaN, "Aardvark", "Aardvark", "Aardvark"]
```

With `shift` to remove from the start of the array:

```js
array.shift() // => ["Aardvark", "Aardvark", "Aardvark"]
```

## A practical use case

A good example of a typical use for an array is an eCommerce website. Take Amazon as an example. In order to build the Amazon website, you would need to create an object representing each product. Each object would have various properties including `name`, `description`, `price`, `image` etc.

Each product object would belong to a category, say _electronics_ or _books_. This category of products is essentially a **collection of objects**. An array would be a great container for those objects, it is easy to add, remove, sort and filter the objects on the fly as the user interacts with the website.

![](https://user-images.githubusercontent.com/3531085/35189299-83de905e-fe3f-11e7-8fb2-b1f3f3cf979f.png)

The image above could be represented by the following code:

```js
const popularMovies = [{
  title: 'Arrival',
  image: 'https://images-eu.ssl-images-amazon.com/images/I/51-e2UyOnxL._PJStripe-Prime-Only-500px,TopLeft,0,0._AC_UX500_SY400_.jpg',
  price: 3.49  
}, {
  title: 'Elf',
  image: 'https://images-eu.ssl-images-amazon.com/images/I/51loMeoCS4L._PJStripe-Prime-Only-500px,TopLeft,0,0._AC_UX500_SY400_.jpg',
  price: 3.49
}, {
  title: 'The Girl on the Train',
  image: 'https://images-eu.ssl-images-amazon.com/images/I/51fMiitYprL._PJStripe-Prime-Only-500px,TopLeft,0,0._AC_UX500_SY400_.jpg',
  price: 3.49
}]
```

## Further reading

* [JavaScript Arrays](https://www.javascript.com/learn/javascript/arrays)
* [Arrays - The Modern JavaScript Tutorial](https://javascript.info/array)
* [Array - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
