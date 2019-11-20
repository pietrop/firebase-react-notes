# Create

[https://firebase.google.com/docs/storage/web/upload-files](https://firebase.google.com/docs/storage/web/upload-files)

On React client 

```javascript
 const handleUpload = () => {
    const selectedFile = selectedFiles[0];

    // Create a root reference
    const storageRef = firebase.storage().ref();
    // Create a reference to 'mountains.jpg'
    const fileRef = storageRef.child('test.txt');
   const uploadTask = fileRef.put(selectedFile).then(function(snapshot) {
      // TODO: add some kind of alert confirming upload of file
      // TODO: when choosing file, have a upload option?
      console.log('Uploaded a blob or file!', snapshot);
    });
    // uploaded file
    setShowNotice(true);
  };
```

### With progress bar upload indication 

```javascript
 const handleUpload = () => {
    const selectedFile = selectedFiles[0];

    // Create a root reference
    const storageRef = firebase.storage().ref();
    // Create a reference to 'mountains.jpg'
    const fileRef = storageRef.child('test.txt');
   const uploadTask = fileRef.put(selectedFile).then(function(snapshot) {
      // TODO: add some kind of alert confirming upload of file
      // TODO: when choosing file, have a upload option?
      console.log('Uploaded a blob or file!', snapshot);
    });
    // https://firebase.google.com/docs/storage/web/upload-files
    // Listen for state changes, errors, and completion of the upload.
    uploadTask.on(
      firebase.storage.TaskEvent.STATE_CHANGED, // or 'state_changed'
      function(snapshot) {
        setIsUploading(true);
        // Get task progress, including the number of bytes uploaded and the total number of bytes to be uploaded
        var progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
        console.log("Upload is " + progress + "% done");
        // TODO: function to update progress bar using percentage
        setUploadProgress(progress);
        switch (snapshot.state) {
          case firebase.storage.TaskState.PAUSED: // or 'paused'
            console.log("Upload is paused");
            break;
          case firebase.storage.TaskState.RUNNING: // or 'running'
            console.log("Upload is running");
            break;
          default:
            console.error('Error with upload to firebase');
            break;
        }
      },
      // error handling
      function(error) {
        setIsUploading(false);
        // A full list of error codes is available at
        // https://firebase.google.com/docs/storage/web/handle-errors
        switch (error.code) {
          case "storage/unauthorized":
            // User doesn't have permission to access the object
            console.error(error.code);
            break;
          case "storage/canceled":
            // User canceled the upload
            console.error(error.code);
            break;
          case "storage/unknown":
            // Unknown error occurred, inspect error.serverResponse
            console.error(error.code);
            break;
          default:
            console.error('Error with upload to firebase', error);
            break;
        }
      },
      // upload completed 
      function() {
        // Upload completed successfully, now we can get the download URL
        uploadTask.snapshot.ref.getDownloadURL().then(function(downloadURL) {
          // upload file
          setShowNotice(true);
          console.log("File available at", downloadURL);
          setDownloadURL(downloadURL);
        });
      }
    );

    // uploaded file
    setShowNotice(true);
  };
```

[How does Firebase associate uploaded files with a specific user?](https://www.quora.com/How-does-Firebase-associate-uploaded-files-with-a-specific-user)

[Create a reference to a Cloud Storage document in Firestore](https://stackoverflow.com/questions/47886777/create-a-reference-to-a-cloud-storage-document-in-firestore)

TODO: add logic to do progress bar of file



