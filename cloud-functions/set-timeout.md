# Set timeout and memory allocation

[https://firebase.google.com/docs/functions/manage-functions\#set\_timeout\_and\_memory\_allocation](https://firebase.google.com/docs/functions/manage-functions#set_timeout_and_memory_allocation)

```javascript
const runtimeOpts = {
  timeoutSeconds: 540,
  memory: '2GB'
}

exports.myStorageFunction = functions
  .runWith(runtimeOpts)
  .storage
  .object()
  .onFinalize((object) = > {
    // do some complicated things that take a lot of memory and time
  });
```

> The maximum value for `timeoutSeconds` is `540`, or 9 minutes.



