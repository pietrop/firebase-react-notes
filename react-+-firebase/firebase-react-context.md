# Firebase React context

if you are importing firebase in every file, like this 

```javascript
import firebase from "firebase/app";
import "firebase/firestore";
import "firebase/auth";
import "firebase/storage";
import "firebase/functions";

const firebaseConfig = {
  apiKey: process.env.REACT_APP_API_KEY,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN,
  databaseURL: process.env.REACT_APP_DATABASE_URL,
  projectId: process.env.REACT_APP_PROJECT_ID,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID,
  appId: process.env.REACT_APP_APP_ID,
  measurementId: process.env.REACT_APP_MEASUREMENT_ID
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);

export const db = firebase.firestore();
export const functions = firebase.functions();

firebase.functions().useFunctionsEmulator('http://localhost:4001') 
export default firebase;

```

you might be getting this issue.

```text
@firebase/app: 
    Warning: Firebase is already defined in the global scope. Please make sure
    Firebase library is only loaded once.
```

There's a better implementation as shown in DPE, using React context and higher order components.

### React context

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';
import Firebase, { FirebaseContext } from './Components/Firebase';

ReactDOM.render(
  <FirebaseContext.Provider value={ new Firebase() }>
    <App />
  </FirebaseContext.Provider>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

[https://github.com/bbc/digital-paper-edit-client/blob/firebase/src/Components/Firebase/context.js](https://github.com/bbc/digital-paper-edit-client/blob/firebase/src/Components/Firebase/context.js)

### Firebase class

```javascript
import app from 'firebase/app';
import 'firebase/auth';
import 'firebase/firestore';
​
const config = {
  apiKey: process.env.REACT_APP_API_KEY,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN,
  databaseURL: process.env.REACT_APP_DATABASE_URL,
  projectId: process.env.REACT_APP_PROJECT_ID,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID
};
class Firebase {
  constructor() {
    app.initializeApp(config);
    this.auth = app.auth();
    this.db = app.firestore();
  }
​
  userRef = uid => this.db.doc(`users/${ uid }`);
  usersRef = () => this.db.collection('users');
​
  user = async uid => await this.userRef(uid).get();
  users = async () => await this.usersRef().get();
  // *** Merge Auth and DB User API *** //
​
  onAuthUserListener = (next, fallback) =>
    this.auth.onAuthStateChanged(async authUser => {
      if (authUser) {
        const dbSnapshot = await this.user(authUser.uid);
        const dbUser = dbSnapshot.data();
​
        // default empty roles
        if (!dbUser.roles) {
          dbUser.roles = {};
        }
​
        // merge auth and db user
        const mergeUser = {
          uid: authUser.uid,
          email: authUser.email,
          emailVerified: authUser.emailVerified,
          providerData: authUser.providerData,
          ...dbUser
        };
​
        next(mergeUser);
      } else {
        fallback();
      }
    });
​
  // doCreateUserWithEmailAndPassword = (email, password) =>
  // this.auth.createUserWithEmailAndPassword(email, password);
  doSignInWithEmailAndPassword = (email, password) =>
    this.auth.signInWithEmailAndPassword(email, password);
  doSignOut = () => this.auth.signOut();
  doPasswordReset = email => this.auth.sendPasswordResetEmail(email);
  doPasswordUpdate = password => this.auth.currentUser.updatePassword(password);
}
​
export default Firebase;
```

[https://github.com/bbc/digital-paper-edit-client/blob/firebase/src/index.js](https://github.com/bbc/digital-paper-edit-client/blob/firebase/src/index.js)

### Higher order component - consumer

```javascript
import React from 'react';

const FirebaseContext = React.createContext(null);

export const withFirebase = Component => props => (
  <FirebaseContext.Consumer>
    {firebase => <Component { ...props } firebase={ firebase } />}
  </FirebaseContext.Consumer>
);
export default FirebaseContext;
```

### Example usage

eg in a sign in page

[https://github.com/bbc/digital-paper-edit-client/blob/firebase/src/Components/SignIn/index.js](https://github.com/bbc/digital-paper-edit-client/blob/firebase/src/Components/SignIn/index.js)

```javascript
import React, { useState } from 'react';
import { withRouter } from 'react-router-dom';
import { compose } from 'recompose';
import { withFirebase } from '../Firebase';
import * as ROUTES from '../../constants/routes';
const SignInPage = () => (
  <div>
    <h1>SignIn</h1>
    <SignInForm />
  </div>
);

const SignInFormBase = props => {
  const [ email, setEmail ] = useState('');
  const [ password, setPassword ] = useState('');
  const [ error, setError ] = useState();

  // TODO: IF signed in, forward always to somewhere else
  const onSubmit = async event => {
    try {
      await props.firebase.doSignInWithEmailAndPassword(email, password);
      setEmail('');
      setPassword('');
      setError(null);
      props.history.push(ROUTES.PROJECTS);
    } catch (err) {
      setError(err);
    }
    event.preventDefault();
  };

  const onChange = event => {
    console.log(event.target);
    if (event.target.name === 'password') {
      setPassword(event.target.value);
    } else {
      setEmail(event.target.value);
    }
  };

  const isInvalid = password === '' || email === '';

  return (
    <form onSubmit={ onSubmit }>
      <input
        name="email"
        value={ email }
        onChange={ onChange }
        type="text"
        placeholder="Email Address"
      />
      <input
        name="password"
        value={ password }
        onChange={ onChange }
        type="password"
        placeholder="Password"
      />
      <button disabled={ isInvalid } type="submit">
        Sign In
      </button>

      {error && <p>{error.message}</p>}
    </form>
  );
};

const SignInForm = compose(withRouter, withFirebase)(SignInFormBase);

export default SignInPage;

export { SignInForm };
```

