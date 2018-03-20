# oAuth with Satellizer

oAuth is a type of authentication which uses a third-party to verify a user's credentials. For example a "Log in with Facebook" button, allows a user to sign up or login to a website without providing user information. The website allows the user access based on the principal that Facebook will verify they are who they say they are.

This suits Facebook because they become more useful to their user, and they increase their brand recognition. It also suits the website in question because they can access the user's information stored by Facebook, plus a user is more likely to register because the process is simple and convenient.

For us though, the process is quite complex and requires three steps:

1. Create an oAuth app with the oAuth provider (Github, Facebook, Twitter etc)
2. Create a login button on the client which connects to the provider
3. Communicate with the oAuth provider on the server-side to access the user's data

For this walkthrough, we'll use Github, since they have good documentation, and all WDI students _should_ have a Github account.

## Step 1: Creating an oAuth app

Navigate to: `https://github.com/settings/developers`. And click on _Register a new application_ in the top right-hand corner.

![creating-github-oauth-app](https://cloud.githubusercontent.com/assets/3531085/23410560/927e7e2c-fdc7-11e6-8b2c-fddc3d1f064a.png)

Fill out the form. All fields are required, except the app's description.

The application name can be anything, but it will be displayed to the user in _Step 1_ of the authentication flow.

The _Homepage URL_ should be your site's homepage, use `http://localhost:8000` for now.

The _Authorization callback URL_ is the url that Github will send its requests to. Set it to `http://localhost:8000`.

Once you submit the form, you'll be taken to your app's settings page:

![oauth-app-settings](https://cloud.githubusercontent.com/assets/3531085/23410561/927e78b4-fdc7-11e6-98a6-2b9b8f8e122e.png)

Here you'll see (amongst other things) a **Client ID** and a **Client Secret**. We'll need these later.

## Step 2: Creating a Login with Github button

Luckily for us this step is relatively trivial with Satellizer. We just need to update our `src/config/auth.js` file with the following:

```js
$authProvider.github({
  clientId: '*************',
  url: '/api/github'
});
```

>**Note**: replace the asterisks with the Client ID given to you by Github

Now inside your login controller add the following:

```js
function authenticate(provider) {
  $auth.authenticate(provider)
    .then(() => $state.go('home'));
}

this.authenticate = authenticate;
```

Then finally in the login template create the button like so:

```html
<button type="button" ng-click="authLogin.authenticate('github')">Log in with Github</button>
```

Clicking on the button should open a tab or popup window in your browser that looks something like this:

![confirmation-screen](https://cloud.githubusercontent.com/assets/3531085/23410562/927e8e58-fdc7-11e6-99db-37ad4852d6c8.png)

## Step 3: The server-side

Once the user confirms that they wish to sign in with Github, an _oAuth handshake_ now happens between our server and Github's servers which looks something like this:

![github-oauth-diagram](https://user-images.githubusercontent.com/3531085/37651669-c63f4d7a-2c30-11e8-8428-68f4cacc64ae.png)

When the client receives the unique code from Github, Satellizer sends it to the url that we set up in the `src/config/auth.js` file (ie `/api/github`). Let's set that up now:

Create an oauth controller on the server side: `controllers/oauth.js`, and  add the following code:

```js
function github(req, res, next) {
  console.log(req.body);
  res.sendStatus(200);
}

module.exports = { github };
```

Now hook it up to the router:

```js
const oauth = require('../controllers/oauth');

router.route('/github')
  .post(oauth.github);
```

Now if you click on the Login with Github button on the login page, you should see the following in the terminal:

```
{ code: 'fd5a3a6a404c98c977bb',
  clientId: 'a4d7dff539facb662d03',
  redirectUri: 'http://localhost:8000' }
```

This is the one time `code` sent from Github's servers.

### Requesting an access token

Inside the `github` controller action, we now need to send that back to Github, with out client ID and client secret and exchange it for an access token which we can then use to get the user's details:

```js
function github(req, res, next) {

  rp({
    method: 'GET',
    url: 'https://github.com/login/oauth/access_token',
    qs: {
      client_id: req.body.clientId,
      client_secret: process.env.GITHUB_APP_SECRET,
      code: req.body.code
    },
    json: true
  })
    .then(token => {
      console.log(token);
      res.sendStatus(200);
    })
    .catch(next);
}
```

Notice we're using an environment variable here `GITHUB_APP_SECRET`. This is the Client Secret provided by Github when we created our oAuth app. It should be stored in you `.zshrc` file like so:

```sh
export GITHUB_APP_SECRET="***********"
```

>**Note**: Don't forget to reload all open tabs when you update your `.zshrc` files!

Clicking on the Login with Github button now, you should see the following in the terminal:

```
{ access_token: 'a1e7708207354341a66437b88f162a3db6b9e474',
  token_type: 'bearer',
  scope: 'user:email' }
```

That's the access token from Github. We can use this to get the user's details.

### Requesting the user's details

We can now request the users details:

```js
function github(req, res, next) {
  .
  .
  .then(token => {
    return rp({
      method: 'GET',
      url: 'https://api.github.com/user',
      qs: token,
      headers: { // Github requires us to set a User-Agent header
        'User-Agent': 'mickyginger' // replace with YOUR username!
      },
      json: true
    });
  })
  .then(profile => {
    console.log(profile);
    res.send();
  })
  .catch(next);
}
```

>**Note**: Github requires a User-Agent header to be set when you request user details. This should be set to your app's name, or your Github username

Clicking on the Login with Github button, you should now see something like this in your terminal:

```
{ login: 'mickyginger',
  id: 3531085,
  avatar_url: 'https://avatars0.githubusercontent.com/u/3531085?v=4',
  gravatar_id: '',
  url: 'https://api.github.com/users/mickyginger',
  html_url: 'https://github.com/mickyginger',
  followers_url: 'https://api.github.com/users/mickyginger/followers',
  following_url: 'https://api.github.com/users/mickyginger/following{/other_user}',
  gists_url: 'https://api.github.com/users/mickyginger/gists{/gist_id}',
  starred_url: 'https://api.github.com/users/mickyginger/starred{/owner}{/repo}',
  subscriptions_url: 'https://api.github.com/users/mickyginger/subscriptions',
  organizations_url: 'https://api.github.com/users/mickyginger/orgs',
  repos_url: 'https://api.github.com/users/mickyginger/repos',
  events_url: 'https://api.github.com/users/mickyginger/events{/privacy}',
  received_events_url: 'https://api.github.com/users/mickyginger/received_events',
  type: 'User',
  site_admin: false,
  name: 'Mike Hayden',
  company: null,
  blog: 'https://github.com/mickyginger',
  location: 'London, UK',
  email: 'mickyginger@gmail.com',
  hireable: null,
  bio: null,
  public_repos: 42,
  public_gists: 1,
  followers: 60,
  following: 1,
  created_at: '2013-02-11T10:17:36Z',
  updated_at: '2018-01-22T15:33:11Z' }
```

That's the details of the user who just logged in.

### Saving the user's details

If this is the first time the user has logged in, we need to create a user record in our database. However, if they have logged in previously we just need to retrieve their details so that we can generate a JWT.

```js
function github(req, res, next) {
  .
  .
  .then(profile => {
    // attempt to find a user by EITHER email OR githubId
    return User
      .findOne({ $or: [{ email: profile.email }, { githubId: profile.id }] })
      .then(user => {
        // if we can't find a user, create one using the github username
        if(!user) {
          user = new User({ username: profile.login });
          // if the user has an email address, add it to the record
          if(profile.email) user.email = profile.email;
        }

        // set the githubId in either case
        user.githubId = profile.id;
        // save the user
        return user.save();
      });
  })
  .then(user => console.log(user))
  .catch(next);
}
```

Firstly we need to see if the user who's logging in already exists in our database. We can search by githubId or email. If the user can't be found, we need to create one, using the Github username (stored as `login` in their database).

If the user has made their email address public, we can also save that.

Finally we set the githubId to the user record, and save the user.

### Updating the user model

We now need to update the user model to accept the `githubId` and also allow a user to login without a password, but _only if they are using oauth_:

```js
const userSchema = new mongoose.Schema({
  username: { type: String, unique: true, required: true },
  email: { type: String, unique: true, required: true },
  password: { type: String },
  githubId: { type: String, unique: true }
});

// passwordConfirmation virtual

userSchema.pre('validate', function checkPasswordMatch(next) {
  if(!this.password && !this.githubId) {
    this.invalidate('password', 'password is required');
  }
  if(this.password && this.isModified('password') && this._passwordConfirmation !== this.password) {
    this.invalidate('passwordConfirmation', 'passwords do not match');
  }
  next();
});

// pre save hook

// validatePassword method

module.exports = mongoose.model('User', userSchema);
```

We've added the `githubId` property in the model, and updated the pre validate hook to check for either a password or a githubId. Notice we've also removed the `required` validation on the password in the model as well.

Clicking on Login with Github now, you should see a user in the terminal:

```
{ username: 'mickyginger',
  _id: 5ab113b3840b9312efe4ba3e,
  email: 'mickyginger@gmail.com',
  githubId: '3531085',
  __v: 0 }
```

### Sending back a token

Finally we just need to create a token and send it back to the client:

```js
function github(req, res, next) {
  .
  .
  .
  .then(user => {
    const token = jwt.sign({ sub: user._id }, secret, { expiresIn: '6h' });
    res.json({
      message: `Welcome back ${user.username}!`,
      token
    });
  })
  .catch(next);
}
```

The process is now complete. You can now login with Github! 💥

## Logging in with other providers

The login flow for any oAuth provider is the same, however the specific endpoints will be different. Also sometimes you need to send `GET` requests with a query string, sometimes you need to send `POST` requests with data in the body.

The only way to know is to consult the oAuth provider's documentation.

## Further reading

- [Satellizer Docs](https://github.com/sahat/satellizer)
- [Github oAuth Docs](https://developer.github.com/apps/building-oauth-apps/authorization-options-for-oauth-apps/)
