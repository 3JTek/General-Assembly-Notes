# React Form Validation

React is not a framework, which means, there are no in-built form validation helpers. We have to roll our own.

>**Note:** This is by no means a definitive solution to the problem of form validation. Different applications might require different solutions, and many man hours could be spent on a highly sophisticated solution.

## The approach

Data validation will be taken care of by the API. If there are any validation errors they will be returned from the API. All we need to do is display them on the form.

The response from the API would have a status code of `422` with the following JSON:

```json
{
  "errors": {
    "name": "This field is required",
    "origin": "This field is required"
  }
}
```

## Accessing the JSON

The actual JSON data of an error response is not very easy to find when using Axios. You can access it like so:

```js
axios.post(`/api/cheeses`, this.state.cheese, {
  headers: { Authorization: `Bearer ${Auth.getToken()}` }
})
  .then(() => this.props.history.push('/cheeses'))
  .catch(err => console.log(err.response.data.errors))
```

If you were to attempt to submit an empty form, you should now see the validation errors in the console:

```js
{
  name: 'This field is required',
  origin: 'This field is required'
}
```

## Displaying the errors

To display the errors, we first need to add them to `state`:

```js
axios.post(`/api/cheeses`, this.state.cheese, {
  headers: { Authorization: `Bearer ${Auth.getToken()}` }
})
  .then(() => this.props.history.push('/cheeses'))
  .catch(err => this.setState({ errors: err.response.data.errors }))
```

We can now display them below the relevant input field like so:

```js
const CheesesForm = ({ handleSubmit, handleChange, cheese, errors }) => {
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Name</label>
        <input name="name" placeholder="Name" onChange={handleChange} />
        {errors.name && <small>{errors.name}</small>}
      </div>
      <div>
        <label>Origin</label>
        <input name="origin" placeholder="Origin" onChange={handleChange} />
        {errors.origin && <small>{errors.origin}</small>}
      </div>

      <button>Submit</button>
    </form>
  )
}
```

## Removing the error when the user updates the field

If a user has generated an error, it is helpful if the error message disappears when the user attempts to rectify the error. To do this we need to remove the error message when the `handleChange` method is called:

```js
handleChange(e) {
  const cheese = { ...this.state.cheese, [e.target.name]: e.target.value }
  const errors = { ...this.state.errors, [e.target.name]: '' }

  this.setState({ cheese, errors })
}
```

Here we are setting the relevant key of the errors object to be an empty string. Since an empty string is _falsey_ the error will no longer display on the form.
