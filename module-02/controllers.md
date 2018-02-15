# Controllers

In order to link the data from our models to a template, we need to create a controller. Our controller files will hold a set of functions, one for each route that they need to handle. For a RESTful resource that will be all seven RESTful routes: `INDEX`, `CREATE`, `NEW`, `SHOW`, `EDIT`, `UPDATE` and `DELETE`.

Let's take a look at a typical RESTful controller for a songs resource:

```js
const Song = require('../models/song'); // require the model

function indexRoute(req, res) {
  return Song
    .find()
    .exec()
    .then(songs => res.render('songs/index', { songs })) // pass the songs into the index template
    .catch(err => {
      console.log(err); // log the error
      return res.sendStatus(500); // send a 500 response
    });
}

function newRoute(req, res) {
  return res.render('songs/new'); // render the new form
}

function createRoute(req, res) {
  return Song
    .create(req.body)
    .then(() => res.redirect('/songs')) // redirect to the INDEX route
    .catch(err => {
      console.log(err); // log the error
      return res.sendStatus(500); // send a 500 response
    });
}

function showRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404); // send a 404 response
      return res.render('songs/show', { song }); // pass the song into the show template
    })
    .catch(err => {
      console.log(err); // log the error
      return res.sendStatus(500); // send a 500 response
    });
}

function editRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404); // send a 404 response
      return res.render('songs/edit', { song }); // pass the song into the edit template
    })
    .catch(err => {
      console.log(err); // log the error
      return res.sendStatus(500); // send a 500 response
    });
}

function updateRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404); // send a 404 response
      Object.assign(song, req.body); // update the song's properties with the data from the form
      return song.save();
    })
    .then(() => res.redirect(`/songs/${req.params.id}`) // redirect to the SHOW route
    .catch(err => {
      console.log(err); // log the error
      return res.sendStatus(500); // send a 500 response
    });
}

function deleteRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404); // send a 404 response
      return song.remove(); // delete the song from the database
    })
    .then(() => res.redirect('/songs'))
    .catch(err => {
      console.log(err); // log the error
      return res.sendStatus(500); // send a 500 response
    });
}

module.exports = {
  index: indexRoute,
  new: newRoute,
  create: createRoute,
  show: showRoute,
  edit: editRoute,
  update: updateRoute,
  delete: deleteRoute
};
```

Now that we have created a controller, we can connect it to the router in our `config/router.js` file:

```js
const router = require('express').Router();
const songs = require('../controllers/songs');

router.route('/songs')
  .get(songs.index)
  .post(songs.create);

// NEW SHOULD COME BEFORE SHOW
router.route('/songs/new')
  .get(songs.new);

router.route('/songs/:id')
  .get(songs.show)
  .put(songs.update)
  .delete(songs.delete);

router.route('/songs/:id/edit')
  .get(songs.edit);
```

It's important that the `/songs/new` route appears above the `/songs/:id` route, otherwise the mistake `new` for an ID and attempt to load the SHOW route.
