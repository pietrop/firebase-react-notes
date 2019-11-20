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

#### Firestore - read

```javascript
db.collection("transcripts").get().then((querySnapshot) => {
  querySnapshot.forEach((doc) => {
      console.log(`${doc.id} => ${JSON.stringify(doc.data())}`);
  });
});
```

