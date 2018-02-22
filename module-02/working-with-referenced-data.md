# Working with Referenced Data

Referencing is slightly different. Rather than adding data directly to a record, we add a reference to another record. A good example of this might be categories. A category is simply a word or phrase which links multiple records together.

By making a category a reference it can be shared by multiple records. Let's update the cheese model to accept a reference:

```js
const mongoose = require('mongoose');

const schema = new mongoose.Schema({
  name: { type: String, minlength: 2, required: true },
  origin: { type: String, minlength: 2, required: true },
  tastingNotes: { type: String, maxlength: 360, required: true },
  image: { type: String, pattern: /^https?:\/\/.+/ },
  comments: [{
    content: { type: String, required: true }
  }]
  category: { type: mongoose.Schema.ObjectId, ref: 'Category', required: true },
});

module.exports = mongoose.model('Cheese', schema);
```

Here we have added a `category` property which should be an ObjectId, the `ref` part points to the model which we are referencing, in this case the category model.

Let's create the category model:

```js
const mongoose = require('mongoose');

const schema = new mongoose.Schema({
  name: { type: String, minlength: 2, required: true }
});

module.exports = mongoose.model('Category', schema);
```

If we wanted to add a cheese to a category we would now need to display a dropdown of all the categories on the NEW and EDIT forms.

To do that we will need to get the categories from the database when we load those views:

```js
function newRoute(req, res) {
  Category.find()
    .then(categories => res.render('cheeses/new', { categories }));
}

function editRoute(req, res) {
  // get both cheese and categories in parallel
  Promise.props({
    cheese: Cheese.findById(req.params.id),
    categories: Category.find()
  })
    .then(data => res.render('cheeses/edit', data)); // inject the data into the view
}
```

Inside the form we can create the dropdown:

```html
<div class="container">
  <form method="POST" action="/cheeses">
    <div class="field">
      <label class="label">Category</label>
      <select name="category">
        <option selected disabled>Please choose</option>
        <% categories.forEach(category => { %>
          <option value="<%= category._id %>"><%= category.name %></option>
        <% }) %>
      </select>
    </div>
    <div class="field">
      <label class="label">Name</label>
      <input class="input" name="name" type="text" placeholder="Name" required minlength="2">
    </div>
    <div class="field">
      <label class="label">Country of Origin</label>
      <input class="input" name="origin" type="text" placeholder="Country of Origin" required minlength="2">
    </div>
    <div class="field">
      <label class="label">Image</label>
      <input class="input" name="image" type="text" placeholder="Image" required pattern="^http">
    </div>
    <div class="field">
      <label class="label">Tasting Notes (380 chars max)</label>
      <textarea class="textarea" name="tastingNotes" placeholder="Tasting Notes (380 chars max)" maxlength="380"></textarea>
    </div>
    <button class="button is-primary">Submit</button>
  </form>
</div>
```

Notice here that the `value` of the `option` tags has to be the ID of the category we wish to set on the record, since it is the ID that we wish to store in the database.

## Populating

Now that we have a way of creating the reference in our database, it's important to understand that the database now includes the ID for the category, and not the category itself. As things stand if we want to display a cheese's category it will simply be a collection of numbers and letters.

To retrieve the actual category from the database mongoose has a special method called `.populate` which will get the data for the referenced record when we retrieve the data from the database. We can use this method in our controller:

```js
function showRoute(req, res, next) {
  Cheese.findById(req.params.id)
    .populate('category') // load in the whole category record
    .then(cheese => {
      if(!cheese) return res.render('pages/404');
      res.render('cheeses/show', { cheese });
    })
    .catch(next);
}
```

Once we populate, the whole record is added to the cheese's category property, so now to access the category name we can do so like this:

```js
<%= cheese.category.name %>
```

## Populating virtuals

Each cheese now has a category ID stored with it, and, using `populate` we can display each cheese's category. We can also now create a category SHOW page, which can display all the cheeses which belong to that category.

Rather than having to store the cheese IDs on each category record, we can use a virtual to do that for us:

```js
const mongoose = require('mongoose');

const schema = new mongoose.Schema({
  name: { type: String, minlength: 2, required: true }
});

schema.virtual('cheeses', {
  ref: 'Cheese',
  localField: '_id',
  foreignField: 'category'
});

module.exports = mongoose.model('Category', schema);
```

This virtual will use the `_id` of the category and try to match it to the `category` property of the cheese records. It will pull them together into an array called `cheeses` which can then be displayed in a view template.

It's important to note that this virtual also needs to be populated in the controller:

```js
function showRoute(req, res, next) {
  Category.findById(req.params.id)
    .populate('cheeses')
    .then(category => res.render('categories/show', { category }))
    .catch(next);
}
```

```html
<h1><%= category.name %></h1>

<ul>
  <% category.cheeses.forEach(cheese => { %>
    <li><%= cheese.name %></li>
  <% }) %>
</ul>
```

## Further reading

- [Mongoose Population](https://jaketrent.com/post/mongoose-population/)
- [Mongoose Populate](http://mongoosejs.com/docs/populate.html)
- [Mongoose Virtual Populate](http://thecodebarbarian.com/mongoose-virtual-populate.html)
