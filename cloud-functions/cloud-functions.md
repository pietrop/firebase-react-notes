# Cloud Functions

## Environment Var and Config

[Using Environment Variables](https://cloud.google.com/functions/docs/env-var)

Also [Environment configuration](https://firebase.google.com/docs/functions/config-env)

[https://medium.com/@vik.ceo/setting-up-environment-variables-for-firebase-functions-d78e52b84f48](https://medium.com/@vik.ceo/setting-up-environment-variables-for-firebase-functions-d78e52b84f48)  
  
[https://stackoverflow.com/questions/44766536/how-do-you-setup-local-environment-variables-for-cloud-functions-for-firebase](https://stackoverflow.com/questions/44766536/how-do-you-setup-local-environment-variables-for-cloud-functions-for-firebase)

For this to run locally   
  
`firebase functions:config:get > .runtimeconfig.json` in `functions`   
  
folder, also add to `.gitignore`   
  
[https://stackoverflow.com/questions/51883178/firebase-functions-environment-variables-can-not-read-property-of-undefined](https://stackoverflow.com/questions/51883178/firebase-functions-environment-variables-can-not-read-property-of-undefined)   
  
[https://cloud.google.com/functions/docs/env-var](https://cloud.google.com/functions/docs/env-var)   
  
 to set in firebase environment do 

`firebase functions:config:set aws.aws_access_key_id="XXXX" aws.aws_scret_access_key="XXXX"`

## Triggers

Firestore triggers [https://firebase.google.com/docs/functions/firestore-events](https://firebase.google.com/docs/functions/firestore-events)

Since you **cannot configure a function to be triggered by subdirectories**. It's only possible to listen to the entire bucket. If you have a trigger based on Bucket, the recommendation is to create a new, separate bucket so that you are not triggered every time a file changes in the Bucket.

You can do environments for Functions:

```bash
$ firebase -P dev functions:config:set storage.bucket="dev-digital-paper-edit"
> ✔  Functions config updated.

$ firebase -P dev functions:config:get
> {
    "storage": {
      "bucket": "dev-digital-paper-edit"
    }
  }
$ firebase -P prod functions:config:get
> {}
```

And then subsequently in a Function you'd use the config like so:

```javascript
const functions = require("firebase-functions");
const BucketFunctions = functions.storage.object(
 functions.config().storage.bucket
);
```



