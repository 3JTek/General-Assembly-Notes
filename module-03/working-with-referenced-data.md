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

## Populating

Now that we have a way of creating the reference in our database, it's important to understand that the database now includes the ID for the category, and not the category itself. As things stand if we want to display a cheese's category it will simply be a collection of numbers and letters.

To retrieve the actual category from the database mongoose has a special method called `.populate` which will get the data for the referenced record when we retrieve the data from the database. We can use this method in our controller:

```js
function showRoute(req, res, next) {
  Cheese.findById(req.params.id)
    .populate('category') // replace the ID with the whole category record
    .then(cheese => res.render(cheese))
    .catch(next);
}
```

## Populating virtuals

Each cheese now has a category ID stored with it, and, using `populate` we can display each cheese's category. We can also now display all the cheeses which belong to that category.

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
    .then(category => res.json(category))
    .catch(next);
}
```

## Further reading

- [Mongoose Population](https://jaketrent.com/post/mongoose-population/)
- [Mongoose Populate](http://mongoosejs.com/docs/populate.html)
- [Mongoose Virtual Populate](http://thecodebarbarian.com/mongoose-virtual-populate.html)
