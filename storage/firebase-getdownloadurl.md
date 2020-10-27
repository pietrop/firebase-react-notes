# Firebase \`getDownloadURL\`

```javascript
const storage = firebase.storage();
const pathReference = storage.ref('audio/interview.wav');
// TODO: add error handling
const url = await pathReference.getDownloadURL();
```

from [Download Files on Web](https://firebase.google.com/docs/storage/web/download-files)

When do the `getDownloadURL` expire?

> **Firebase Storage tokens do not expire.**
>
> They may be revoked from the Firebase Console, which would invalidate URLs based on them.

from [Firebase Storage getDownloadUrl's token validity](https://stackoverflow.com/questions/42593002/firebase-storage-getdownloadurls-token-validity)

