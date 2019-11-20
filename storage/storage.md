# Storage

[https://firebase.google.com/docs/storage/web/upload-files](https://firebase.google.com/docs/storage/web/upload-files)

```javascript
 const handleUpload = () => {
    const selectedFile = selectedFiles[0];

    // Create a root reference
    const storageRef = firebase.storage().ref();
    // Create a reference to 'mountains.jpg'
    const fileRef = storageRef.child('test.txt');
    fileRef.put(selectedFile).then(function(snapshot) {
      // TODO: add some kind of alert confirming upload of file
      // TODO: when choosing file, have a upload option?
      console.log('Uploaded a blob or file!', snapshot);
    });

    // upload file
    setShowNotice(true);
  };
```

[How does Firebase associate uploaded files with a specific user?](https://www.quora.com/How-does-Firebase-associate-uploaded-files-with-a-specific-user)

[Create a reference to a Cloud Storage document in Firestore](https://stackoverflow.com/questions/47886777/create-a-reference-to-a-cloud-storage-document-in-firestore)

### [Automatically delete your Firebase Storage Files from Firestore with Cloud Functions for Firebase](https://medium.com/google-developer-experts/automatically-delete-your-firebase-storage-files-from-firestore-with-cloud-functions-for-firebase-36542c39ba0d)

