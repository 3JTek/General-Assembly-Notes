# Testing React Components

We have lots of previous experience testing our node APIs with Mocha, Chai, and Supertest (for making mock requests). We can test our front-end React components in a similar way, using Mocha, Chai and Enzyme (used to render components in a test environment).

## Setup

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

```json
"test": "node_modules/.bin/_mocha --require ignore-styles test/helper \"test/**/*_test.js\""
```

You may want to split your test scripts into `test:server` and `test:client` if you are testing front and back.

* There are a few packages we'll need to install too:

```bash
yarn add mocha chai enzyme enzyme-adapter-react-16 jsdom ignore-styles 
```

* Now, in our `rps_test.js` file, let's import what's required, including any components we want to test.

```javascript
/* global describe, it */
import React from 'react';
import { expect } from 'chai';
import { shallow } from 'enzyme';

// import components
import Buttons from '../src/components/Buttons';
import App from '../src/app';
```

## Testing

Now let's write some tests! 

Our first test is going to check that our `<Buttons />` component is going to render 3 buttons. We render our component for test purposes with Enzyme's `shallow()` and then search the component for all elements of type `button`. We know there should be 3 so we check the length of the returned array.

```js
describe('Buttons tests', () => {

it('should render buttons with correct icons', done => {

const wrapper = shallow(<Buttons />);
expect(wrapper.find('button').length).to.equal(3);
done();
});

});
```

Nice! Now run `yarn test` in terminal and watch your test pass! 

It's always a good idea at this point to test for something you aren't expecting and see a test fail. You need to know that your tests are passing because they are good tests!

Now let's update our test to check that the 3 buttons have been rendered with the correct icons in them. This time we our component for an element with a certain value, finding us our button. Then, we check that the child of the element (the icon) has the correct class. The test is repeated for each button:

```js
describe('Buttons tests', () => {

  it('should render buttons with correct icons', done => {

    const wrapper = shallow(<Buttons />);
    expect(wrapper.find({ value: 'Rock' }).childAt(0).hasClass('fa-hand-rock')).to.equal(true);
    expect(wrapper.find({ value: 'Paper' }).childAt(0).hasClass('fa-hand-paper')).to.equal(true);
    expect(wrapper.find({ value: 'Scissors' }).childAt(0).hasClass('fa-hand-scissors')).to.equal(true);
    done();
  });

});
```

Let's now test our function that determines the winner of Rock-Paper-Scissors.

This test renders our `<App />` component, the component in which our game logic lives. Once rendered, we can `setState()` on our test component just as we do normally. Now we have values for player and computer picks, we can invoke our `checkWin()` function that determines the winner of the game.

Our function determines the winner and then updates the state of `<App />` to declare a winner. We can check that our function works by checking the new `winner` state is what we are expecting.

```js
describe('Winner check tests', () => {

  it('should display \'You win\' if player chooses rock and computer chooses scissors', done => {

    const wrapper = shallow(<App />);
    wrapper.setState({ playerPick: 'Rock', computerPick: 'Scissors' });
    wrapper.instance().checkWin();
    expect(wrapper.state().winner).to.equal('You win!');
    done();
  });

});
```

This is only the tip of the iceberg in terms of what we can test. Chances are if you have thought of something you want to test, there is a way to do it.

## Further Reading

* [Enzyme](https://github.com/airbnb/enzyme)
* [Enzyme shallow rendering](https://github.com/airbnb/enzyme/blob/master/docs/api/shallow.md)
* [Chai](http://www.chaijs.com/)

