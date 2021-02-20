# Manual setup

Assuming you are using [Create React App](https://create-react-app.dev/) \(CRA\) and you want to setup firebase to have 3 distinct ENVs for development of a production application. 

* `dev`
* `prod` 
* `uat` \(user acceptance test\)

## 1. Create firebase projects

Create a firebase project for each ENVs from the firebase dashboard/console at [console.firebase.google.com](https://console.firebase.google.com/)

{% embed url="https://firebase.google.com/" %}

### 2. Create a web app in firebase for your ENVs

{% embed url="https://console.firebase.google.com/" %}

### 3. Turn on firebase services

For all 3 envs

1. Create firestore db
2. Enable cloud functions
3. Enable hosting
4. Turn on auth 
   1. with preferred auth method eg google/gmail

### 4. and config credentials to the project

add the Firebase config object values CRA in `.env` file, see `.env.example`

{% embed url="https://firebase.google.com/docs/web/setup" %}

```text
# Dev
REACT_APP_API_KEY_DEV=
REACT_APP_AUTH_DOMAIN_DEV=
REACT_APP_PROJECT_ID_DEV=
REACT_APP_STORAGE_BUCKET_DEV=
REACT_APP_MESSAGING_SENDER_ID_DEV=
REACT_APP_APP_ID_DEV=
REACT_APP_MEASUREMENT_ID_DEV=
# UAT
REACT_APP_API_KEY_UAT=
REACT_APP_AUTH_DOMAIN_UAT=
REACT_APP_PROJECT_ID_UAT=
REACT_APP_STORAGE_BUCKET_UAT=
REACT_APP_MESSAGING_SENDER_ID_UAT=
REACT_APP_APP_ID_UAT=
REACT_APP_MEASUREMENT_ID_UAT=
# Prod
REACT_APP_API_KEY_PROD=
REACT_APP_AUTH_DOMAIN_PROD=
REACT_APP_PROJECT_ID_PROD=
REACT_APP_STORAGE_BUCKET_PROD=
REACT_APP_MESSAGING_SENDER_ID_PROD=
REACT_APP_APP_ID_PROD=
REACT_APP_MEASUREMENT_ID_PROD=
```

In your code add some logic to switch between envs eg

```javascript
import 'firebase/analytics';
import firebase from 'firebase/app';
import 'firebase/auth';
import 'firebase/firestore';
import 'firebase/functions';
import 'firebase/storage';

const FIREBASE_CONFIG_DEV = {
  apiKey: process.env.REACT_APP_API_KEY_DEV,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN_DEV,
  databaseURL: process.env.REACT_APP_DATABASE_URL_DEV,
  projectId: process.env.REACT_APP_PROJECT_ID_DEV,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET_DEV,
  messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID_DEV,
  appId: process.env.REACT_APP_APP_ID_DEV,
  measurementId: process.env.REACT_APP_MEASUREMENT_ID_DEV,
};

const FIREBASE_CONFIG_UAT = {
  apiKey: process.env.REACT_APP_API_KEY_UAT,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN_UAT,
  projectId: process.env.REACT_APP_PROJECT_ID_UAT,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET_UAT,
  messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID_UAT,
  appId: process.env.REACT_APP_APP_ID_UAT,
  measurementId: process.env.REACT_APP_MEASUREMENT_ID_UAT,
};

const FIREBASE_CONFIG_PROD = {
  apiKey: process.env.REACT_APP_API_KEY_PROD,
  authDomain: process.env.REACT_APP_AUTH_DOMAIN_PROD,
  databaseURL: process.env.REACT_APP_DATABASE_URL_PROD,
  projectId: process.env.REACT_APP_PROJECT_ID_PROD,
  storageBucket: process.env.REACT_APP_STORAGE_BUCKET_PROD,
  messagingSenderId: process.env.REACT_APP_MESSAGING_SENDER_ID_PROD,
  appId: process.env.REACT_APP_APP_ID_PROD,
  measurementId: process.env.REACT_APP_MEASUREMENT_ID_PROD,
};

// Initialize Firebase
if (process.env.REACT_APP_NODE_ENV.toLowerCase() === 'dev') {
  firebase.initializeApp(FIREBASE_CONFIG_DEV);
} else if (process.env.REACT_APP_NODE_ENV.toLowerCase() === 'uat') {
  firebase.initializeApp(FIREBASE_CONFIG_UAT);
} else if (process.env.REACT_APP_NODE_ENV.toLowerCase() === 'prod') {
  firebase.initializeApp(FIREBASE_CONFIG_PROD);
} else {
  // firebase.initializeApp(FIREBASE_CONFIG_DEV);
  console.error('ENVs not set correctly');
}

export const db = firebase.firestore();
export const functions = firebase.functions();
export const analytics = firebase.analytics();
export const storage = firebase.storage();

// https://stackoverflow.com/questions/50884534/how-to-test-functions-https-oncall-firebase-cloud-functions-locally
// TODO: add condition that if in dev, then use emulator for functions
// firebase.functions().useFunctionsEmulator('http://localhost:4001');

export default firebase;

```

  
you can then build the CRA app in different 

* `dev`
* `prod`
* `uat`

```text
npx cross-env REACT_APP_NODE_ENV=dev npm run build
```

## Optional other services

### 5. Google Cloud task

#### 5.1 Setup a google cloud task for each env

Enable cloud task API for all 3 envs from GCP dashboard for these firebase projects

{% embed url="https://console.cloud.google.com/cloudtasks" %}

Eg the google cloud task coul be used by the an STT cloud function

#### 5.2 Create a google queue

Create a Google Queue to use with the cloud task \(for all 3 envs\)

[Create a Google Queue ](https://cloud.google.com/tasks/docs/creating-queues#creating_a_queue) \(eg `firestore-stt`\) to use Google Task within this project. This is for using [GCP STT](https://cloud.google.com/speech-to-text) with firebase cloud functions. See architecture diagram for more info.

```text
gcloud tasks queues create firestore-stt
```

If you don't have [`gcloud`](https://cloud.google.com/sdk/gcloud) you can either set it up, or use the [gcp cloud shell](https://cloud.google.com/shell), in [cloud console](https://cloud.google.com/cloud-console) for the project. 

### 6 GCP STT \(Speech To Text\)

To use GCP STT via cloud functions 

#### 6.1 Enable GCP STT API for all 3 envs

Enable GCP STT API \(for all 3 envs\) from GCP dashboard for these firebase projects

{% embed url="https://console.cloud.google.com/apis/library/speech.googleapis.com" %}

#### 6.2 Firestore API Keys in cloud function for STT

Setup Firestore API Keys in cloud function \(for all 3 envs\)

1.  Login to [Firebase Console](https://console.firebase.google.com/)
2. Select your **project**
3. **Project Settings**
4. Under the **General Tab**
5. See the **Web API key**

{% embed url="https://stackoverflow.com/questions/13863297/firebase-what-is-the-api-key" %}

Set the Firebase web API key as env var for cloud functions, to be able to use to call the GCP STT operation end point, outside of the STT SDK to check progress of a transcription.

```text
firebase functions:config:set webapi.key="THE FIREBASE WEB API KEY"
```

[more info here](https://stackoverflow.com/questions/34442739/how-does-one-set-private-environment-variables-on-firebase-hosting)  
This could be used by a cloud function to retrieve data from a STT operation eg

```javascript
exports.getSttOperationUrl = (operationName, firebaseApiKey) => {
  return `https://speech.googleapis.com/v1/operations/${operationName}?key=${firebaseApiKey}`;
};
```

## 7. deploy the app

```text
firebase login
```

```text
firebase use <env>
```

eg

```text
firebase use dev
```

Assuming you bundled the react app

```text
npx cross-env REACT_APP_NODE_ENV=dev npm run build
```

```text
firebase deploy  -P  <env>
```

eg

```text
firebase deploy  -P  dev
```

This will deploy hosting and cloud function

Successful deployment, will return the hosting url.

_TODO: This can be done with npm scripts with less steps._

```text
"scripts": {
  ...
  "deploy:all:dev": "firebase use dev && rimraf build && cross-env REACT_APP_NODE_ENV=dev npm run build && firebase deploy  -P dev",
  "deploy:all:prod": "firebase use prod && rimraf build && cross-env REACT_APP_NODE_ENV=prod npm run build && firebase deploy  -P prod",
  "deploy:all:uat": "firebase use uat && rimraf build && cross-env REACT_APP_NODE_ENV=uat npm run build && firebase deploy  -P uat",
  ...
```

## Links

* [Deploy to multiple environments with Firebase Hosting](https://firebase.googleblog.com/2016/07/deploy-to-multiple-environments-with.html)
* [Configure multiple projects](https://firebase.google.com/docs/projects/multiprojects)
* [Environment configuration](https://firebase.google.com/docs/functions/config-env)
* [Firebase CLI reference](https://firebase.google.com/docs/cli)

