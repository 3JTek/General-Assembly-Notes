# Working with Embedded Data

When we embed data we store the data with a specific record. A good example of this might be adding comments to a blog post for example. The comments are only relevant to the specific blog post and would not be shared by multiple posts. Embedding is a good choice here, since if we delete the blog post, the comments will be deleted with it.

Let's look at how we might set up comments in an Express app. Firstly we will need to update the model:

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
});

module.exports = mongoose.model('Cheese', schema);
```

Here we have created a property `comments` which will be an array of objects, each of which will have a `content` property, and will also be given an `_id` when created.

With that done, we'll need to create a form, and a controller to handle it.

Before we build that out, it would be worth looking at the extra RESTful routes we are going to need to perform CRUD actions on our comments:

| **Route** | **Path** | **Verb** |
|-----------|----------|----------|
| INDEX | `/cheeses/:id/comments` | GET |
| NEW | `/cheeses/:id/comments/new` | GET |
| CREATE | `/cheeses/:id/comments` | POST |
| SHOW | `/cheeses/:id/comments/:commentId` | GET |
| EDIT | `/cheeses/:id/comments/:commentId/edit` | GET |
| UPDATE | `/cheeses/:id/comments/:commentId` | PUT / PATCH |
| DELETE | `/cheeses/:id/comments/:commentId` | DELETE |

As you can see we can create a full set of RESTful routes for an embedded record, however, realistically we would only use a few of them.

Imagine we are adding comments to a cheese. The form would most probably appear on the SHOW page of the cheese, and there might be a delete button for each comment there as well. The SHOW page of the cheese would also display all of the comments for that cheese, and vary rarely would we want to display a single comment on its own.

For comments then, we will only need the following routes:

| **Route** | **Path** | **Verb** |
|-----------|----------|----------|
| CREATE | `/cheeses/:id/comments` | POST |
| DELETE | `/cheeses/:id/comments/:commentId` | DELETE |

These would handle the form on the cheese SHOW page, and the delete button next to each comment.

## Controller actions

Since the embedded data is on the cheese model, it would make sense to add the controller actions to the cheeses controller:

```js
function commentCreateRoute(req, res, next) {
  // find the relevant cheese
  Cheese.findById(req.params.id)
    .then(cheese => {
      // push the data from the form into the `comments` array
      cheese.comments.push(req.body);

      // save the parent record
      return cheese.save();
    })
    .then(cheese => res.json(cheese)) // reload the cheese SHOW page
    .catch(next);
}

function commentDeleteRoute(req, res, next) {
  // find the relevant cheese
  Cheese.findById(req.params.id)
    .then(cheese => {
      // find the specific comment
      const comment = cheese.comments.id(req.params.commentId);
      // remove the comment from the parent record
      comment.remove();

      // save the parent record
      return cheese.save();
    })
    .then(cheese => res.json(cheese)) // reload the cheese SHOW page
    .catch(next);
}
```

Now that the controller actions has been written, we just need to hook them up to the router:

```js
router.post('/cheeses/:id/comments', cheeses.commentCreate);
router.delete('/cheeses/:id/comments/:commentId', cheeses.commentDelete);
```

## Further reading

- [Schema Within A Schema: Use Embedded Documents in Mongoose/Mongo](http://www.jonahnisenson.com/schema-within-a-schema-use-embedded-documents-in-mongoosemongo/)
- [Mongoose Subdocuments](http://mongoosejs.com/docs/subdocs.html)
