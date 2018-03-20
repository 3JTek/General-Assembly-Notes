# Handling Form Errors

## Connecting the form to the controller

Handling form errors with Angular is slightly involved. It requires linking the form to a controller using the form's `name` property:

```html
<form name="placesNew.form" ng-submit="placesNew.handleSubmit()">
  ...
</form>
```

Once this has been done you can access a huge amount of information about the form and the fields it contains.

## Adding HTML5 validations

If you add HTML5 validations to the form fields, Angular will ascertain whether the field and therefore the form itself is valid or not.

```html
<form name="placesNew.form" ng-submit="placesNew.handleSubmit()">
  <label for="username">Username</label>
  <input type="text" id="username" name="username" required minlength="2" />

  <button>Submit</button>
</form>
```

If we were to log `placesNew.form` with the form above we would see the following information:

```js
{
  "$error": {
    "required": [
      {
        "$validators": {},
        "$asyncValidators": {},
        "$parsers": [],
        "$formatters": [
          null
        ],
        "$viewChangeListeners": [],
        "$untouched": true,
        "$touched": false,
        "$pristine": true,
        "$dirty": false,
        "$valid": false,
        "$invalid": true,
        "$error": {
          "required": true
        },
        "$name": "name",
        "$options": {}
      }
    ]
  },
  "$name": "placesNew.form",
  "$dirty": false,
  "$pristine": true,
  "$valid": false,
  "$invalid": true,
  "$submitted": false,
  "username": {
    "$validators": {},
    "$asyncValidators": {},
    "$parsers": [],
    "$formatters": [
      null
    ],
    "$viewChangeListeners": [],
    "$untouched": true,
    "$touched": false,
    "$pristine": true,
    "$dirty": false,
    "$valid": false,
    "$invalid": true,
    "$error": {
      "required": true
    },
    "$name": "name",
    "$options": {}
  }
}
```

Some of the more interesting parts of the data are as follows:

| **Property** | **Meaning** |
|--------------|-------------|
| `$dirty` | The field has been changed |
| `$pristine` | The field has not been changed |
| `$touched` | The field has been focussed and blurred |
| `$untouched` | The field has yet to be blurred |
| `$valid` | The data in the field passes the HTML5 validations |
| `$invalid` | The data in the field does not pass the HTML5 validations |
| `$error` | The current form error, each property of the error matches a validation |

You should notice that `username` is a property of the `placesNew.form` object, which refers to the field with the name of `username`, and provides information about that field.

As a user types in the form, the information automatically updates. This allows us to dynamically display messages to the user.

## Angular Messages

To help with this there is an external module called Angular Messages which was created by the Angular core team.

It can be installed with `npm` or `yarn`:

```sh
yarn add angular-messages
```

And included in your app in `src/app.js`:

```js
import angular from 'angular';
import 'angular-messages';

angular.module('myAwesomeApp', ['ngMessages']);
```

We now have two directives at our disposal: `ngMessages` and `ngMessage` which can be used to display the relevant message depending on the current field's error:

```html
<div ng-messages="placesNew.form.username.$error">
  <div ng-message="required">Please fill in this field</div>
  <div ng-message="minlength">You must use at least 2 characters</div>
</div>
```

As you can see, we need to pass `placesNew.form.username.$error` to our `ngMessages`. `placesNew.form.username.$error` is an object which would look something like this:

```js
{
  required: true
}
```

If the field was empty, and:

```js
{
  minlength: true
}
```

If the field only had one character in it.

The `ngMessages` directive will decide which `ngMessage` directive to display based on the string passed as an argument, so `<div ng-message="required">Please fill in this field</div>` will be displayed if the `$error` object has a property of `required`.

If you start to type in the field now, you will see the errors changing based on the contents of the field.

## Hiding messages on page load

We don't really want to display error messages to a user who hasn't had a chance to enter anything into the form at all, so using `$dirty` and `ngIf` we can only display messages once the user has started typing:

```html
<div ng-messages="placesNew.form.username.$error" ng-if="placesNew.form.username.$dirty">
  <div ng-message="required">Please fill in this field</div>
  <div ng-message="minlength">You must use at least 2 characters</div>
</div>
```

Now the errors will only display once the user starts typing in the form.

## Displaying messages on form submission

There is another issue, what if the user submits the form without entering a username at all? We need to make sure the form doesn't submit, but also we need to display the messages.

Firstly though we need to disable the HTML5 form errors otherwise our form will look like this:

![](https://user-images.githubusercontent.com/3531085/37649052-a185112a-2c28-11e8-961e-c9699a4106b5.png)

To suppress these default error messages we can add a `novalidate` attribute to the form:

```html
<form name="placesNew.form" ng-submit="placesNew.handleSubmit()" novalidate>
  ...
</form>
```

Now we can update our `ngIf` directives to also display the error if the form was submitted:

```html
<div ng-messages="placesNew.form.username.$error" ng-if="placesNew.form.username.$dirty || placesNew.form.$submitted">
  <div ng-message="required">Please fill in this field</div>
  <div ng-message="minlength">You must use at least 2 characters</div>
</div>
```

However this won't stop the form from being submitted.

## Preventing an invalid form from being submitted

Finally to stop the form from being submitted we can write some logic in the `handleSubmit` method in the controller:

```js
function handleSubmit() {
  if(this.form.$invalid) return false;

  Place.create(this.newPlace)
    .then(() => $state.go('placesIndex'));
}
```

So if the form is invalid, we `return false` which will stop the function at that line and never try to create the request, however the error messages will now display.

## Further reading

- [Angular Messages Docs](https://docs.angularjs.org/api/ngMessages/directive/ngMessages)
- [AngularJS Form Validation with NgMessages](https://scotch.io/tutorials/angularjs-form-validation-with-ngmessages)
