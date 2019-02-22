# Modules & Packages

As we build more complex applications with Python it is important that we spilt our code out into more files across multiple folders.

In order to share functionality between files and folders we need to become familiar with Python's `import` syntax and how we can structure our projects.

The `import` syntax is similar to ES6, with a few important caveats:

1. There is no need to explicitly export data from a file to be used in another file
1. The syntax is the reverse of ES6: `from <module> import <property>`

## Modules

When structuring our code, each new file becomes a _module_ which allows us to oragnise our code more effectively.

Let's have a look at an example. Let's say we want to create a converter module, which converts imperial to metric. We create a `converter.py` where our converter logic will live, and a `main.py` file where we will import our converter to be used in the main business logic:

```py
# converter.py
ratios = {
  'lbs': 0.453592,
  'stone': 6.35029,
  'fl oz': 28.4131,
  'pint': 568.261
}

def convert(amount, unit):
  return amount * ratios.get(unit, 1)
```

```py
# main.py
import converter # converter module loaded into memory

unit = input('What do you want to conver? lbs / stone / fl oz / pint ')
amount = input('How many %ss to convert? ' % unit)
result = converter.convert(amount, unit)
metric = 'kg' if unit in ['lbs', 'stone'] else 'ml'

print('%s %s is %s %s' % (amount, unit, result, metric))
```

Here we have imported the whole `converter` module, including the `ratios` dictionary. This is actually no necessary since we do not use `ratios` in the `main.py` file.

Instead we can just import the `convert` method like so:

```py
# main.py
from converter import convert

unit = raw_input('What do you want to conver? lbs / stone / fl oz / pint ')
amount = input('How many %ss to convert? ' % unit)
result = convert(amount, unit)
```

## Packages

We can take this idea further with _packages_. A collection is a collection of modules in a dedicated folder. Dot notation can be used to target a specific module in the package.

However, in order for this to actually work we need to add an empty `__init__.py` file in the folder. Consider the following folder structure:

```
.
├── config
│   ├── __init__.py
│   ├── environment.py
│   └── router.py
└── main.py
```

We can now import `environment.py` and `router.py` in `main.py` like so:

```py
import config.environment
import config.router
```

Alternatively:

```py
from config import environment, router
```

If we want to add any initialisation logic for the module we can add it to the `__init__.py` file, but otherwise it can be left blank. You can think of it a bit like the `__init__()` method of a class.

## Further reading

* [Python Modules and Packages – An Introduction - Real Python](https://realpython.com/python-modules-packages/)
* [How to Write Modules in Python 3 - DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-write-modules-in-python-3)
