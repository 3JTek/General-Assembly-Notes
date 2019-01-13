# npm

npm is the package manager for Node.js. It allows us to install 3rd-party packages (or _modules_) into our Node.js projects. npm is automatically installed with Node.js.

## Setup

Before using npm, we need to create a `package.json` file in our project. The `package.json` will keep a track of all the packages that we have installed in this project.

Rather than creating the `package.json` file manually, we can use the following npm command:

```sh
npm init
```

npm will then ask a series of questions. If we hit enter npm will use the default, the value in parenthesis.

```
package name: (intro-to-webpack)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
license: (ISC)
```

This would then generate the following `package.json` file:

```json
{
  "name": "intro-to-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Mike Hayden <mickyginger@gmail.com> (https://github.com/mickyginger)",
  "license": "ISC"
}
```

## Usage

To install a package we can use the following command:

```sh
npm install <package-name>
```

So for example if we wanted to install jQuery, we would write:

```sh
npm install jquery
```

This will add the jQuery library to our project. npm will store the module in a `node_modules` folder in the project, and also update the `package.json` file to include the new package:

```json
{
  "name": "intro-to-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Mike Hayden <mickyginger@gmail.com> (https://github.com/mickyginger)",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.3.1"
  }
}
```

If we want to install a development package, something that we would **only** use in development, for example, testing packages like Mocha and Chai, or tooling packages like Webpack, we should pass the `--save-dev` flag:

```sh
npm install webpack --save-dev
```

This will add the package into the `devDependencies` part of the `package.json`:

```json
{
  "name": "intro-to-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Mike Hayden <mickyginger@gmail.com> (https://github.com/mickyginger)",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.3.1"
  },
  "devDependencies": {
    "webpack": "^4.28.3"
  }
}
```

To uninstall packages, we use the following command:

```sh
npm uninstall <package-name>
```

## Common commands

| Command | Description | Example |
|:--------|:------------|:--------|
| `init` | Creates a `package.json` file so that you can start using npm | `npm init` |
| `install` | Installs a package into your project | `npm install jquery` |
| `i` | Shorthand for `install` | `npm i jquery` |
| `uninstall` | Removes a package from your project | `npm uninstall jquery` |
| `un` | Shorthand for `uninstall` | `npm un jquery` |
| `--save-dev` | Indicate package is for development only | `npm install webpack --save-dev` |
| `-D` | Shorthand for `--save-dev` | `npm i webpack -D` |

## Further reading

* [An Absolute Beginners Guide to Using npm - NodeSource](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm/)
* [npm Cheatsheet](https://devhints.io/npm)
* [NPMmmm #1: Dev Dependencies, Dependencies - Medium](https://medium.com/@dylanavery720/npmmmm-1-dev-dependencies-dependencies-8931c2583b0c)
