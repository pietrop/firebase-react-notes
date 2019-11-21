# Removing a listener from firestorm

```javascript
let unsub = db.collection('cities').onSnapshot(() => {

});


// ...

// Stop listening for changes
unsub();
```

[https://firebase.google.com/docs/firestore/query-data/listen\#detach\_a\_listener](https://firebase.google.com/docs/firestore/query-data/listen#detach_a_listener)

