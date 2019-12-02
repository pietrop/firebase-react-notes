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

There current is roughly 40,000 limit to indexing for Firestore.

[https://firebase.google.com/docs/firestore/query-data/index-overview\#indexing\_best\_practices](https://firebase.google.com/docs/firestore/query-data/index-overview#indexing_best_practices)

> Large string fields  
> If you have a string field that often holds long string values that you don’t use for querying, you can cut storage costs by exempting the field from indexing.

{% embed url="https://firebase.google.com/docs/firestore/query-data/indexing" %}

### Remove indexes <a id="remove_indexes"></a>

To delete an index:

1. Go to the **Database** section of the [Firebase console](https://console.firebase.google.com/project/_/database/firestore/data).
2. Click the **Indexes** tab.
3. Hover over the index you want to delete and select **Delete** from the context menu.
4. Confirm that you want to delete it by clicking **Delete** from the alert.

It doesn't mention anywhere how to do this programmatically... :\( 

## Server Timestamp

{% embed url="https://firebase.google.com/docs/reference/node/firebase.firestore.FieldValue.html\#servertimestamp" %}

You need access to FieldValue, which is available from `firebase.firestore.FieldValue.serverTimestamp()`

