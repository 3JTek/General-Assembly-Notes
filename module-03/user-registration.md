# User Registration

When a user registers on a website, two things need to happen:

1. A user record is created in the database
1. Their password is _hashed_ and saved with the user record.

## Hashing vs encryption

Hashing is not the same as encryption. Encryption is a process by which data is encoded so that it cannot be read or processed unless it is first decoded. Generally a key or password is used to encrypt and decrypt the data. Once encrypted only a user with the correct key or password could decode the data.

Hashing is similar, except it is mean to be a one way process. Once hashed data _cannot_ be un-hashed. Of course a hacker could potentially un-hash a password, but it would require a huge amount of time and processing power.

On top of the hashing process we can add a little more complexity with salt. Salt is extra random data which can make it even more difficult for hackers to un-hash a password.

## Bcrypt

In order to hash a password in an Express app we will use a package called Bcrypt, which is an industry standard hashing tool. It can be installed with yarn:

```sh
yarn add bcrypt
```

Generally speaking we want to keep as much logic out of our controllers as possible, their job is simply to get data from the model and pass it to the view. Since passwords are essentially data, it is the responsibility of the model to hash the password before it is stored in the database.

To do that we can use a _lifecycle_ hook.

## Lifecycle hooks

A lifecycle hook, it a function that can be called at specific points during the process of saving a retrieving data from a database. Some typical Mongoose lifecycle hooks are:

- pre-validate
- post-validate
- pre-save
- post-save

When a user registers to our site, we want them to provide a password, and a password confirmation. We then need to make sure the password and password confirmation match. This will be done in a pre-validate hook. If the passwords match, we will hash the password before saving it in the database. This will be done in a pre-save hook.

## Virtuals

If we want to receive data in our lifecycle hooks that we don't actually want to store in the database, like the password confirmation, we can use a Mongoose virtual. This behaves like a normal model property (like name, email, etc) but will not be added to the database.

## Putting it all together

Here's what a typical user model might look like, with pre-validate and pre-save hooks in place:

```js
const mongoose = require('mongoose')
const bcrypt = require('bcrypt')

const schema = new mongoose.Schema({
  username: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }
})

// set up the passwordConfirmation virtual
schema
  .virtual('passwordConfirmation')
  .set(function setPasswordConfirmation(passwordConfirmation) {
    // store the password on the user model temporarily so we can access it in our pre-validate hook
    // `this` refers to the user object
    this._passwordConfirmation = passwordConfirmation
  })

// set up a pre-validate hook
schema.pre('validate', function checkPassword(next) {
  // check if the password has been modified and if so whether the password and the passwordConfirmation match
  // if not invalidate the passwordConfirmation, so that the validations fail
  if(this.isModified('password') && this._passwordConfirmation !== this.password) this.invalidate('passwordConfirmation', 'does not match')

  // otherwise continue to the next step (validation)
  next()
})

schema.pre('save', function hashPassword(next) {
  // if the password has been modified, it needs to be hashed
  if(this.isModified('password')) {
    // hash the password with bcrypt and store the hashed password on the user object
    this.password = bcrypt.hashSync(this.password, bcrypt.genSaltSync(8))
  }

  // continue to the next step (save)
  next()
})

module.exports = mongoose.model('User', userSchema)
```

## Registering a user

All that's left to do is create a form and a controller which will handle the user's data:

```js
const User = require('../models/user')

function register(req, res) {
  User
    .create(req.body)
    .then(() => res.json({ message: 'Registration successful'}))
    .catch(next)
}

module.exports = {
  register
}
```

Since all the work is done by the model, the controller is fairly standard. Once a user registers their password will be hashed and stored in the database.

## Further reading

- [Why You Should use BCrypt to Hash Passwords](https://medium.com/@danboterhoven/why-you-should-use-bcrypt-to-hash-passwords-af330100b861)
- [What's the Difference Between Hashing and Encryption?](https://www.securityinnovationeurope.com/blog/page/whats-the-difference-between-hashing-and-encrypting)
- [Why You Should Always Salt Your Hashes](https://www.addedbytes.com/blog/why-you-should-always-salt-your-hashes)
