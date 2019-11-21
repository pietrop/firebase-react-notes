# Firestore write

[https://firebase.google.com/docs/firestore/manage-data/add-data\#set\_a\_document](https://firebase.google.com/docs/firestore/manage-data/add-data#set_a_document)

```javascript
db.collection("transcripts").add({
  title: "new one",
  description: "new description"
})
.then(function(docRef) {
  console.log("Document written with ID: ", docRef.id);
})
.catch(function(error) {
  console.error("Error adding document: ", error);
});
```

#### 

