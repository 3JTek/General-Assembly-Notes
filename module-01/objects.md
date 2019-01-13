# Objects

Objects are a fundamental concept with any programming language. They are used to attempt to describe something in the real world in a programming language. Take a book for example. Each book on your book shelf has a certain set of properties: author, title, ISBN, number of pages, cover art, publishing house etc. If you were to express a book with JavaScript it might look like this:

```js
const title = 'A Wizard of Earthsea'
const author = 'Ursula K. LeGuin'
const isbn = '978-0141354910'
const pages = 304
const publisher = 'Puffin'
const cover = 'https://images-na.ssl-images-amazon.com/images/I/51iQfxB5rHL._SX323_BO1,204,203,200_.jpg'
```

Great, but what if you wanted to express another book, how would that look:

```js
const book1Title = 'A Wizard of Earthsea'
const book1author = 'Ursula K. LeGuin'
const book1Isbn = '978-0141354910'
const book1Pages = 304
const book1Publisher = 'Puffin'
const book1Cover = 'https://images-na.ssl-images-amazon.com/images/I/51iQfxB5rHL._SX323_BO1,204,203,200_.jpg'

const book2Title = 'The Tombs of Atuan'
const book2author = 'Ursula K. LeGuin'
const book2Isbn = '978-0140306323'
const book2Pages = 106
const book2Publisher = 'Puffin'
const book2Cover = 'https://images-na.ssl-images-amazon.com/images/I/41ENY8FS71L._SX281_BO1,204,203,200_.jpg'
```

And what if you wanted to express a whole library of books? Hopefully you can see that the code would quickly get very unmaintainable.

You can consider an object a wrapper around a collection of variables and and functions that are all related.

```js
const book1 = {
  title: 'A Wizard of Earthsea',
  author: 'Ursula K. LeGuin',
  isbn: '978-0141354910',
  pages: 304,
  publisher: 'Puffin',
  cover: 'https://images-na.ssl-images-amazon.com/images/I/51iQfxB5rHL._SX323_BO1,204,203,200_.jpg'
}

const book2 = {
  title: 'The Tombs of Atuan',
  author: 'Ursula K. LeGuin',
  isbn: '978-0140306323',
  pages: 106,
  publisher: 'Puffin',
  cover: 'https://images-na.ssl-images-amazon.com/images/I/41ENY8FS71L._SX281_BO1,204,203,200_.jpg'
}
```

Basically we are creating variables inside our object, only now we call them `properties`.

## Key / value pairs

When we make a variable, we have `name` and a `value`:

```js
const title = 'A Wizard of Earthsea'
```

When we assign a property to an object, we have a `key` and a `value`:

```js
const book1 = {
  title: 'A Wizard of Earthsea'
}
```

The syntax looks like this:

```js
{ key: value }
```

## Getters and setters

When we create an object we can also get and set the values of the keys using a `.`, or with square brackets:

```js
const book1 = {
  title: 'A Wizard of Earthsea',
  author: 'Ursula K. LeGuin',
  isbn: '978-0141354910',
  pages: 304,
  publisher: 'Puffin',
  cover: 'https://images-na.ssl-images-amazon.com/images/I/51iQfxB5rHL._SX323_BO1,204,203,200_.jpg'
}

book1.title // => "A Wizard of Earthsea"
book1['title']  // => "A Wizard of Earthsea"
```

Here we are saying "get me the `title` of the object called `book1`".

```js
book1.title = 'Around the World in 80 days'
book1['title'] = 'Around the World in 80 days'

book1 // => { title: "Around the World in 80 days", author: "Ursula K. LeGuin", isbn: "978-0141354910" ... }
```

Here we are saying "set the `title` of the object called `book1` to 'Around the World in 80 days'"

## Methods

We can also add functions to an object. When we do this we call the function a _method_. Let's take a different example, a bank account. The properties of the bank account will be account holder, account number, sort code and balance. We can also add methods for depositing and withdrawing funds:

```js
const account = {
  holder: 'Mr M Hayden',
  number: '0342516609',
  sortCode: '40-50-23',
  balance: 1280,
  deposit(amount) {
    this.balance += amount
  },
  withdraw(amount) {
    this.balance -= amount
  }
}

account.balance // => 1280
account.deposit(250)
account.withdraw(20)
account.balance // => 1510
```

### `this`

In JavaScript `this` refers to the object that the function is attached to _when it was called_. So when we made a deposit we called the `deposit` method from the `account` object, so inside the function `this` refers to the `account` object.

It is very useful when we have a method on an object which is designed to modify the properties of that object.

### Arrow functions

We can also use arrow functions to create methods on objects, however **`this` behaves differently with an arrow function**. For this reason, it is best to not use arrow functions for methods.


## Destructuring

Often when working with objects, we want to store a property into a variable of the same name. You might do that like so:

```js
const title = book.title
```

But you can also do it like this:

```js
const { title } = book
```

It's the exact same thing, but it's just a little bit quicker to write out. It's called _destructuring_.

You can also change the name of the variable when you destructure like so:

```js
const { title: bookTitle } = book // same as const bookTitle = book.title
bookTitle // => "A Wizard of Earthsea"
```

## Deleting properties

If you would like to remove a property from an object you can use the `delete` keyword:

```js
const book = {
  title: 'A Wizard of Earthsea',
  author: 'Ursula K. LeGuin',
  isbn: '978-0141354910'
}

delete book.isbn
book // => { title: "A Wizard of Earthsea", author: "Ursula K. LeGuin" }
```

## Comparing objects

When we compare primitive data types we can use the `===` sign:

```js
console.log(1 === '1') // => false
console.log('a' === 'a') // => false
```

However the same is not true for complex data types:

```js
const book1 = { title: 'A Wizard of Earthsea' }
const book2 = { title: 'A Wizard of Earthsea' }

console.log(book1 === book2) // => false
```

This is because when JavaScript compares objects it check whether they are the same object, and not whether they contain the same data. In the example above we created two objects, and as such each live in a different place in memory. When we compared them we asked JavaScript, "are they the same object?"

Imagine two identical cars, they are both black Honda Prius, same engine size, same number of doors, same amount of miles on the clock. Are they the same car? No. They have the same properties, but they are not the same car.

## Further reading

- [JavaScript Object Basics](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Basics)
- [Objects - The Modern JavaScript Tutorial](https://javascript.info/object)
- [Back To Basics: JavaScript Object Syntax](https://www.sitepoint.com/back-to-basics-javascript-object-syntax/)
