---
description: On the backend you can add security rules to the database
---

# Limit auth to certain domain only

[Restrict Firebase authentication to a certain domain](https://stackoverflow.com/questions/46614476/restrict-firebase-authentication-to-a-certain-domain)

```text
{
  "rules": {
    ".read": "auth.token.email.matches(/.*@example.com$/)",
    ".write": "auth.token.email.matches(/.*@example.com$/)"
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
       return (request.auth.token.email.matches('.*@example[.]com$') && 
        request.auth.token.email_verified)
      }
      allow read, write: if isValidEmail();
    }
  }
}
```

