# Advanced ES6

## Intro

React takes advantage of some new ES6 features that up until now we haven't really needed.

Before we dive into React, let's take a moment to familiarise ourselves with some of this new syntax.

## `class`

Most programming languages have a concept of a class. A class is a blueprint for an object. It aims to describe something in the real world.

In JavaScript we don't have a class, we have a _constructor_. A constructor is a function that we call with the `new` keyword.

With ES6 we can now create constructor functions using a `class` syntax which is much more similar to other languages like Python or Ruby.

#### `constructor` syntax

```js
function Monkey(name, species) {
  this.name = name
  this.species = species
  this.foodsEaten = []
}

Monkey.prototype.eatSomething = function(food) {
  return this.foodsEaten.push(food)
}

Monkey.prototype.introduce = function() {
  return `Hi my name is ${this.name}, I'm a ${this.species}`
}
```

#### `class` syntax

```js
class Monkey {
  constructor(name, species) {
    this.name = name
    this.species = species
    this.foodsEaten = []
  }

  eatSomething(food) {
    return this.foodsEaten.push(food)
  }

  introduce() {
    return `Hi my name is ${this.name}, I'm a ${this.species}`
  }
}
```

It's important to note that both methods are performing **exactly the same thing**. They are both creating constructor functions. Behind the scenes JavaScript is converting the `class` into a constructor for us.

Interestingly it will also add the methods we write onto the `prototype` for us.

## Destructuring

New in ES6 we can destructure objects. Although a weird word it's simply an elegant way of pulling properties from objects and storing them into variables.

Let's look at a simple example:

```js
const myMonkey = { name: 'Bubbles', species: 'Chimp' }
const { name, species } = myMonkey

console.log(name, species)
// => 'Bubbles' 'Chimp'
```

So here we have used our `Monkey` class to create a new monkey object, then we have stored the `name` and `species` properties from the object in to constants of the same name.

It's the equivalent of doing this:

```js
const myMonkey = { name: 'Bubbles', species: 'Chimp' }
const name = myMonkey.name
const species = myMonkey.species
```

With React we can take things one step further by destructring in the arguments of a function!

Here's an example:

```js
function logName({ name, species }) {
  return `The name is: ${name} and the species is: ${species}`
}

const myMonkey = { name: 'Bubbles', species: 'Chimp' }

logName(myMonkey)
// => "The name is: Bubbles and the species is: Chimp"
```

So we've pulled the name property off the object **as it is passed into the function!**

This is pretty cool, and we'll see it a lot in React.

## The `spread` operator

`spread` is a very apt name for this cool new feature of JavaScript. It is used to spread the properties of an object into another object. Let's look at a few examples.

#### With an array

Ok, let's say we want to add an element to the start and the end of an array **at the same time**.

With the spread operator, we can do it like this:

```js
const arr = ['monkey', 'house', 23]
const newArray = ['caterpillar', ...arr, '?']

console.log(newArray)
// => ["caterpillar", "monkey", "house", 23, "?"]
```

This creates a new array for us with the contents of the old array _spread_ into it at a point that we decide.

Let's look at how we would have had to do this without `spread`:

```js
const arr = ['monkey', 'house', 23]
const newArray = arr.slice(0) // copies the array
newArray.unshift('caterpillar')
newArray.push('?')

console.log(newArray)
// => ["caterpillar", "monkey", "house", 23, "?"]
```

#### With an object

Similarly we can use `spread` with an object:

```js
const me = { name: 'Mike', age: 36 }
const workMe = { ...me, role: 'Instructor', employer: 'General Assembly' }

console.log(workMe)
// => { name: 'Mike', age: 36, role: 'Instructor', employer: 'General Assembly' }
```

This could also have been done with `Object.assign` like so:

```js
const me = { name: 'Mike', age: 36 }
const workMe = Object.assign({}, me, { role: 'Instructor', employer: 'General Assembly' })

console.log(workMe)
// => { name: 'Mike', age: 36, role: 'Instructor', employer: 'General Assembly' }
```

We'll be using `spread` **a lot** with React, so it's really important that we are comfortable with the syntax, so we can focus on React and not ES6.

## Computed property keys

With ES5 we can use what we call a `computed` property. This is where we can use the contents of a variable as the key of an object. Let's take a look:

```js
const key = 'species'
const monkey = { name: 'Bubbles', species: 'Chimp' }

console.log(monkey[key])
// => 'Chimp'
```

We can also set properties in the same way:

```js
const key = 'species'
const monkey = { name: 'Bubbles', species: 'Chimp' }

monkey[key] = 'Orangutan'
console.log(monkey)
// => { name: 'Bubbles', species: 'Orangutan' }
```

However, with ES6 we can now set properties like this with object literals, like so:

```js
const key = 'species'
const monkey = { name: 'Bubbles', [key]: 'Chimp' }

console.log(monkey)
// => { name: 'Bubbles', species: 'Chimp' }
```

This is actually really cool. Let's take a look at a slightly more real-world example. Let's say we have a form and we want to map the contents of each field to an object in JavaScript on the change event.

```html
<form>
  <input type="text" name="name" placeholder="Name" onchange="mapToObject()" />
  <input type="text" name="species" placeholder="species" onchange="mapToObject()" />
  <button>Submit</button>
</form>
```

```js
function mapToObject(e) {
  const obj = { [e.target.name]: e.target.value }
  console.log(obj)
  return obj
}
```

When the `onchange` event is triggered the value of the input is set to the name of the input in the object. If we added the word "Bubbles" in the name field in the example above, we would get the following object back from the `mapToObject` function:

```js
{
  name: 'Bubbles'
}
```

## Conclusion

As you can see it's now quite trivial to perform some quite complex functionality with simple ES6 methods. These are going to become much more familiar we work through this React module.

## Further reading

* [ECMAScript 6 Features](http://es6-features.org/)
* [Top 10 ES6 Features Every Busy JavaScript Developer Must Know](https://webapplog.com/es6/)
