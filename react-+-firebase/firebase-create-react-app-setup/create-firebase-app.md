# Create firebase app

## Create your firebase application

Head on over to console.firebase.google.com and activate Firebase for your Google account. If you select the Spark plan then for moderately sized applications there is no cost.

Once you have activated firebase, go back to the terminal and login:

```bash
firebase login
```

You will be presented with a login prompt in your browser. Click Allow to complete the login

## Firebase environments

If you are just playing around then you can skip this step. When you get serious about developing a real application you will want to create multiple Firebase environments. To start let's create two firebase environments -- a development and production environment. The development environment will be the default environment that can be used for trying out new features and the production environment will the the "stable" customer facing environment.

```bash
firebase projects:create ${REACT_APP}-production
firebase projects:create ${REACT_APP}-development
```

Firebase environment names must be unique, so if the name you want is not available, try a different name.



## Initialize Firebase

Now we are ready to initialize firebase inside our create-react-app. Firebase has an interactive application for that:

```bash
firebase init
```

For now let's just pick Hosting because that is all I am going to be covering. You can always re-run this command later to add features

We'll use an existing project that we created earlier. If using multiple environments I recommend picking your development environment as the default.

Change the hosting location to `build`. Answer yes to if the application is a SPA \(single page application\).

If you going to deploy to multiple environments, then add your secondary environment now:

```bash
firebase use --alias production ${REACT-APP}-production; # or whatever name you ended up picking
firebase use default; # switch back to development
```

## Firebase implicit initialization scripts

Implicit initialization is a nifty way to initialize Firebase without config files. When supporting multiple configurations from a single project it is the only easy way to support multiple configurations.

### Environments

#### Don't do

We really **do not want to have the following code** because it is not a good idea to leak non-production environment details in a production application:

```javascript
if (process.env.REACT_APP_DEPLOY_ENV='production') {
  firebase.initializeApp(productionConfig)
} else {
  firebase.initializeApp(developmentConfig) 
}
```

#### Do

**Hosting**

Set up 2 different projects, e.g. `digital-paper-edit-<environment>` as seen above

\*\*\*\*[**https://dev.to/valentinprgnd/multiple-environments-with-firebase-hosting-1ao4**](https://dev.to/valentinprgnd/multiple-environments-with-firebase-hosting-1ao4)\*\*\*\*

**Functions**

Firebase SDK for Cloud Functions offers built-in environment configuration to make it easy to store and retrieve this type of data for your project. ****You can access them via `functions.config()`

{% embed url="https://firebase.google.com/docs/functions/config-env" %}

### Implicit Initialization \(/\_\_/\)

Implicit initialization works by loading Firebase javascript from a special `/__/` folder. We we deploy our app to Firebase this route will exist for us but locally we will need to do a little more work to set it up.

In your favorite code editor open up the build/index.html that was created by firebase init. At the top of the body tag you will see a series of Firebase related script tags. Select and copy these tags:

```markup
<!-- Insert these scripts at the bottom of the HTML, but before you use any Firebase services -->
<!-- Firebase App (the core Firebase SDK) is always required and must be listed first -->
<script src="/__/firebase/7.4.0/firebase-app.js"></script>

<!-- Add Firebase products that you want to use -->
<script src="/__/firebase/7.4.0/firebase-auth.js"></script>
<script src="/__/firebase/7.4.0/firebase-firestore.js"></script>
<script src="/__/firebase/7.4.0/firebase-functions.js"></script>
```

Paste them in your `public/index.html` file at the top of the body tag. Feel free to remove features you are not using.

`/__/firebase/js-sdk-version/firebase-sdk-name.js`

for more details see [Load Firebase SDKs from reserved URLs](https://firebase.google.com/docs/hosting/reserved-urls?authuser=0)

**Note** Don't skip this step!

## Firebase serve for local development

firebase-tools comes with a server that supports the implicit initialization, but first we need to set up a few things in create-react-app.

First let's install run-p which is a handy tool for running multiple npm scripts in parallel. In addition we will need to configure an http proxy.

```bash
npm i -D npm-run-all http-proxy-middleware
```

In your package.json change the start script from react-scripts start to:

```javascript
"start": "run-p --race dev:firebase dev:react",
"dev:firebase": "firebase serve -p 4000",
"dev:react": "react-scripts start",
```

Now create `src/setupProxy.js` file and type:

```javascript
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    '/__',
    proxy({
      target: 'http://localhost:4000'
    })
  )
}
```

Now we are ready for local development. In your terminal run:

```bash
npm start
```

If all is well you should see you react app.

Checking developer tools and terminal console you should see that Firebase javascript is loading and no errors are present. Hot reloading works so you can make changes to code and they will be reflected immediately.

