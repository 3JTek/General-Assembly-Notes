# Control flow

Control flow in Python is similar to JavaScript, but does differ slightly in syntax. Here's a run through some common control flow patterns in Python:

## `if elif else`

```py
a = 5

if a < 5:
  print('Too small')
elif a > 5:
  print('Too big')
else:
  print('Just right!')
```

Similar to JavaScript, but note the lack of parentheses, curly-braces and shortening of `else if`.

Interestingly **there is no switch statement** in Python. However, there are some [clever workarounds](https://stackoverflow.com/questions/60208/replacements-for-switch-statement-in-python)

## `while`

The while loop is almost identical to its JavaScript counterpart:

```py
a = 10
while a > 0:
  print("a is now %s" % a)
  a-=1
```

## `for in`

The `for in` loop is probably the most common way to loop and iterate in Python. It can be used with lists, dictionaries, sets, tuples and ranges:

```py
# iterating over an array
names = ['Agnetha', 'Benny', 'Björn', 'Anni-Frid']
for name in names:
  print(name)

# iterating over a dictionary
car = { 'make': 'Fiat', 'model': 'Uno', 'color': 'red' }
for attribute in car:
  print(car[attribute])

# looping a set number of times (will print 0 - 9)
for i in range(10):
  print(i)
```

### `break` and `continue`

We can use the `break` statement to stop (or break out of) a loop, and the `continue` to skip to the next iteration:

```py
for i in range(10):
  if i == 2:
    continue
  elif i == 5:
    break

  print("i is %s" % i)

# output:
# 0
# 1
# 3
# 4
```

## Boolean logic

Boolean logic is the same in all languages, but with Python the symbols `&&`, `||` and `!` are replaced with `and`, `or` and `not`:

```py
True or False # True
False or True # True
False or False # False

0 or "Steve Jobs" # Steve Jobs
100 or "Bill Gates" # 100
'' or None # ""

True and False # False
False and True # False
True and True # True

0 and "Steve Jobs" # 0
100 and "Bill Gates" # "Bill Gates"
'' and None # ""
```

The only time you will see a `!` is when checking for inequality:

```py
a = 10
b = 20

a != b # True
```

## Ternary operator

Python also has a ternary operator, much like JavaScript, except it is far more Python-y:

```py
a = 10

b = 15 if a > 10 else 20

b == 20 # True
```

## Further reading

* [Control Flow - A Byte of Python](https://python.swaroopch.com/control_flow.html)
* [Control Flow - A Whirlwind Tour of Python](https://jakevdp.github.io/WhirlwindTourOfPython/07-control-flow-statements.html)
* [Understanding Boolean Logic in Python 3 - DigitalOcean](https://www.digitalocean.com/community/tutorials/understanding-boolean-logic-in-python-3)
