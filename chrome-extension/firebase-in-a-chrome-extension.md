---
description: Adding firebase/firestore to chrome extension
---

# Firebase in a chrome extension

based on stackoverflow [Add Cloud firestore library to chrome extension, content\_scripts](https://stackoverflow.com/questions/49087348/add-cloud-firestore-library-to-chrome-extension-content-scripts)

in chrome extension manifest file `manifest.json` add `firebase-app.js` \(`https://www.gstatic.com/firebasejs/7.5.0/firebase-app.js`\) and `firebase-firestore.js`\(`https://www.gstatic.com/firebasejs/7.5.0/firebase-firestore.js`\) as follows

```javascript
...
  "content_scripts": [
    {
      "matches": [
        "https://www.wsj.com/*"
      ],
      "js": [
        "firebasejs/7.5.0/firebase-app.js",
        "firebasejs/7.5.0/firebase-firestore.js",
    ...
```

As well as 

```javascript
  ...
  "content_security_policy":"script-src 'self' https://www.gstatic.com/ https://*.firebaseio.com https://www.googleapis.com; object-src 'self'"
}
```

then in your extension code you can just initialise firebase as usual eg

```javascript
const firebaseConfig = {
    apiKey: "XXX",
    authDomain: "XXX",
    databaseURL: "XXX",
    projectId: "XXX",
    storageBucket: "XXX",
    messagingSenderId: "XXX",
    appId: "XXX",
    measurementId: "XXX"
  };

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// do something with firebase/firestore
```



