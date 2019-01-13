# Functions

Functions in python are similar to JavaScript, with a few bonus features.

We can define a function with the `def` keyword:

```py
def add(a, b):
  return a + b

add(10, 20)
```

If we call a function without passing the correct number of arguments, Python with throw an error:

```py
def add(a, b):
  return a + b

add(10) # TypeError: add() takes exactly 2 arguments (1 given)
```

## Docstring

In Python it is common to add a string as the first line of a function. This is called a docstring, which documents the function. The docstring can be accessed using the `__doc__` attribute of the function itself:

```py
def add(a, b):
  """ Adds two values together """
  return a + b

add.__doc__ # "Adds two values together"
```

In this way a developer can use the `__doc__` attribute to get some documentation from the function itself.

## Scope

Scope in Python works similarly to scope in JavaScript. Variables inside a function cannot be accessed outside the function, however a function is able to access variables in the parent scope:

```py
a = 10

def scope_test(b):
  return a + b

scope_test(20) # 30
```

By default a function cannot modify a variable outside of its scope:

```py
a = 10

def scope_test(b):
  a = 20 # a new variable has been created in the function's scope
  return a + b

scope_test(20) # 40
a # 10 - a in the global scope has not been changed
```

However this can be altered using the `global` keyword:

```py
a = 10

def scope_test(b):
  global a # a is now the global a!
  a = 20 # a new variable has been created in the function's scope
  return a + b

scope_test(20) # 40
a # 20 - a in the global scope has now changed
```

## Default arguments

As with ES6, we can create functions in Python with default arguments:

```py
def add(a=0,b=0):
  return a + b

add(10) # 10
add(10, 20) # 30
```

## Positional arguments (`args`) vs keyword arguments (`kwargs`)

We can pass arguments to a function in two ways, as positional arguments, or keyword arguments. Positional arguments require a knowledge of the correct _order_ in which to pass the arguments:

```py
def stones_to_kgs(st, lbs):
  return ((stone * 14) + lbs) * 0.453592

stones_to_kgs(10, 7) # The order of the arguments is important here
```

With keyword arguments we can pass the arguments in any order, but we require a knowledge of the correct _name_ of the arguments:

```py
def stones_to_kgs(st, lbs):
  return ((stone * 14) + lbs) * 0.453592

stones_to_kgs(lbs=7, st=10) # The name of the arguments is important here
```

We can also mix and match if we like:

```py
def stones_to_kgs(st, lbs):
  return ((stone * 14) + lbs) * 0.453592

stones_to_kgs(10, lbs=10) # The name of the arguments is important here
```

However, **the positional arguments must come first**

## Arbitrary arguments

With Python we can also create arbitrary arguments, where the function can handle as many arguments as we throw at it:

```py
def greet(*names):
  """ This function will loop through all names given and say hi to them """

  for name in names:
    print "Hi %s" % name

greet('Monica', 'Joey', 'Rachel', 'Chandler', 'Phoebe', 'Ross')
```

> **Note**: The arguments are stored as a `tuple`
> eg: ('Monica', 'Joey', 'Rachel', 'Chandler', 'Phoebe', 'Ross')

We can also pass other arguments either side of our arbitrary argument like so:

```py
def greet(salutation, *names):
  """ This function will loop through all names given and say hi to them with
  the given salutation """

  for name in names:
    print("%s %s" % (salutation, name))

greet('Well hey there', 'Monica', 'Joey', 'Rachel', 'Chandler', 'Phoebe', 'Ross')
```

If we want to handle an arbitrary number of keyword arguments we can do so like so:

```py
def greet(**data):
  """ This function will accept any kwargs assuming that the keyword is a name
  and the value is the salutation"""

  for name, saultation in data.items():
    print("%s %s" % (salutation, name))

greet(Monica="Hey", Joey="How you doin", Rachel="Hi", Chandler="Whats up", Phoebe="Yaaas", Ross="Yo")
```

> **Note:** The keyword arguments or _kwargs_ are stored as a `dict`
> eg: {'Monica': 'Hey', 'Rachel': 'Hi', 'Phoebe': 'Yaaas', 'Joey': 'How you doin', 'Chandler': 'Whats up', 'Ross': 'Yo'}

## Further reading

* [Python Functions - programiz](https://www.programiz.com/python-programming/function)
* [Python Function Arguments - programiz](https://www.programiz.com/python-programming/function-argument)
* [Python `*args` and `**kwargs` - programiz](https://www.programiz.com/python-programming/args-and-kwargs)
