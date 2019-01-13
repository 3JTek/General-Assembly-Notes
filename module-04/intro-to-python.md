# Intro to Python

Python was first released in 1991. It was created by a Dutch programmer called Guido van Rossum, and is named after the Britsh comedy group _Monty Python's Flying Circus_. Rossum was awarded the title of _Benevolent Dictator for Life_ by the Python community for his long standing work with Python. (He announced permanent vacation the position in July 2018)

The core philosophies of the language were summarised in [The Zen of Python](https://en.wikipedia.org/wiki/Zen_of_Python), which include:

* Beautiful is better than ugly
* Explicit is better than implicit
* Simple is better than complex
* Complex is better than complicated
* Readability counts

## History

The first release of Python was in 1991, v2.0 was released in 1994. Version 3.0 (aka Python 3000 or Py3K) was released in 2008.

Python 3 was designed to rectify some fundamental flaws in the the language which meant that it could not be backwards compatible. This meant that code written in Python 2 could not be easily ported to the new version of the language.

For this reason a lot of the major features of Python 3 were added to Python 2.6 and 2.7, which remains the final release of Python 2.

Python 2's end-of-life date was initially set to be 2015, but has since been postponed to 2020 due to the amount of Python 2 code still in production environments.

## Syntax and features

Python's syntax is based around indentation. Code block are delineated by a colon at the start and a carriage return at the end. Compared to JavaScript, parentheses are optional for conditionals, loops and other control flow constructs.

```py
a = 10

if a < 5:
  print('Too small')
elif a > 10:
  print('Too big')
else:
  print('Just right!')
```

Python is a _strongly typed_ language which means that it won't allow you to add a number to a string, for example. Because of this there is no need for a strict comparison operator `===` in Python.

Python uses the words `and`, `or` and `not`, rather than the symbols `&&`, `||` and `!` that are more common in C-based languages like JavaScript and PHP.

Python's ternary operator is also more wordy:

```py
a = 10
b = 20 if a < 10 else 40

b == 40 # True
```

Like most server-side language Python has a package manager for installing 3rd-party modules known as `pip`. Its syntax is similar to Node's `npm`.

## Popularity

Python's popularity has grown steadily over the last decade. According to [Stack Overflow](https://stackoverflow.blog/2017/09/06/incredible-growth-python/) it has the most question views within high-income nations in 2018, surpassing PHP, Java and JavaScript:

![growth_major_languages](https://zgab33vy595fw5zq-zippykid.netdna-ssl.com/wp-content/uploads/2017/09/growth_major_languages-1-1400x1200.png)

Part of the reason for this is that Google heavily adopted the language back in 2006. It is also used heavily by data scientists because it is able to handle large sets of data easily. This makes it a suitable language for machine learning and AI systems.

## Further reading

* [Python (programming language) - Wikipedia][1]
* [Official Python Website][2]
* [How is Python Different from Other Programming Languages - Hackernoon][3]

[1]: https://en.wikipedia.org/wiki/Python_(programming_language
[2]: https://www.python.org/
[3]: https://hackernoon.com/how-is-python-different-from-other-programming-languages-63311390f8dd
