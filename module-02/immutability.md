# Immutability

One of the things that makes React very quick is that it adheres to the concept of immutable state. But what does immutable mean?

Well at its root is the word _mutate_. When we mutate something we change it. We modify it in place. When Bruce Banner turns into the Incredible Hulk, he mutates. He changes his body.

## Mutating data

In JavaScript we often _mutate_ data. Let's take a look at a couple of examples:

```js
const arr = ['Charles Xavier', 'James Logan', 'Jean Grey']
arr.push('Scott Summers')

const mutant = { name: 'Rogue', alterEgo: 'Jean Grey' }
mutant.alterEgo = 'Anne Marie'
```

In both instances above we have mutated an object.

In the first case we extended an array with an extra element. The array has been modified **in place**. The original array has changed.

In the second case we changed the values of the keys of an object. The object itself is the same, we **mutated** it by changing its contents.

## React and mutation

React doesn't generally like it when we mutate data. There is a good reason for this. When we mutate an object we don't get a new object, so it's hard for React to know what has changed or even whether anything has changed at all.

When the underlying data of a React app changes it normally means that React has to get to work to update the UI. The way that React knows whether do to this is by comparing the old data with the new data. If we mutate the old data, then React does not know what has changed.

Let's take a look:

```js
const beforeMutation = { name: 'Rogue', alterEgo: 'Jean Grey' }
const afterMutation = beforeMutation

afterMutation.alterEgo = 'Anne Marie'

console.log(beforeMutation === afterMutation) // true
console.log(beforeMutation.alterEgo === afterMutation.alterEgo) // true

console.log(beforeMutation, afterMutation)
// => { name: 'Rogue', alterEgo: 'Anne Marie' } { name: 'Rogue', alterEgo: 'Anne Marie' }
```

Because the object has been mutated its data has changed **in place**. React has no way of seeing the way the object looked before, so has no way of knowing which part of the object has been changed, and in what way.

## Working immutably

So in order for React to know if the underlying data of the app has changed and in what way, we need to make sure we change data in an immutable way. In order to do that we need to use methods and patterns that return **new objects**, rather than mutating objects in place.

Let's look at some examples:

```js
const arr = ['Charles Xavier', 'James Logan', 'Jean Grey']
const newArr = arr.concat('Scott Summers')

// could also use the spread operator
// const newArr = [...arr, 'Scott Summers']

console.log(arr === newArr) // false
console.log(arr.length === newArr.length) // false
```

The immutable version of `push` is `concat`. `concat` returns a new array, it takes the existing array, plus the new element and returns a brand new array, leaving the existing one in tact. This makes it very easy to see what has changed, and what part of the app needs to be updated.

```js
const beforeMutation = { name: 'Rogue', alterEgo: 'Jean Grey' }
const afterMutation = { ...beforeMutation, alterEgo: 'Anne Marie' }

console.log(beforeMutation === afterMutation) // false
console.log(beforeMutation.alterEgo === afterMutation.alterEgo) // false
```

`...` is called a _spread operator_, it's used to copy properties from one object to another. In the example above we have created a new object, and added the properties from the `beforeMutation` object to it with the _spread operator_. We then also overwrite the `alterEgo` property. The result is a new object with all the data that we want. The original object has not been modified, so we can easily compare the two.

Let's have a look at some array methods that are mutable and immutable:

| Mutable | Immutable |
|---------|-----------|
| `splice()` | `slice()` |
| `push()` | `concat()` |
| `pop()` | `slice(-1)` |
| `shift()` | `slice(1)` |
| `unshift()` | `[element, ...oldArray]` |
| `sort()` | `slice(0).sort()` |
| `reverse()` | `slice(0).reverse()` |
| N/A | `filter()` |
| N/A | `map()` |
| N/A | `reduce()` |

## Further reading

* [Immutability in JavaScript](https://www.sitepoint.com/immutability-javascript/)
* [Pure javascript immutable arrays](https://vincent.billey.me/pure-javascript-immutable-array/)
