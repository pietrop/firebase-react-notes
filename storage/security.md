# Security

## NoSQL Schema

Recommended schema is having the userId somewhere in the file path, and updating the Security Rules like so:

```javascript
// Grants a user access to a node matching their user ID
service firebase.storage {
  match /b/{bucket}/o {
    // Files look like: "user/<UID>/path/to/file.txt"
    match /user/{userId}/{allPaths=**} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
```

## Sub-collections

If you use something like `{allPaths=**}`, this also includes subcollections. If you have set up conditionals based on the `response` object, you will need to ensure that the condition exists at every level. For example:

```text
// Grants a user access to a node matching their user ID
service firebase.firestore {
  match /document/{database}/tables {
    match /user/{userId}/{allPaths=**} {
      allow read, write: if response.data.role == "ADMIN";
    }
  }
}
```

This will expect even subcollections that matched with `{allPaths=**}` to have a field `role`. In order to get around this you want to set a full path:

```text
// Grants a user access to a node matching their user ID
service firebase.firestore {
  match /document/{database}/tables {
    match /user/{userId}/{allPaths=**} {
      allow read, write: if get(/document/$(database)/tables/user/$(request.auth.uid)).data.role == "ADMIN";
    }
  }
}
```



[https://medium.com/@khreniak/cloud-firestore-security-rules-basics-fac6b6bea18e](https://medium.com/@khreniak/cloud-firestore-security-rules-basics-fac6b6bea18e)

{% embed url="https://firebase.google.com/docs/storage/security/start" %}

