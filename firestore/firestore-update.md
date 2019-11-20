# Firestore update

**set vs update**

> The way I understood the difference:
>
> * set without merge will overwrite a document or create it if it doesn't exist yet
> * set with merge will update fields in the document or create it if it doesn't exists
> * update will update fields but will fail if the document doesn't exist
> * create will create the document but fail if the document already exists
>
> There's also a difference in the kind of data you provide to set and update.
>
> For set you always have to provide document-shaped data:
>
> ```javascript
> set(
>   {a: {b: {c: true}}},
>   {merge: true}
> )
> ```
>
> With update you can also use field paths for updating nested values:
>
> ```javascript
> update({
>   'a.b.c': true
> })
> ```

from [Difference between set with {merge: true} and update](https://stackoverflow.com/questions/46597327/difference-between-set-with-merge-true-and-update)

**Removing a listener from firestorm**

```javascript
let unsub = db.collection('cities').onSnapshot(() => {

});


// ...

// Stop listening for changes
unsub();
```

[https://firebase.google.com/docs/firestore/query-data/listen\#detach\_a\_listener](https://firebase.google.com/docs/firestore/query-data/listen#detach_a_listener)

