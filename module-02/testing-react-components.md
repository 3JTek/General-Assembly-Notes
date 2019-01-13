# Testing React Components

To test React components we are going to use Mocha and Chai, but also a few other packages:

* Enzyme: A testing utility specifically designed for testing React components
* JSDOM: A utility that generates a virtual browser in the terminal, so that we can run front-end tests that output to the terminal.
* Sinon: A tool for faking AJAX requests amongst other things

## Setup

First of all, let's create a `test` directory in the root folder of our projects.

Within the `test` directory, create a `helper.js` file, and add the following configuration code:

```js
// allows us to write our tests in ES6
require('@babel/register')()

// setup JSDOM
const { JSDOM } = require('jsdom')

const { window } = new JSDOM(`
  <!DOCTYPE html>
  <html>
  <body></body>
  </html>
`)

// setup Enzyme
const Adapter = require('enzyme-adapter-react-16')
require('enzyme').configure({ adapter: new Adapter() })

// copy any global properties from `window` to `global`
const props = Object.getOwnPropertyNames(window)
  .filter(prop => typeof global[prop] === 'undefined')
  .map(prop => Object.getOwnPropertyDescriptor(window, prop))

Object.defineProperties(global, props)

global.window = window
global.document = window.document
```

This file is essentially enabling us to render a fake DOM when we run our tests. Components will be rendered in our new `JSDOM` template.

We must also create a script in our `package.json` to let us run our tests in terminal:

  ```json
  "scripts": {
    "test": "mocha --require test/helper --require ignore-styles test/**/*_spec.js"
  }
  ```

* Install the packages into `devDependencies`:

  ```bash
  yarn add mocha chai enzyme enzyme-adapter-react-16 jsdom ignore-styles sinon --dev
  ```

## Testing

Enzyme gives us two methods which allow us to test components.

* `shallow`: For testing _functional_ components. This can be used for _classical_ components as well, but will not invoke any lifecycle hooks like `componentDidMount`
* `mount`: For testing _classical_ components. This will call lifecycle hooks, which may have side effects like making AJAX requests.

### Functional component

Functional components are simpler to test because they do not have any logic and are purely presentational. When we test a functional component we can render it with Enzyme and check that the correct DOM elements have been rendered.

We can also pass it props to check that it renders any data correctly.

Here's an example of some tests for a form:

```js
/* global describe, it */
import React from 'react'
import { expect } from 'chai'
import { shallow } from 'enzyme'
import BurgersForm from '../../src/components/cheese/Form'

describe('BurgersForm', () => {
  it('should render 3 input fields, 1 textarea and 1 select field', done => {
    const state = {
      cheese: {},
      errors: {}
    }

    component = shallow(<BurgersForm cheese={state.cheese} errors={state.errors} />)
    expect(component.find('input').length).to.eq(3)
    expect(component.find('textarea').length).to.eq(1)
    expect(component.find('select').length).to.eq(1)
    done()
  })

  it('should populate the form', done => {
    const state = {
      cheese: {
        name: 'Cheddar',
        origin: 'England'
      },
      errors: {}
    }

    component = shallow(<BurgersForm cheese={state.cheese} errors={state.errors} />)
    expect(component.find({ name: 'name' }).instance().value).to.eq('Cheddar')
    expect(component.find({ name: 'origin' }).instance().value).to.eq('England')
    done()
  })

  it('should display errors', done => {
    const state = {
      cheese: {},
      errors: {
        name: 'This field is required',
        origin: 'This field is required'
      }
    }

    component = shallow(<BurgersForm cheese={state.cheese} errors={state.errors} />)
    expect(component.find('small').length).to.eq(2)
    done()
  })
})
```

As you can see we are really just rendering the component and then checking the number of DOM elements, and their properties.

Notice if we want to access the underlying DOM element, rather than the React component, we can use the `.instance()` method. In this way we can access properties of the DOM elements directly, like `value` on an `input` field for example.

We should not test user input with _functional_ components, since that logic requires `state` which lives in the parent component. Which we'll look at next.

### Classical component

A classical component is generally harder to test. There is a lot more going on when the component is rendered. Most classical have logic, they _do something_ which in turn updates the UI.

Any user action can be simulated and tests can be written to check that the component has updated correctly afterward.

Classical components may also make requests to APIs using AJAX. **We should allow our components to actually make requests to external resources during our tests**. If we were to do that I tests may fail if the external resource is down. Instead we need to fake the response from the server and test that _if the server were to respond with this data, would the component behave as expected_.

Let's take a look at some tests for a component that handles user input, and makes AJAX requests. An _EDIT_ component is a good example. It needs to load in the data from an API, then modify the data as the user types.

#### Faking AJAX requests

```js
/* global describe, it, before, after, beforeEach */
import React from 'react'
import Promise from 'bluebird'
import axios from 'axios'
import sinon from 'sinon'
import { expect } from 'chai'
import { mount } from 'enzyme'
import { MemoryRouter, Route } from 'react-router-dom'
import CheesesEdit from '../../src/components/cheeses/CheesesEdit'

// fake data to send in place of the actual AJAX request
const data = {
  _id: "1",
  name: "Cheddar",
  origin: "England"
}

describe('CheesesEdit', () => {
  let component
  let promise

  before(done => {
    // create a promise that resolves with the fake cheese data above
    promise = Promise.resolve({ data })
    // stub the AJAX request so that when axios makes a get request, it send back the fake data instead
    sinon.stub(axios, 'get').returns(promise)
    done()
  })

  after(done => {
    // remove the stub from axios, so it behaves normally again
    axios.get.restore()
    done()
  })

  beforeEach(done => {
    component = mount(
      // mount the component using MemoryRouter setting a URL so that
      // `this.props.match.params.id` is set correctly
      <MemoryRouter initialEntries={['/cheeses/1/edit']}>
        <Route path="/cheeses/:id/edit" component={CheesesEdit} />
      </MemoryRouter>
    )
    done()
  })

  it('should populate the form', done => {
    // wait for the axios to call the promise
    promise.then(() => {
      // once the promise has resolved update the component
      component.update()
      expect(component.find({ name: 'name' }).instance().value).to.eq('Cheddar')
      expect(component.find({ name: 'origin' }).instance().value).to.eq('England')
      done()
    })
  })
})
```

There's quite a lot going on here, so let's break it down a little:

First we create some fake data. In the `before` hook we use `sinon` to return the fake data from `axios`, suppressing the actual AJAX request whenever a GET request is made in Axios. We restore Axios back the way it was in the `after` hook.

In the `beforeEach` hook we render the component using Enzyme's `mount` method. Since the `CheesesEdit` component relies on the `Router`, we need to fake that as well using the `MemoryRouter` which was created for that purpose. We can supply a URL with the `initialEntries` prop.

Finally we are able to test our component. In the `it` block, we first need to wait for the promise containing the fake data to be resolved, then we can update the component so that any effects of the fake data will take place (ie populating the form).

#### Faking user input

We can now test that updating the input fields will update the data on state. To do this we need to simulate the `change` event on the input fields, then check the component's `state` object:

```js
it('should handle user input', done => {
  promise.then(() => {
    component.find({ name: 'name' })
      .simulate('change', { target: { name: 'name', value: 'Mozzarella' }})

    component.find({ name: 'origin' })
      .simulate('change', { target: { name: 'origin', value: 'Italy' }})

    const state = component.find(CheesesEdit).state()

    expect(state.data.name).to.eq('Mozzarella')
    expect(state.data.origin).to.eq('Italy')
    done()
  })
})
```

Here we find each input using the `find` method as before. We can simulate the `change` event using Enzyme's `simulate` method. The first argument is the event that we want to trigger, and  the second argument is the event object that would be sent from the DOM element. He we can fake the event object to send the data that we are testing that the user added.

We can get the state from our component, but first we need to find it. The component we are testing is actually the `MemoryRouter`. The `CheesesEdit` is nested two levels deep inside that. So we first need to get it using the `find` method.

Once we get, we can use the `state` method to grab the component's state object. In the example above the form data is being added to a `data` property on the state which is why we are checking `state.data.name`.

## Summary

React testing can be quite challenging because there is a lot of setup and a lot of the Enzyme methods are not particularly intuitive. However we have covered everything needed to write tests for a RESTful app.

There are some great helper methods that can be used when writing tests for React components. The `debug` method is particularly useful:

```js
console.log(component.debug())
```

This would output something like:

```
<CheesesForm data={{...}} handleChange={[Function: bound handleChange]} handleSubmit={[Function: bound handleSubmit]} errors={{...}}>
  <form onSubmit={[Function: bound handleSubmit]}>
    <div className="field">
      <label className="label">
        Name
      </label>
      <div className="control">
        <input className="input" placeholder="Name" name="name" onChange={[Function: bound handleChange]} value="Cheddar" />
      </div>
    </div>
    <div className="field">
      <label className="label">
        Origin
      </label>
      <div className="control">
        <input className="input" placeholder="Origin" name="origin" onChange={[Function: bound handleChange]} value="England" />
      </div>
    </div>
    <button className="button is-primary">
      Submit
    </button>
  </form>
</CheesesForm>
```

Which allows you to see exactly what you are working with.

## Further Reading

* [Enzyme](https://github.com/airbnb/enzyme)
* [Enzyme shallow rendering](https://github.com/airbnb/enzyme/blob/master/docs/api/shallow.md)
* [Enzyme cheatsheet - DevHints.io](https://devhints.io/enzyme)
* [Getting started - Sinon](https://sinonjs.org/#get-started)
* [JavaScript Testing with Mocks, Spies & Stubs - SitePoint](https://www.sitepoint.com/sinon-tutorial-javascript-testing-mocks-spies-stubs/)
