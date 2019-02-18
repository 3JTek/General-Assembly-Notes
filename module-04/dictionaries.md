# Dictionaries

Dictionaries in Python are similar to objects in JavaScript, except they are a little simpler. You cannot use the dot notation with dictionaries in Python, and if you use strings as keys they must be placed in quotes:

```py
car = { 'brand': 'Fiat', 'seats': 5, 'mph': 0 }

car['brand'] # Fiat
car['mph'] = 60
car['alloys'] = True

del car['seats'] # removes the 'seats' key/value pair

car # { 'brand': 'Fiat', 'mph': 60, 'alloys': True }
```

## `dict` methods

Python has a few dictionary methods which are similar to JavaScript:

| Method | Description | JS equivalent | Example |
|--------|-------------|---------------|---------|
| `keys()` | Returns a list of the dictionary's keys | `Object.keys()` | `dict.keys()` |
| `values()` | Returns the dictionary's values | `Object.values()` | `dict.values()` |
| `items()` | Returns tuples containing key/value pairs | `Object.items()` | `dict.items()` |
| <code>get(key,&nbsp;default)</code> | Returns the value of the key or a default value if none is found | <code>obj[key]&nbsp;&#124;&#124;&nbsp;default</code> | <code>car.get('mph',&nbsp;0)</code> |
| `pop(key)` | Removes the key specified and returns the value | N/A | `car.pop('mph')` |
| `clear()` | Empties the dictionary | N/A | `dict.clear()` |
| `update()` | Merges two dictionaries together | `Object.assign()` | `dict.update(car)` |

## Limitations

It is worth noting that Python dictionaries are not as versatile as JavaScript objects. While you can technically add functions to a dictionary in Python, they are not designed to hold complex logic in the same way.

You should think of a dictionary in Python as being far more similar to an array or list, except it can use strings as well as numbers for its keys.

If you need to create an object which has properties and methods in Python, you should create a `class`, similar to a JavaScript constructor function. Interestingly JavaScript's ES6 `class` syntax was heavily influenced by Python.

## Iteration

A dictionary is also iterable, just like a `set`, `tuple` or `list`. The simplest method is a `for in` loop, but other constructs are possible, including _composition_:

```py
for key in car:
  print key, car[key]

# 'brand' 'Fiat'
# 'mph' 60
# 'alloys' True

for key, value in car.items():
  print key, value

# 'brand' 'Fiat'
# 'mph' 60
# 'alloys' True

[car[key] for key in car] # ['Fiat', 60, True]
```

## Further reading

* [How to use dictionaries in Python - Python for Beginners](https://www.pythonforbeginners.com/dictionary/how-to-use-dictionaries-in-python)
* [Dictionaries in Python - Real Python](https://realpython.com/python-dicts/)
* [Python dictionary - Programiz](https://www.programiz.com/python-programming/dictionary)
