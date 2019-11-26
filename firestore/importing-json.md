---
description: 'Firebase: Import JSON To Cloud Firestore'
---

# Importing json

{% hint style="info" %}
This can be useful for seeding a firebase project, and or restore a backup.
{% endhint %}

from [Firebase: Import JSON To Cloud Firestore](https://levelup.gitconnected.com/firebase-import-json-to-firestore-ed6a4adc2b57), abridge below

get `users.json`

```text
{
  "users": [
    {
      "id": "1",
      "firstName": "Kristin",
      "lastName": "Smith",
      "occupation": "Teacher",
      "reviewCount": "6",
      "reviewScore": "5",
    },
    {
      "id": "2",
      "firstName": "Olivia",
      "lastName": "Parker",
      "occupation": "Teacher",
      "reviewCount": "11",
      "reviewScore": "5"
    },
    {
      "id": "3",
      "firstName": "Jimmy",
      "lastName": "Robinson",
      "occupation": "Teacher",
      "reviewCount": "9",
      "reviewScore": "4"
    },
    {
      "id": "4",
      "firstName": "Zack",
      "lastName": "Carter",
      "occupation": "Teacher",
      "reviewCount": "4",
      "reviewScore": "5"
    },
    {
      "id": "5",
      "firstName": "Brad",
      "lastName": "Rayburn",
      "occupation": "Teacher",
      "reviewCount": "2",
      "reviewScore": "4"
    }
  ]
}
```

\[...\]

module `firestore-export-import`

```javascript
// Imports
const firestoreService = require('firestore-export-import');
const firebaseConfig = require('./config.js');
const serviceAccount = require('./serviceAccount.json');

// JSON To Firestore
const jsonToFirestore = async () => {
  try {
    console.log('Initialzing Firebase');
    await firestoreService.initializeApp(serviceAccount, firebaseConfig.databaseURL);
    console.log('Firebase Initialized');

    await firestoreService.restore('./data-clean/firebase/users.json');
    console.log('Upload Success');
  }
  catch (error) {
    console.log(error);
  }
};

jsonToFirestore();
```

