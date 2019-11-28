---
description: cloud function auth email domain check
---

# Auth check email address domain

Sometimes you want to add an extra layer of security in case the firebase client side auth is compromised.  

`context.auth` would tell you if the user is authenticated. But if you have a setup where you want to restrict auth to a specific email domain, malicious attackers might be able to circumvent this on the client side. 

For instance if you set up auth with[ Google sign in custom domain](../../auth/google-sign-in/google-sign-in-custom-domain.md) \(using G Suite\)  there is the possibility that on the client the attacker could remove the custom domain restriction and sign in with any regular gmail.  
  
The following code can help you further mitigate that with server side validation in a cloud function and check the auth email against a specific domain.

```javascript
  // Get email address from auth and check is @example.com email
  const email = context.auth.token.email;
  if(!email.endsWith('@example.com')){
    throw new functions.https.HttpsError(
      "failed-precondition",
      "The function must be called " + "while authenticated with @example.com email address."
    );
  }
```

Modified from  stackoverflow [Get user phone number in firebase cloud functions](https://stackoverflow.com/questions/52913170/get-user-phone-number-in-firebase-cloud-functions)

For more background on [`endsWith`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith) it's exactly what you'd expect from its name

> The **`endsWith()`** method determines whether a string ends with the characters of a specified string, returning `true` or `false` as appropriate.

