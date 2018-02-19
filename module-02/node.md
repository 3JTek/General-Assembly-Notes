# NodeJS

JavaScript is not a great language, as far as programming languages go. It is poorly designed and has a lot of nuances that can make it unpredictable and difficult to work with. However there is something very unique and useful about JavaScript: it's fast!

JavaScript requires a host environment to run. Most commonly that is the browser, but these days it's found its way onto the server and into databases. The reason JavaScript is considered fast is because anything that takes time (like a network request, or a `setTimeout`) is passed off to the host environment (the browser for example) to take care of. When the job is done, the host environment hands the job back to JavaScript and it deals with the result.

## Asynchronicity

This behaviour is known as _asynchronicity_. A great example of this in real life is when you are buying drinks at a bar. A _synchronous_ bar tender would pour each drink individually, then wait patiently for your card transaction to go through before serving the next customer.

An _asynchronous_ bar tender would start the beer flowing for all the drinks at the same time. While she is waiting for the drinks to pour, she would ring them up and start the card transaction. At that point she could tend to the drinks while the card receipt is printing, then pass the drinks over with the receipt before dealing with the next customer.

When the bar is quiet, the synchronous bar tender would probably fair reasonably well, but when the bar gets busy, the asynchronous bar tender would be able to server more customers and generate more revenue.

The same is true for a web server. With a synchronous language like PHP or Ruby (or basically anything that's not JavaScript), the amount of clients that can be handled by a web application at the same time is much less than with JavaScript. This makes it a great fit for small, high volume web applications.

The other benefit of using JavaScript on the server is that it means that anyone who already knows JavaScript does not have to learn a second language to work on a server-side codebase. And generally most web developers know at least a little JavaScript.

## Server-side JavaScript

In order to run JavaScript it needs a Virtual Machine (VM) and an interface to the host environment. In the browser that interface is the `window` object, which houses a number of browser APIs, including things like `setTimeout`, `alert`, `console` etc.

On the server we use NodeJS to perform the same function as the browser, but instead of the `window` object, we have the `global` object. Instead of browser APIs we have server APIs which replicate `setTimeout`, `console` and the like.

When we use NodeJS, we often split our code into separate files or _modules_. The code from each module needs to _exported_ from the file, so that it can be _imported_ into other files.

Imagine we have a `Car` class, and want to export it into another file:

```js
class Car {
  constructor(make, model, registration, color) {
    this.make = make;
    this.model = model;
    this.registration = registration;
    this.color = color;
    this.currentSpeed = 0;
  }

  accelerate() {
    return this.speed += 10;
  }

  decelerate() {
    return this.speed -= 10;
  }
}

module.exports = Car;
```

Anything we attach to the `module.exports` object will be exported from the file. We can now import it into another file:

>**Note**: This example assumes that the `Car` class is defined in a file called `car.js` in the same folder

```js
const Car = require('./car');

const audi = new Car('Audi', 'TT', 'M34 7PS', 'silver');
audi.accelerate();
```

## npm and Yarn

It is also possible to install 3rd-party libraries and modules using a package manager called npm. This will download and install packages into a special folder called `node_modules` in our node project. These modules can be imported in a similar way:

```sh
npm install morgan
```

```js
const morgan = require('morgan');
```

Notice that when we import a module that we wrote, we must use the relative file path: `require('./car)`. However when we import a 3rd-party module, we simply use the name of the module: `require('morgan')`.

npm is a great tool, but it has recently been superseded by Yarn. Yarn uses npm under the hood, but is quicker and more robust. We use it in the same way as npm but the syntax is slightly different:

| **npm syntax** | **Yarn syntax** | **Behaviour** |
|------------------|-------------------|---------------|
| `npm init` | `yarn init` | Initialises a node project ready to<br>use npm/yarn |
| `npm install --save` | `yarn add` | Installs the package into our<br>project |
| `npm install -g` | `yarn global add` | Installs a global package<br>(generally a command line tool) |
| `npm install --save-dev` | `yarn add --dev` | Installs a package to be used only<br>for development |
| `npm uninstall --save` | `yarn remove` | Uninstalls a package from our<br>project |

It's important that we either use npm or yarn, _but not both_. On this course **we only use `yarn`**.

## Using Yarn

To set up your node project to use Yarn you first need to run:

```sh
yarn init
```

This is similar to `git init`, in that it tells the project that you want to use Yarn to manage all your 3rd-party dependencies.

You will now have to answer a series of questions. 99 times out of 100 you will use the default settings, which you can achieve by submitting an empty response for each.

Once you have answered all the questions Yarn will create a file called `package.json`. This is a manifest file, which keeps a record of all the node modules you have installed in this project and an acceptable version that can be used (something like _greater than 6.1.8_)

Yarn will also create a `yarn.lock` file which maintains a record of the _exact_ version of each node module you have installed in a project, which is useful when deploying an application.

Whenever you want to install a package with Yarn, you simply use `yarn add <package>` and it will install the package into the `node_modules` folder in your project and updates the `package.json` file and `yarn.lock` files accordingly.

## Executing JavaScript on the server

To run a JavaScript file, you can simply run `node` followed by the file name, without the extension. So for example if you wanted to run a script called `index.js`, you would type:

```sh
node index
```

This will execute the file.

## Nodemon

If you want watch for changes in a file, and re-run it every time something changes, you can install a node module called Nodemon:

```sh
yarn global add nodemon
```

Once installed you can simply type `nodemon` and it will run the file that is set as the `main` file in the `package.json` file. The default setting is `index.js`.

You can also specify a file in the command. So to run a file called `car.js` _and_ watch for changes, you would type:

```sh
nodemon car
```

Nodemon is particularly useful when working on a web application, as it will effectively restart the server every time you make a change to your codebase.

## Further reading

- [When, How And Why Use Node.js as Your Backend](https://www.netguru.co/blog/use-node-js-backend)
- [Why the Hell Would You Use Node.js](https://medium.com/the-node-js-collection/why-the-hell-would-you-use-node-js-4b053b94ab8e)
- [Node.js Tutorial for Absolute Beginners](https://www.youtube.com/watch?v=U8XF6AFGqlc)
