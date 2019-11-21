# Firestore - read

[https://firebase.google.com/docs/firestore/query-data/get-data](https://firebase.google.com/docs/firestore/query-data/get-data)

```javascript
db.collection("transcripts").get().then((querySnapshot) => {
  querySnapshot.forEach((doc) => {
      console.log(`${doc.id} => ${JSON.stringify(doc.data())}`);
  });
});
```

## Listen for realtime updates

[https://firebase.google.com/docs/firestore/query-data/listen](https://firebase.google.com/docs/firestore/query-data/listen)

