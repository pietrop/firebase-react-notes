# Firebase function local dev react

There's a way to handle [CORS requests](https://cloud.google.com/functions/docs/writing/http#handling_cors_requests) in firebase cloud function, but for local development it makes mrore sense to setup your Create React App to be able to handle the functions locally as follows.

```javascript
firebase.functions().useFunctionsEmulator('http://localhost:4001')
```

From [How to test `functions.https.onCall` firebase cloud functions locally?](https://stackoverflow.com/questions/50884534/how-to-test-functions-https-oncall-firebase-cloud-functions-locally)

in context

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
// https://stackoverflow.com/questions/50884534/how-to-test-functions-https-oncall-firebase-cloud-functions-locally
firebase.functions().useFunctionsEmulator('http://localhost:4001') 
export default firebase;
```

[from firebase API reference](https://firebase.google.com/docs/reference/android/com/google/firebase/functions/FirebaseFunctions)

