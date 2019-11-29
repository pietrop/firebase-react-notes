# Getting full path

The proper way to get a documents' path:

**From** [**DocumentReference**](https://firebase.google.com/docs/reference/js/firebase.firestore.DocumentReference.html)**:** .path

> e.g. `var path = ref.path`

**From** [**DocumentSnapshot**](https://firebase.google.com/docs/reference/js/firebase.firestore.DocumentSnapshot.html)**:** .ref.path

> e.g. `var path = doc.ref.path`

