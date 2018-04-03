# React Form Validation

React is not a framework, which means, unlike frontend frameworks like Angular, there is no in-built form validation helpers. We have to roll our own.

In this session we'll look at a very simple approach to handling form errors and displaying them to the user.

>**Note:** This is by no means a difinitive solution to the problem of form validation. Different applications might require different solutions, and many man hours could be spent on a highly sophisticated solution.

## The approach

Rather than using front-end validation we'll allow the user to post a form and handle validation on the server-side. If there are any validation errors, we'll send them back to the React app to display on the form.

## Server-side

Install the dependencies of the starter-code with `yarn install` and start the server with `yarn start:server`.

Open Insomnia and make a `POST` request to `http://localhost:4000/api/criminals` with the following payload:

```json
{
  "status": "Alive"
}
```

You should see the following response:

```json
{
	"message": "Unprocessable Entity",
	"errors": "ValidationError: location: Path `location` is required., name: Path `name` is required."
}
```

This does give us all the info we need, but it's not that easy to work with. Let's update `lib/errorHandler.js` to give us an errors object:

```js
if(err.name === 'ValidationError') {
  err.status = 422;
  err.message = 'Unprocessable Entity';

  const errors = {};
  for(const key in err.errors) {
    errors[key] = err.errors[key].message;
  }
  err.errors = errors;
}
```

Ok, run the same `POST` request in Insomnia, you should see the following response:

```json
{
	"message": "Unprocessable Entity",
	"errors": {
		"location": "Path `location` is required.",
		"name": "Path `name` is required."
	}
}
```

Ok, we're getting there, but the messages are not very human-readable. Luckily it's trivial to set custom messages in the model.

Modify `models/criminal.js`:

```js
const criminalSchema = mongoose.Schema({
  name: { type: String, required: 'Please provide a name' },
  location: { type: String, required: 'Please provide a location' },
  status: { type: String, default: 'Unknown' }
});
```

Ok run the `POST` request once again, and you should see this:

```json
{
	"message": "Unprocessable Entity",
	"errors": {
		"location": "Please provide a location",
		"name": "Please provide a name"
	}
}
```

Great! We're done with the server, it's time to display the errors to the user in React.

## Client-side

We want to update the `CriminalsNew` component to grab the error messages from the request and pass them into the `CriminalsForm` component to be displayed.

Let's update the `handleSubmit` method in `CriminalsNew`:

```js
handleSubmit = (e) => {
  e.preventDefault();

  Axios.post('/api/criminals', this.state.criminal)
    .then(() => this.props.history.push('/'))
    .catch(err => console.log(err.response.data.errors));
}
```

Fire up the React app with `yarn start:client` and submit the empty form. In the console, you should see the following:

```js
{location: "Please provide a location", name: "Please provide a name"}
```

Ok, let's set those messages to state, so we can pass them into the form:

```js
state = {
  criminal: { ... },
  errors: {}
}

handleSubmit = (e) => {
  e.preventDefault();

  Axios.post('/api/criminals', this.state.criminal)
    .then(() => this.props.history.push('/'))
    .catch(err => this.setState({ errors: err.response.data.errors }));
}

render() {
  return (
    <CriminalsForm
      handleChange={this.handleChange}
      handleSubmit={this.handleSubmit}
      criminal={this.state.criminal}
      errors={this.state.errors}
    />
  );
}
```

Ok, inside the `CriminalsForm` let's grab the errors from `props` and display them, if they exist:

```js
const CriminalsForm = ({ handleSubmit, handleChange, criminal, errors }) => {

  return (
    <form onSubmit={handleSubmit} noValidate>
      <div>
        <label htmlFor="name">Name:</label>
        <input type="text"
          id="name"
          name="name"
          placeholder="Prof. Moriarty"
          onChange={handleChange}
          value={criminal.name}
        />
        {errors.name && <small>{errors.name}</small>}
      </div>
      ...
    </form>
  );
};
```

Ok, great, now if we post an empty form we'll get an error message displaying under the relevant form field.

## Removing the errors

We want to remove error messages if a user beings to type in the relevant input field. To do this we can simply clear the error message when we update `state` in the `handleChange` method:

```js
handleChange = ({ target: { name, value } }) => {
  const criminal = Object.assign({}, this.state.criminal, { [name]: value });
  const errors = Object.assign({}, this.state.errors, { [name]: '' });
  this.setState({ criminal, errors });
}
```

Now as soon as a user starts typing in a field, the field's error message will instantly disappear.

## Disabling the form button

For a final touch let's disable the form if there are error messages currently being displayed.

Inside the `CriminalsForm` component let's create a boolean that is `true` when there are form errors:

```js
const formInvalid = Object.keys(errors).some(key => errors[key]);
```

There's actually quite a lot going on here, so let's break it down a little:

```js
Object.keys(errors); // => ['name', 'location']
```

`Object.keys` returns an array of the keys of an object.

```js
['name', 'location'].some(key => errors[key]); // true
```

`.every()` is an array method that iterates over an array and checks that some of the elements pass the test. In this case we are simply returning the value of each key in the object.

Since a string like `Please provide a name` is truthy, and an empty string is falsy, if any one of the values in the array is a string with at least one character, the `some` method will return `true`, which will set the `formInvalid` variable to be `true`.

We can now update the form button to be disabled if `formInvalid` is true:

```js
<button disabled={formInvalid}>Submit</button>
```

So the user is allowed to submit an invalid form the first time, but must clear out all the errors in order to submit the form a second time. This will stop a user from continuously submitting a form with the same errors.

That's it, we're done! 💥
