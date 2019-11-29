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

{% embed url="https://firebase.google.com/docs/storage/security/start" %}



