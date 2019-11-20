# Firebase cloud function local development

To use firebase cloud functions while doing local development you need to add `useFunctionsEmulator` when you initialise firebase in your app.  


> For **locally** you must call \(after firebase.initializeApp\)

```javascript
firebase.functions().useFunctionsEmulator('http://localhost:4001')
```

from Stack Overflow: [How to test \`functions.https.onCall\` firebase cloud functions locally?  
](https://stackoverflow.com/questions/50884534/how-to-test-functions-https-oncall-firebase-cloud-functions-locally)  
[https://firebase.google.com/docs/reference/js/firebase.functions.Functions\#use-functions-emulator](https://firebase.google.com/docs/reference/js/firebase.functions.Functions#use-functions-emulator)

