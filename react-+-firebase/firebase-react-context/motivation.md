# Motivation

React's Context management is great for keeping a Firebase Singleton for your application. By also setting up a Context specifically for Sessions, you can make sure that certain components are only accessible via successful authentication.

If you are initialising firebase like this 

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

Using these Create React App Env in a `.env` \(added to `.gitignore`\)

```text
# Firebase
REACT_APP_API_KEY=""
REACT_APP_AUTH_DOMAIN=""
REACT_APP_DATABASE_URL=""
REACT_APP_PROJECT_ID=""
REACT_APP_STORAGE_BUCKET=""
REACT_APP_MESSAGING_SENDER_ID=""
```

Or

```text
REACT_APP_API_KEY=""
REACT_APP_AUTH_DOMAIN=""
REACT_APP_DATABASE_URL=""
REACT_APP_PROJECT_ID=""
REACT_APP_STORAGE_BUCKET=""
REACT_APP_MESSAGING_SENDER_ID=""
REACT_APP_APP_ID=""
REACT_APP_MEASUREMENT_ID=""
```

  
and importing it in your React app side code  


```javascript
...
import firebase, { db } from "../../Firebase.js";
...
```

  
you might be getting this warning message in the console.

{% hint style="warning" %}
`@firebase/app:` `Warning: Firebase is already defined in the global scope. Please make sure Firebase library is only loaded once.`
{% endhint %}

There's a better implementation as shown in the next section, using React context and higher order components.

