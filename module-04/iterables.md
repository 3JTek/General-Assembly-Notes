# Iterables

There are a number of data types in Python that are _iterable_, meaning they can be iterated over. The obvious ones are lists and tuples, but there are also many iterable objects in Python.

An iterable is any object that can return an _iterator_, and an iterator is a helper object that will return a value when you call `next` on it. How it does that is irrelevant, just that it _can_ handle `next` being called on it is enough.

Let's take a look at some common iterables in Python:

## `range`

The `range` method in Python creates a pre-populated `list` with numeric values:

```py
range(10, 20) # [10,11,12,13,14,15,16,17,18,19]
range(6) # [0,1,2,3,4,5]

range = range(10)
iterator = iter(range)

next(iterator) # 0
next(iterator) # 1
next(iterator) # 2 etc
```

## `enumerate`

The `enumerate` function takes an iterable and returns an object that contains a collection of the values of the iterable and indexes expressed as tuples

```py
names = ["Caroline", "Henry", "Priti", "Mohammed"]
enumerate_object = enumerate(names)
iterator = iter(enumerate_object)

next(iterator) # (0, 'Caroline')
next(iterator) # (1, 'Henry')
next(iterator) # (2, 'Priti')
next(iterator) # (3, 'Mohammed')
```

## `set`

A `set` in Python is the same as JavaScript: it is an immutable, unordered collection with no duplicated elements. It can be written like an array, but with curly brackets:

```py
names = { "James", "Amanda", "James", "Stavros" }
iterator = iter(names)

next(iterator) # 'James'
next(iterator) # 'Stavros'
next(iterator) # 'Amanda'
next(iterator) # Error: StopIteration
```

## `dict`

A `dict` is also iterable (as it is in JavaScript):

```py
student = { 'name': 'Selina', 'course': 'WDI', 'location': 'London' }
iterator = iter(student)

next(iterator) # 'course'
next(iterator) # 'location'
next(iterator) # 'name'
```

## `list`

As you would expect a `list` is iterable:

```py
names = ["Caroline", "Henry", "Priti", "Mohammed"]
iterator = iter(names)

next(iterator) # 'Caroline'
next(iterator) # 'Henry'
next(iterator) # 'Priti' etc
```

## `string`

Finally a string in Python is also iterable:

```py
str = "No way dude!"
iterator = iter(str)

next(iterator) # 'N'
next(iterator) # 'o'
next(iterator) # ' ' etc
```

## Iteration

This is not an exhaustive list of iterables, but it does outline some of the more common ones.

Any iterable can be iterated over using the `for in` loop:

```py
names = ["Caroline", "Henry", "Priti", "Mohammed"]
enumerate_object = enumerate(names)

for index, name in enumerate_object:
  print index, name

# 0 "Caroline"
# 1 "Henry"
# 2 "Priti"
# 3 "Mohammed"
```

## Comprehension

Iterables can be created from other iterables using _comprehension_. Here are a few examples:

```py
# list from range
[x*x for x in range(10)] # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
# set from list
{x*x for x in [10,20,30]} # { 400, 900, 100 }
# dict from tuple
{x: x*x for x in (1,2,3,4)} # { 1: 1, 2: 4, 3: 9, 4: 16 }
```

## Further reading

* [Iterables vs Iterators vs Generators - nvie](https://nvie.com/posts/iterators-vs-generators/)
* [Comprehensions - Python 3 Patterns, Recipes and Idioms](https://python-3-patterns-idioms-test.readthedocs.io/en/latest/Comprehensions.html) _(ignore the More Complex Example)_
