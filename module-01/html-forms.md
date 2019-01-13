# HTML Forms

One of the most important features of a website is the ability to interact with it. One of the ways of doing that is through the use of a form. Almost all websites have at least one form: a login form, a signup form, a contact form.

Since forms are a point of contact between a company and its client base, it's important that forms are easy to use and give useful feedback if the user enters incorrect data.

## Structure

The `form` tag is a wrapper for the content of the form. Only elements inside the form are part of the form.

### The `<label>` tag.

A `label` is used to indicate what information the user is expected to enter in each field of the form. A label can be associated with a specific input either with a `for` attribute, containing the input's id:

```html
<label for="name">Name</label>
<input type="text" id="name">
```

Or by placing the input _inside_ the label. This is particularly useful with radio buttons and checkboxes:

```html
<label>
  <input type="radio"> Pick me!
</label>
```

### The `<input>` tag.

An input is a form field that can hold data from the user. It is a self-closing element, like `img`. It has a `type` attribute which tells the browser what type of data the field should contain, and how it should behave. Here's a list of input types and what data they accept:

| type | info |
| :--- | :--- |
| `button` | a simple button \(no default behaviour\) |
| `checkbox` | boolean value \(true or false; can be checked by default with the `checked` attribute\) |
| `color` | colour \(opens a colour picker; takes the format \#RRGGBB\) |
| `date` | date \(opens a date picker\) |
| `email` | valid email address |
| `file` | file to be uploaded |
| `hidden` | alpha-numeric characters \(not displayed to the user\) |
| `image` | a button with an image \(submits form and adds mouse co-ordinates to form data\) |
| `month` | month & year \(opens a date picker\) |
| `number` | numbers only \(requires `step` attribute for decimals\) |
| `password` | alpha-numeric characters \(characters are hidden with a •\) |
| `radio` | select one of many options \(can be checked by default with the `checked` attribute\) |
| `range` | number between two points \(dislpays a slider; can be modified with `min`, `max` and `step` attributes |
| `reset` | a button that resets the form |
| `search` | alpha-numeric characters \(line-breaks are removed\) |
| `submit` | submits the form \(requires a `value` attribute\) |
| `tel` | telephone number \(can be modified with `pattern` and `maxlength` attributes |
| `text` | alpha-numeric characters \(line-breaks are removed\) |
| `time` | time \(HH:MM format\) |
| `url` | a valid URL \(can be modified with `pattern` and `maxlength` attributes |
| `week` | week of the year \(opens a week picker\) |

#### Input tag attributes

There are a few attributes that are useful when working with input fields:

```html
<input type="text" autofocus /> <!-- field will be focused on page load -->
<input type="text" autocomplete="off" /> <!-- will prevent the field showing previous user entries -->
<input type="text" disabled /> <!-- prevents the user from editing the field's contents -->
<input type="checkbox" checked /> <!-- will be checked on page load, also works with radio buttons -->
```

### The `<select>` & `<option>` tags

A `select` tag displays a dropdown menu. It contains `options`, each item in the drop down menu. Each option can be `selected` (ie that option is displayed when the page is loaded) and `disabled` (ie cannot be selected by the user). Typically a select has an instructional menu option which is `selected` and `disabled`.

```html
<select>
  <option selected disabled>Please choose...</option>
  <option>Bernie Sanders</option>
  <option>Donald Trump</option>
  <option>Hillary Clinton</option>
</select>
```

### The `<textarea>` tag

If you need to handle a lot of data from the user, a review or blog post for example, you can use a `textarea`. This will accept multi-lines of text and will keep line-breaks. It also takes `row` and `col` attributes which we traditionally used to set the dimensions of the input, however it is more common to use CSS these days.

```html
<textarea></textarea>
```

### The `name` attribute

If you want to actually collect the data from a form, you **must** add a name attribute.

```html
<input type="text" /> <!-- no data collected, the field is useless! -->
<input type="text" name="firstname" /> <!-- data collected -->
```

### The `value` attribute

You can set the value of the data for a form element using the `value` attribute. For `input` fields this will set the initial content of the field.

For `radio` buttons and `checkboxes`, it will set the value of each item. The value of the selected item will be added to the form data.

For `submit` buttons, it will set the button text, and will also be added to the form data.

With dropdown menus a `value` attribute can be added to the `option` tag. The value inside the `value` attribute of the selected `option` will be added to the form data, rather than the menu text.

Textareas do not have a value attribute. However, you can add text between the tags which will behave in the same way.

## The `<button>` tag

There are several HTML elements that can submit a form. The two most common are `<input type="submit">` or `<button>`.

Without any attributes, if a `<button>` tag is inside a `<form>` it will submit the form when clicked.

The button tag can have the following `types`:

| type | info |
| :--- | :--- |
| `submit` | Submits the form (default) |
| `reset` | Resets the form |
| `button` | Does nothing |

`<button type="button">`**?!!**

Yep, weird, I know. The `button` type is used to basically stop the button from having any default behaviour. The idea being that it would be used in conjunction with some custom JavaScript.

### See also

* [My First HTML Form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/My_first_HTML_form)
* [How To Structure A HTML Form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/How_to_structure_an_HTML_form)
* [Mozilla HTML Forms Guide](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms)
