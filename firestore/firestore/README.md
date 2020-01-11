# Firestore

[  
https://firebase.google.com/docs/firestore/quickstart](
https://firebase.google.com/docs/firestore/quickstart)

[https://medium.com/get-it-working/get-googles-firestore-working-with-react-c78f198d2364](https://medium.com/get-it-working/get-googles-firestore-working-with-react-c78f198d2364)

[https://firebase.google.com/docs/firestore/security/get-started](https://firebase.google.com/docs/firestore/security/get-started)

```javascript
// Allow read/write access on all documents to any user signed in to the application
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth.uid != null;
    }
  }
}
```

`allow read, write: if request.auth.uid != null;`

```javascript
service cloud.firestore {
  match /databases/{database}/documents {
    // Make sure the uid of the requesting user matches name of the user
    // document. The wildcard expression {userId} makes the userId variable
    // available in rules.
    match /users/{userId} {
      allow read, update, delete: if request.auth.uid == userId;
      allow create: if request.auth.uid != null;
    }
  }
}
```

[https://firebase.google.com/docs/firestore/security/rules-conditions](https://firebase.google.com/docs/firestore/security/rules-conditions)

[https://firebase.google.com/docs/firestore/security/rules-query](https://firebase.google.com/docs/firestore/security/rules-query)

