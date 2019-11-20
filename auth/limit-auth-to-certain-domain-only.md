# limit auth to certain domain only





[Firebase limit access to certain domains](https://blog.jimmycai.com/p/firebase-limit-access-to-certain-domains/)

```javascript
var provider = new firebase.auth.GoogleAuthProvider();
provider.setCustomParameters({
    hd: "example.com"
});
```

[`firebase.auth.GoogleAuthProvider`](https://firebase.google.com/docs/reference/js/firebase.auth.GoogleAuthProvider)

~[Restrict Firebase authentication to a certain domain](https://stackoverflow.com/questions/46614476/restrict-firebase-authentication-to-a-certain-domain)~

```text
{
  "rules": {
    ".read": "auth.token.email.matches(/.*@wsj.com$/)",
    ".write": "auth.token.email.matches(/.*@wsj.com$/)"
   }
}
```

[How do I create firestore rules based on users email domain?](https://stackoverflow.com/questions/49174634/how-do-i-create-firestore-rules-based-on-users-email-domain)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /podcasts/{podcast=**} {
      function isValidEmail(){
       return (request.auth.token.email.matches('.*@wsj[.]com$') && 
        request.auth.token.email_verified)
      }
      allow read, write: if isValidEmail();
    }
  }
}
```

