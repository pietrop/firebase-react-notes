# Firestore write

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

