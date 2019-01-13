# Python Data Types

What JavaScript developers would called _primitives_, basic data types, as their name implies are the most basic data types that Python offers.

## Strings

Strings can be defined with either single quotes or double quotes

```py
"This is a string"

'This is also a string'
```

As with JavaScript you can escape characters with a backslash:

```py
"\"Hello there\", he said"

'I\'m not sure I heard you correctly old bean!'
```

In python we can also use three quote marks around a string, which means that we can use carriage returns, tabs and quotes within the string. This is very similar to ES6 backticks:

```py
"""this
is a
multiline string.

"Why how fantastic", she declared."""
```

We can also create a _raw string_ in python by prefixing the string with a lower or uppercase _r_. Escape characters are now considered part of the string:

```py
r'This\'ll not come out quite as expected Holmes'
```

## Numbers

Like most programming languages, numbers can either be integers (whole numbers) or floats (fractions):

```py
type(15) # <type 'int'>
type(1.5) # <type 'float'>
```

Python has a neat way of handling  binary (base 2), octal (base 8) and hexadecimal (base 16) numbers, since these are the number bases we use most frequently when programming:

```py
0b1010 # 10 - 1010 in binary is 10
0o20 # 16 - 20 in octal is 16
0xf0 # 240 - f0 in hexadecimal is 240
```

## Booleans

Python has the same concept of booleans as any other programming language. Unlike JavaScript, Python spells its boolean values in title case:

```py
type(True) # <type 'bool'>
type(False) # <type 'bool'>
```

Just like JavaScript, any value can be considered _truthy_ or _falsey_, depending on whether that value would become True or False when converted to a boolean. Unlike JavaScript however the list of _falsey_ values is a little longer.

All values can be considered _truthy_, except the following _falsey_ values:

```py
None # same as null
False
0
0.0
''
[] # an empty list (equivalent to an empty array in JS)
{} # an empty dict (equivalent to an empty object in JS)
() # an empty tuple (an immutable array)
set() # an empty set
range(0) # am empty range
```

## Lists

A list is what a JavaScript developer would call an array. In Python the syntax is almost identical:

```py
list = [10, 'Python', True, 'hamster']

list[1] # "Python"
type(list) # <type 'list'>

list[1:3] # ['Python', True] - like .slice() in JS
list + ['OMG', 'amazing'] # [10, 'Python', True, 'hamster', 'OMG', 'amazing'] - like .concat() in JS

list.append('Crazy yo!') # like .push() in JS
list.remove(True) # removes the value from the list
list.pop() # removes the last value from the list
list.pop(0) # remove the value at the given index
```

## Tuple

A tuple is an immutable list. If something is immutable, it cannot change. Unlike JavaScript, if you store an array in a `const`, the array is constant, but its contents _can_ change. A tuple's contents _cannot_ change. Otherwise, they are almost identical:

```py
tuple = (10, 'Python', True, 'hamster')

tuple[1] # Python
type(tuple) # <type 'tuple'>

tuple[1:3] # ('Python', True)
tuple + ('OMG', 'amazing') # (10, 'Python', True, 'hamster', 'OMG', 'amazing')
```

## Dictionary

A dictionary is similar to an object in JavaScript, albeit far simpler:

```py
dict = { 'name': 'Julien', 'role': 'Regional director' }

dict['name'] # 'Julien'
type(dict) # <type 'dict'>

dict['age'] = 32
dict.keys() # ['name', 'role', 'age']
dict.values() # ['Julien', 'Regional director', 32]
```

## Type coercion

In JavaScript if you add a number to a string, or an array to a number, one of the values will be coerced (or automatically converted) to the same data type as the other, often leading to unexpected results.

Python, like most other programming languages does not do this. Instead it will throw an error:

```
TypeError: cannot concatenate 'str' and 'int' objects
```

We can however attempt to convert data types ourselves:

```py
int('10') # 10
str(10) # "10"
tuple(['a', 10, True]) # ["a", 10, True]
dict(('name', 'Mike'), ('role', 'Instructor')) # { 'name': 'Mike', 'role', 'Instructor' }
```

## Variables

In JavaScript there are three ways of declaring a variable: `var`, `let` and `const`. With Python we simply assign values to a variable name, without the need for a keyword:

```py
name = 'Colin'
things_in_my_fridge = ["Yoghurt", "Leftovers", "Salad", "Mayonnaise"]
```

Variables cannot be constant, and can use upper or lowercase letters and underscores. The general preference is for _snake_case_, with the exception of _PascalCase_ for classes.

## Further reading

* [Basic Data Types in Python - Real Python](https://realpython.com/python-data-types)
* [Python Data Types - Rhino Docs](https://developer.rhino3d.com/guides/rhinopython/python-datatypes/)
* [Python - Variable Types - Tutorials Point](https://www.tutorialspoint.com/python/python_variable_types.htm)
