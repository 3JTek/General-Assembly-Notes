# Yarn

Yarn is a package manager for Node.js projects. While npm is the _official_ package manager, Yarn is a 3rd-party package manager which has a few benefits over npm:

* It is quicker
* It caches previously install packages

While npm is automatically installed when we install Node.js, Yarn can be installed with Homebrew:

```sh
brew install yarn
```

## Usage

Yarn's syntax is almost identical to npm's:

| Command | Description | Example |
|:--------|:------------|:--------|
| `init` | Creates a `package.json` file so that you can start using npm | `yarn init` |
| `add` | Installs a package into your project | `yarn add jquery` |
| `remove` | Removes a package from your project | `yarn remove jquery` |
| `--dev` | Indicate package is for development only | `yarn add webpack --dev` |

## Further reading

* [Yarn Cheatsheet - Devhints.io](https://devhints.io/yarn)
* [Migrating from npm - Yarn](https://yarnpkg.com/lang/en/docs/migrating-from-npm/)
* [Yarn vs npm - which Node package manager to use in 2018?](https://blog.risingstack.com/yarn-vs-npm-node-js-package-managers/)
