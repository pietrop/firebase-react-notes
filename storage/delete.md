---
description: Firestore trigger to delete file cloud storage
---

# Delete

```javascript
const admin = require("firebase-admin");
admin.initializeApp();
...
exports.deleteTranscriptMedia = functions.firestore
  .document("transcripts/{transcriptId}")
  .onDelete(async (snap, context) => {
    const defaultStorage = admin.storage();
    const deletedValue = snap.data();
    const storageRefPath = deletedValue.storageRef;
    const bucket = defaultStorage.bucket();
    // const file = bucket.file('profilePicture/' + userID + '.png');
    const file = bucket.file(storageRefPath);
    // Delete the file
    return file.delete()
  });
```

[Automatically delete your Firebase Storage Files from Firestore with Cloud Functions for Firebase](https://medium.com/google-developer-experts/automatically-delete-your-firebase-storage-files-from-firestore-with-cloud-functions-for-firebase-36542c39ba0d)

