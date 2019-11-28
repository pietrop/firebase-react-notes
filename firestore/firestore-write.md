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

## Indexing

[https://firebase.google.com/docs/firestore/query-data/index-overview\#indexing\_best\_practices](https://firebase.google.com/docs/firestore/query-data/index-overview#indexing_best_practices)

> Large string fields  
> If you have a string field that often holds long string values that you don’t use for querying, you can cut storage costs by exempting the field from indexing.



