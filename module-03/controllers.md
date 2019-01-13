# Controllers

In order to link the data from our models to a template, we need to create a controller. Our controller files will hold a set of functions, one for each route that they need to handle. For a RESTful resource that will be five RESTful routes on the server side: `INDEX`, `CREATE`, `SHOW`, `UPDATE` and `DELETE`.

Let's take a look at a typical RESTful controller for a songs resource:

```js
const Song = require('../models/song') // require the model

function indexRoute(req, res, next) {
  return Song
    .find()
    .exec()
    .then(songs => res.json(songs))
    .catch(next)
}

function createRoute(req, res, next) {
  return Song
    .create(req.body)
    .then(song => res.json(song))
    .catch(next)
}

function showRoute(req, res, next) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404)
      return res.json(song)
    })
    .catch(next)
}

function editRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404)
      return res.json(song)
    })
    .catch(next)
}

function updateRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404)
      song.set(req.body) // update the song's properties with the data from the request
      return song.save()
    })
    .then(song => res.json(song))
    .catch(next)
}

function deleteRoute(req, res) {
  return Song
    .findById(req.params.id)
    .exec()
    .then(song => {
      if(!song) return res.sendStatus(404)
      return song.remove() // delete the song from the database
    })
    .then(() => res.sendStatus(204)) // send a 204 No Content status code
    .catch(next)
}

module.exports = {
  index: indexRoute,
  new: newRoute,
  create: createRoute,
  show: showRoute,
  edit: editRoute,
  update: updateRoute,
  delete: deleteRoute
}
```

Now that we have created a controller, we can connect it to the router in our `config/router.js` file:

```js
const router = require('express').Router()
const songs = require('../controllers/songs')

router.route('/songs')
  .get(songs.index)
  .post(songs.create)


router.route('/songs/:id')
  .get(songs.show)
  .put(songs.update)
  .delete(songs.delete)
```
