# Custom tokens



[Minting Custom Tokens with the Admin SDK for Node.js - Firecasts](https://youtu.be/WtYzHTXHBp0)

[Firebase: Create custom tokens without service account credentials](https://medium.com/@hiranya911/firebase-create-custom-tokens-without-service-account-credentials-d6049c2d2d85)

> If your code is not running in a managed runtime in Google Cloud, you must either provide a service account JSON file or at very least the serviceAccountId app option.

```javascript
admin.initializeApp({
  serviceAccountId: 'my-client-id@my-project-id.iam.gserviceaccount.com',
});
```

When you got to `Firebase service account` under settings it gives you the IAM role for the account

From [How to create Firebase Authentication custom token without service account json in nodejs?](https://stackoverflow.com/questions/56673471/how-to-create-firebase-authentication-custom-token-without-service-account-json)

