# HTTPS Callable

HTTPS Callable [Call functions from your app](https://firebase.google.com/docs/functions/callable)

```javascript
exports.addMessage = functions.https.onCall((data, context) => {
  // ...
});
```

[Sending back the result](https://firebase.google.com/docs/functions/callable)

> to send data back to the client, return data that can be JSON encoded.
>
> To return data after an asynchronous operation, return a promise. The data returned by the promise is sent back to the client.

In client [Call the function](https://firebase.google.com/docs/functions/callable#call_the_function)

```javascript
var addMessage = firebase.functions().httpsCallable('addMessage');
addMessage({text: messageText}).then(function(result) {
  // Read result of the Cloud Function.
  var sanitizedMessage = result.data.text;
  // ...
});
```

[How to protect firebase Cloud Function HTTP endpoint to allow only Firebase authenticated users?](https://stackoverflow.com/questions/42751074/how-to-protect-firebase-cloud-function-http-endpoint-to-allow-only-firebase-auth)

```javascript
export const getData = functions.https.onCall((data, context) => {
  // verify Firebase Auth ID token
  if (!context.auth) {
    // return { message: 'Authentication Required!', code: 401 };
// https://firebase.google.com/docs/functions/callable#handle_errors
  throw new functions.https.HttpsError(
      "failed-precondition",
      "The function must be called " + "while authenticated."
    );
  }

  // do your things..
  const uid = context.auth.uid;
  const query = data.query;

  return { message: 'Some Data', code: 400 };
});
```

```
exports.helloWorld = functions.https.onCall((data, context) => {
```

```javascript
  if (!context.auth) {
    throw new functions.https.HttpsError(
      "failed-precondition",
      "The function must be called " + "while authenticated."
    );
  }
  return { text: "Hello World" };
});
```

  
[https://github.com/firebase/functions-samples](https://github.com/firebase/functions-samples)   
 

