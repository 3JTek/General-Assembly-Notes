# Token Authentication

## What is token authentication?

With an API we can generate a token which is passed between the client and the server. Think of it like a train ticket. When the client authenticates she receives a unique token with her user ID encoded in to it. Whenever the client makes a request she sends the token along with the request in an Authorization header. This is then checked by the server before sending back any data.

The Authorization header can be one of several options, the most common being `Basic` or `Bearer`. Since we are using tokens, we should use the `Bearer` type. Our header should look something like this:

```
Authorization: Bearer <token>
```

## JSON Web Tokens

The technology we will be using is JWT (pronounced 'jot'), which stands for **JSON Web Token**. It allows us to embed JSON into an encrypted token.

A JWT consists of three parts:

- **Header**: which contains information about the token, encryption method etc
- **Payload**: which contains any data that we want to store in the token (most commonly the user's ID)
- **Signature**: which contains the header and payload encrypted with a secret. The secret is stored on the server and is used to generate the token. If a token's signature cannot be decrypted using the correct secret it is deemed to be invalid, and the user is refused access to the requested resource.

A typical JWT might look like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.XbPfbIHMI6arZ3Y922BhjWgQzWXcXNrz0ogtVhfEd2o
```

You can see each section delineated by a period `.`.

## Usage

It is fairly straight forward to create a JWT using a node library. My preferred one is jsonwebtoken which can be installed with yarn:

```sh
yarn add jsonwebtoken
```

### Creating a token

The syntax for creating a JWT is as follows:

```js
const jwt = require('jsonwebtoken')

// `sub` means subject, ie the subject of the token, and usually contains the user's ID
const payload = { sub: 1526 }
const secret = 'shhhh!'
const options = { expiresIn: '1hr' }

const token = jwt.sign(payload, secret, options)
```

### Verifying a token

Token verification is fairly trivial, except that it is an asynchronous function requiring a callback:

```js
jwt.verify(token, secret, (err, payload) => {
  if(err) console.log(err) // token is invalid
  console.log(token) // { sub: 1526 }
})
```

We can turn this into a promise using a promise library like Bluebird:

```js
new Promise((resolve, reject) => {
  jwt.verify(token, secret, (err, payload) => {
    if(err) return reject(err) // token is invalid
    return resolve(token) // { sub: 1526 }
  })
})
  .then(payload => console.log(payload)) // { sub: 1526 }
  .catch(err => console.log(err)) // token is invalid
```

## Adding JWT to a login route

To incorporate a JWT into the authentication flow of our APIs, we can create a JWT in our login (and register) controller and send it to the client when they successfully authenticate:

```js
const User = require('../models/user')
const jwt = require('jsonwebtoken')
const { secret } = require('../config/environment')

function login(req, res, next) {
  User.findOne({ email: req.body.email })
    .then(user => {
      if(!user || !user.validatePassword(req.body.password)) {
        return res.status(401).json({ message: 'Unauthorized' })
      }

      const token = jwt.sign({ sub: user._id }, secret, { expiresIn: '24h' })
      res.json({ user, token, message: `Welcome back ${user.username}` })
    })
    .catch(next)
}
```

## Checking for a token

In order check for a token when the client makes a request, we need to verify the token in our `secureRoute`:

```js
const Promise = require('bluebird')
const jwt = require('jsonwebtoken')
const { secret } = require('../config/environment')
const User = require('../models/user')

function secureRoute(req, res, next) {
  // if there is no Authorization header, respond with 401 Unauthorized
  if(!req.headers.authorization) return res.status(401).json({ message: 'Unauthorized' })

  // get the token out of the Authorization header
  const token = req.headers.authorization.replace('Bearer ', '')

  // create a new promise to verify the token
  new Promise((resolve, reject) => {
    jwt.verify(token, secret, (err, payload) => {
      if(err) return reject(err)
      return resolve(payload)
    })
  })
      .then(payload => User.findById(payload.sub)) // find the user by the user ID in the payload
      .then(user => {
        // if the user can't be found, respond with 401 Unauthorized
        if(!user) return res.status(401).json({ message: 'Unauthorized' })

        // add the user to the `req` object for use in the controllers
        req.currentUser = user

        // go to the destination controller action
        next()
      })
      .catch(next)
}

module.exports = secureRoute
```

## Testing

Now that the authentication flow has been completed it we can test that everything is working using Insomnia.

Make a request to a secured route, you should receive a 401 response:

![](https://user-images.githubusercontent.com/3531085/37476299-af82b422-286c-11e8-9f37-fdc66c782028.png)

Now login, you should receive a token in the response:

![](https://user-images.githubusercontent.com/3531085/37476179-6921ea98-286c-11e8-9ad9-cdedb4c94c9b.png)

In the **Header** tab of Insomnia add the Authorization header with the token you received in the previous response:

![](https://user-images.githubusercontent.com/3531085/37476683-b0d6da00-286d-11e8-928a-a87bcedc1c02.png)

You should now be able to make an authenticated request:

![](https://user-images.githubusercontent.com/3531085/37476685-b2ec1116-286d-11e8-9b91-2120b932a26f.png)

## Further reading

- [JSON Web Tokens](https://jwt.io/)
- [Cookies vs Tokens: The Definitive Guide](https://auth0.com/blog/cookies-vs-tokens-definitive-guide/)
- [jsonwebtoken Docs](https://github.com/auth0/node-jsonwebtoken)
