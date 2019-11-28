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
 

## Auth check email address domain

Sometimes you want to add an extra layer of security in case the firebase client side auth is compromised.  

`context.auth` would tell you if the user is authenticated. But if you have a setup where you want to restrict auth to a specific email domain, malicious attackers might be able to circumvent this on the client side. 

For instance if you set up auth with[ Google sign in custom domain](../auth/google-sign-in/google-sign-in-custom-domain.md) \(using G Suite\)  there is the possibility that on the client the attacker could remove the custom domain restriction and sign in with any regular gmail.  
  
The following code can help you further mitigate that with server side validation in a cloud function and check the auth email against a specific domain.

```javascript
  // Get email address from auth and check is @example.com email
  const email = context.auth.token.email;
  if(!email.endsWith('@example.com')){
    throw new functions.https.HttpsError(
      "failed-precondition",
      "The function must be called " + "while authenticated with WSJ email address."
    );
  }
```

Modified from  stackoverflow [Get user phone number in firebase cloud functions](https://stackoverflow.com/questions/52913170/get-user-phone-number-in-firebase-cloud-functions)

For more background on [`endsWith`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith) it's exactly what you'd expect from its name

> The **`endsWith()`** method determines whether a string ends with the characters of a specified string, returning `true` or `false` as appropriate.

