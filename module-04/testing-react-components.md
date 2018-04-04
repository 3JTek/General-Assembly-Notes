# Testing React Components

We have lots of previous experience testing our node APIs with Mocha, Chai, and Supertest (for making mock requests). We can test our front-end React components in a similar way, using Mocha, Chai and Enzyme (used to render components in a test environment).

* First of all, lets create a `test` directory in the root folder of our projects.

You may have server-side tests in this folder too, but for now we'll pretend that we are just testing a front-end application. 

* Within the `test` directory, create two files: `helper.js` and `rps_test.js`.

In this example, we are testing a small React Rock-Paper-Scissors app, hence `rps`, but you can name your own tests appropriately.

* In `helper.js` paste the following configuration code:

```js
process.env.NODE_ENV = 'test';
require('babel-register')();

function nullFunc() {
  return null;
}

require.extensions['.css'] = nullFunc;
require.extensions['.scss'] = nullFunc;
require.extensions['.png'] = nullFunc;
require.extensions['.jpg'] = nullFunc;

const { configure } = require('enzyme');
const Adapter = require('enzyme-adapter-react-16');

configure({ adapter: new Adapter() });

const { JSDOM } = require('jsdom');

const jsdom = new JSDOM('<!doctype html><html><body></body></html>');
const { window } = jsdom;

function copyProps(src, target) {
  const props = Object.getOwnPropertyNames(src)
    .filter(prop => typeof target[prop] === 'undefined')
    .map(prop => Object.getOwnPropertyDescriptor(src, prop));
  Object.defineProperties(target, props);
}

copyProps(window, global);
```

This file is essentially enabling us to render a fake DOM when we run our tests. Components will be rendered in our new `JSDOM` template.

We must also create a script in our `package.json` to let us run our tests in terminal.

* Add the following script just underneath any existing `start` or `build` scripts:

```
"test": "node_modules/.bin/_mocha --require ignore-styles test/helper \"test/**/*_test.js\""
```

You may want to split our your test scripts into `test:server` and `test:client` if you are testing front and back.



