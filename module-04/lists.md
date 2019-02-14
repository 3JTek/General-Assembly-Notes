# Lists

While lists are identical to arrays in JavaScript, there are some interesting differences in the syntax we can use with lists in Python.

## Iteration

There are several ways to iterate over an array in JavaScript:

```js
const nums = [10, 20, 30]

// for loop
for(let i=0;i<nums.length;i++) {
  console.log(nums[i])
}

// for in loop
for(const i in nums) {
  console.log(nums[i])
}

// while loop
let i=0
while(i<nums.length) {
  console.log(nums[i])
  i++
}

// forEach method
nums.forEach(num => console.log(num))
```

Python is no different:

```py
nums = [10, 20, 30]

# for in
for num in nums:
  print(num)

# while
i = 0
while i < len(nums):
  print(nums[i])

# for in (with index)
for i in range(len(nums)):
  print(nums[i])
```

We can also get the index if we create an enumerator object with our list first. The `enumerate` returns an iterable of tuples containing `index` and `element`.

```py
nums = [10, 20, 30]
for index, num in enumerate(nums):
  print(index, num, num[index])
```

## `list` methods


| Method | Description | JS equivalent | Example |
|:-------|:------------|:--------------|:--------|
| `append(elem)` | Add an element on the end of a list | `push(elem)` | `list.append(10)` |
| `extend(list)` | Add the contents of one list to the other | `concat(array)` | `list.concat(["hamster", "marmalade"])` |
| `insert(index, elem)` | Add an element at the given index | `splice()` | `list.insert(2, 'avocado')` |
| `remove(elem)` | Removes the given element from the list | N/A | `list.remove('hamster')` |
| `pop(index)` | Removes an element at the given index, or the end if no index is given | `pop()` | `list.pop()` |
| `clear()` | Removes all elements from the list | N/A | `list.clear()` |
| `count()` | Return the number of elements in the array | `length` | `list.count()` |
| `sort()` | Sorts an array in ascending order (numerically or alphabetically) | `sort()` | `list.sort()` |
| `reverse()` | Reverses the order of the elements in an array | `reverse()` | `list.reverse()` |
| `copy()` | Returns a copy (clone) of the array | `slice()` | `list.copy()` |

## Built in functions

There are some very useful array methods in JavaScript like `forEach`, `map`, `reduce`, `filter` etc., which are not list methods in Python. Instead the methods are global functions. Python does this because these functions can be used with data types other than `lists`.

These methods take functions as arguments. In JavaScript we can use either named functions or arrow functions, and Python is no different, except Python's equivalent to an arrow function is a _lambda_.

Let's have a look at this in action:

### `map`

```py
# Double the numbers in the list
nums = [10, 20, 30]
map(lambda num: num*2, items) # [20, 40, 60]
```

The equivalent in JavaScript would be:

```js
const nums = [10, 20, 30]
nums.map(num => num*2) // [20, 40, 60]
```

The _lambda_ is very similar to an arrow function in JavaScript, and can be stored in a variable in the same way:

```py
doubler = lambda x: x*2
nums = [10, 20, 30]

map(doubler, nums) # [20, 40, 60]
```

We can also create a named function to do the same thing:

```py
def doubler(x):
  return x * 2

nums = [10, 20, 30]

map(doubler, nums) # [20, 40, 60]
```

### Example built in functions

| Function | Description | JS equivalent | Example |
|----------|-------------|---------------|---------|
| `map(func, list)` | Applies a function to all elements in a list | `map(func)` | `map(lambda x: x*2, list)` |
| `filter(func, list)` | Returns a list with the value that passed the test in the provided function | `filter(func)` | `map(lambda x: x%2 == 0)` |
| `reduce(func, list)` | Creates an accumulated value over function calls applied to a list | `reduce(func)` | `reduce(lambda sum,num: sum + num, list)` |
| `sum(list)` | Sum the elements of the list | N/A | `sum(list)` |
| `all(list)` | Returns true if ALL values are _truthy_ | `every(func)` | `all(list)` |
| `any(list)` | Returns true if ANY values are _truthy_ | `some(func)` | `any(list)` |

## List comprehension

Python as a very elegant syntax for creating new lists from existing lists, whilst modifying their contents. This syntax can be used to perform a number of different list methods. The syntax can be a little tricky to grasp at first, but it can lead to some very concise code.

The syntax looks like this:

```
[value for value in iterable if condition]
```

Here are some examples for existing list methods:


### `filter`

We can create a new list with a sub-set of another list like so:

```py
nums = [11, 200, 7, 44]

# with filter
filter(lambda x: x%2==0, nums)

# with list comprehension
[x for x in nums if x%2==0]
```

### `map`

```py
nums = [11, 200, 7, 44]

# with map
map(lambda x: x*2, nums)

# with list comprehension
[x*2 for x in nums]
```

This becomes even more elegant when we use other iterable data types like `range`, `set`, `dict` etc:

```py
[x*2 for x in range(4)] # [0,2,4,6]
```


## Further reading

* [Map, Filter and Reduce - Python Tips](http://book.pythontips.com/en/latest/map_filter.html)
* [Python List - Programiz](https://www.programiz.com/python-programming/list)
* [A Cheap Introduction to Comprehension in Python - Medium](https://medium.com/the-andela-way/a-cheap-introduction-to-comprehension-in-python-2269895f996f)
