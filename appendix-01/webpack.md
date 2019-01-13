# Webpack

Webpack is an open-source JavaScript module bundler. What that means it that it allows us to import code from files and folders into other files and folders.

Not only that it can also transpile ES6 to ES5, convert SASS to CSS and copy static files from one folder to another. We can even have Webpack refresh the browser window when we save our files, so we don't have keep refreshing manually!

All of these things are required when developing complex JavaScript applications. Having a tool automate these tasks is very helpful.

> **Note:** Webpack and all its dependencies can be installed with `npm` or `yarn`

### Install Webpack

There are a number of packages we are going to need for webpack:

* `webpack`: the core library
* `webpack-cli`: the terminal commands needed to execute webpack
* `webpack-dev-server`: needed to watch for file changes and refresh the browser
* `html-webpack-plugin`: allows Webpack to add script tags to html files

### Install loaders

Webpack can be configured with _loaders_ which add support for various types of files:

* `babel-loader`: adds support for ES6 files
* `style-loader`: adds CSS to the `<head>` of the `index.html`
* `css-loader`: adds support for CSS files
* `sass-loader`: adds support for SASS/SCSS files

### Install babel

In order to transpile ES6 to ES5 we need to install Babel. Babel, named after the [Babel Fish from _The Hitchhiker's Guide to the Galaxy_](https://www.youtube.com/watch?v=YWqHkYtREAE), is a package that does just that.

In order to use it we need to install two packages:

* `@babel/core`: the core library
* `@babel/preset-env`: adds support for the latest version of JavaScript (ES6, ES7, ES8 etc)

We also need to tell babel to use the preset. To do that we need to create a `.babelrc` file, and add the following configuration:

```json
{
  "presets": ["@babel/preset-env"]
}
```
> **Note**: when using React, we also need to install `@babel/preset-react` and add it to the `.babelrc` file in the same way:
> ```json
> {
>   "presets": [
>     "@babel/preset-env",
>     "@babel/preset-react"
>   ]
> }
> ```

### Install `node-sass`

Finally we need a Node.js SASS compiler to compile SCSS to SASS

### Create an `src` folder

All the code that we write in a Webpack project will be transpiled or compiled at some point. We call the original pre-compiled code, the code that we write, the _source_ code. Once compilation is complete we need to store the compiled code somewhere. The compiled code will ultimately get sent to the browser, the source code will not.

For this reason we need to keep our source code and compiled code separate so we often use an `src` folder to hold our source code and a `dist` or `dest` to hold our compiled code.

## Configure Webpack

With that in place we can now configure webpack. To do that we need to create a file called `webpack.config.js`. Here is a basic configuration file:

```js
const path = require('path')
const webpack = require('webpack')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: './src/app.js',
  output: {
    path: path.resolve('dist'),
    filename: 'bundle.js'
  },
  module: {
    rules: [
      { test: /\.jsx?$/, loader: 'babel-loader', exclude: /node_modules/ },
      { test: /\.css$/, loader: ['style-loader', 'css-loader'] },
      { test: /\.s(a|c)ss$/, loader: ['style-loader', 'css-loader', 'sass-loader'] }
    ]
  },
  devServer: {
    contentBase: path.resolve('src'),
    hot: true,
    open: true,
    port: 8000,
    watchContentBase: true
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({
      template: 'src/index.html',
      filename: 'index.html',
      inject: 'body'
    })
  ]
}
```

| Setting | Meaning |
|:---------|:---------|
| `entry` | Location of the main JavaScript file for the project |
| `output` | Location and filename used to store the compiled code |
| `module.rules` | The loaders that we want Webpack to use |
| `devServer` | Configuration for `webpack-dev-server` |
| `devServer.contentBase` | Where the source code files are stored |
| `devServer.port` | The port number `webpack-dev-server` will use |
| `devServer.hot` | Whether the hot module replacement plugin will be used, which will refresh the browser when changes are detected |
| `devServer.open` | Whether `webpack-dev-server` should open the browser for you when it starts |
| `devServer.watchContentBase` | Whether `webpack-dev-server` should what for changes in static files like `index.html` |
| `plugins` | Which plugins should be used |

> **Note**: In the example above the `HotModuleReplacementPlugin` will re-compile the code when changes are made, then refresh the browser. The `HtmlWebpackPlugin` allows Webpack to add script tags and style tags to the `index.html` file.

## Usage

With Webpack configured we can now use it. To do so we need to add some scripts to our `package.json`:

```json
{
  "name": "webpack-config",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "serve": "webpack-dev-server --mode=development",
    "build": "webpack -p"
  },
  "author": "Mike Hayden <mickyginger@gmail.com> (https://github.com/mickyginger)",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.2.2",
    "@babel/preset-env": "^7.2.3",
    "babel-loader": "^8.0.5",
    "css-loader": "^2.1.0",
    "html-webpack-plugin": "^3.2.0",
    "node-sass": "^4.11.0",
    "sass-loader": "^7.1.0",
    "style-loader": "^0.23.1",
    "webpack": "^4.28.3",
    "webpack-cli": "^3.2.1",
    "webpack-dev-server": "^3.1.14"
  }
}
```

By adding commands in the `scripts` section of the `package.json` we can use them in the terminal:

```sh
npm serve
```

Will start `webpack-dev-server`, which will open the browser and watch for file changes. It will not compile any code, but instead stores it all in memory.

```sh
npm build
```

Will actually compile our source code. It will create the `dist` folder and add the `bundle.js` file, `index.html` and any CSS files that may have been created. It will not open the browser or watch for file changes.

## Further reading

* [A Beginner’s Guide to Webpack 4 and Module Bundling](https://www.sitepoint.com/beginners-guide-webpack-module-bundling/)
* [Learn Webpack in 15 minutes](https://tutorialzine.com/2017/04/learn-webpack-in-15-minutes)
