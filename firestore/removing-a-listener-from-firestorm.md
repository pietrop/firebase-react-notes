# Removing a listener from firestorm

In react, if you attach a listener for realtime updates when a component mounts, you'd need to remove it when the component amounts. 

Failing to do so could raise an error if you delete that item and there's a listener that is still associated with it.

```javascript
let unsub = db.collection('cities').onSnapshot(() => {

});


// ...

// Stop listening for changes
unsub();
```

[https://firebase.google.com/docs/firestore/query-data/listen\#detach\_a\_listener](https://firebase.google.com/docs/firestore/query-data/listen#detach_a_listener)

### In React hooks

if using React hooks, you use `useEffect`  and return a function to do the clean up. See the React docs for more details on the equivalent syntax to `componentWillUnmount`   
[https://reactjs.org/docs/hooks-effect.html\#example-using-hooks-1](https://reactjs.org/docs/hooks-effect.html#example-using-hooks-1)

{% hint style="warning" %}
TODO: Add code example
{% endhint %}

