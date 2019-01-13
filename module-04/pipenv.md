# `pipenv`

Python comes with its own package manager called `pip`. While `pip` is a great tool, it does have one major flaw. It will install packages globally, meaning that any Python project you start will have access to all the packages you have previously installed.

This becomes a problem when you want to upgrade a package. Perhaps another project on your computer is using the older version. When you upgrade the package, your old project will break. As developers it is normally preferable to keep a manifest file for each project, and ensure that only specific modules are made available for specific projects.

Enter `pipenv`. It is a tool which adds that functionality to your workflow, so that you can manage your Python projects in a similar way to your Node projects.

## Installation

You can install `pipenv` with Homebrew:

```sh
brew install pipenv
```

This will install the `pipenv` module globally, which is exactly what we want.

## Usage

To use `pipenv`, create a new python project folder and navigate to it.

Now rather than using `pip` to install dependencies, you can use `pipenv`:

```sh
pipenv install flask
```

This will automatically create a Pipfile, which is similar to Node's `package.json` and a `Pipfile.lock` which is similar to npm's `package-lock.json` or Yarn's `yarn.lock` file.

Using `pipenv` to install your packages means that you can collaborate on projects in the same way as with Node, simply clone or download the repo and type `pipenv install` to install all the dependencies.

## How it works

When you install a package with `pipenv` it creates a _virtual environment_ for that project. This mimics a development environment where only the packages you have install for that project exists. It does this by installing all the packages to a uniquely named folder somewhere in the file system of your laptop.

In order to run a project managed by `pipenv` you need to prefix any command with `pipenv run`. So for example:

```sh
# Have Python execute the main.py file in the virtual enviromnebt
pipenv run python main.py

# Launch a flask server in the virtual enviromnent
pipenv run flask run
```

## Further reading

* [Pipenv - Official Documentation](https://pipenv.readthedocs.io/en/latest/)
* [Pipenv: A Guide to the New Python Packaging Tool - Real Python](https://realpython.com/pipenv-guide/)
* [How to manage your Python projects with Pipenv - Thoughtbot](https://robots.thoughtbot.com/how-to-manage-your-python-projects-with-pipenv)
