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

[https://stackoverflow.com/questions/43486278/how-do-i-structure-cloud-functions-for-firebase-to-deploy-multiple-functions-fro](https://stackoverflow.com/questions/43486278/how-do-i-structure-cloud-functions-for-firebase-to-deploy-multiple-functions-fro)   
  
[https://github.com/firebase/functions-samples](https://github.com/firebase/functions-samples)   
[  
https://github.com/firebase/functions-samples/blob/master/ffmpeg-convert-audio/functions/index.js](https://github.com/firebase/functions-samples/blob/master/ffmpeg-convert-audio/functions/index.js)   
  
[https://medium.com/@vik.ceo/setting-up-environment-variables-for-firebase-functions-d78e52b84f48](https://medium.com/@vik.ceo/setting-up-environment-variables-for-firebase-functions-d78e52b84f48)   
  
[https://stackoverflow.com/questions/44766536/how-do-you-setup-local-environment-variables-for-cloud-functions-for-firebase](https://stackoverflow.com/questions/44766536/how-do-you-setup-local-environment-variables-for-cloud-functions-for-firebase)

For this to run locally   
  
`firebase functions:config:get > .runtimeconfig.json` in `functions`   
  
folder, also add to `.gitignore`   
  
[https://stackoverflow.com/questions/51883178/firebase-functions-environment-variables-can-not-read-property-of-undefined](https://stackoverflow.com/questions/51883178/firebase-functions-environment-variables-can-not-read-property-of-undefined)   
  
[https://firebase.google.com/docs/functions/local-emulator\#serve\_http\_functions\_from\_the\_command\_line](https://firebase.google.com/docs/functions/local-emulator#serve_http_functions_from_the_command_line)

[https://cloud.google.com/functions/docs/env-var](https://cloud.google.com/functions/docs/env-var)   
  
 to set in firebase environment do 

`firebase functions:config:set aws.aws_access_key_id="XXXX" aws.aws_scret_access_key="XXXX"`

