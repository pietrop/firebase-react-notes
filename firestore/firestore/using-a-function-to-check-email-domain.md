# Using a function to check email domain

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /podcasts/{podcast=**} {
      function isValidEmail(){
       return (request.auth.token.email.matches('.*@domain[.]com$') && 
        request.auth.token.email_verified)
      }
      allow write: if isValidEmail();
      allow read;
    }
  }
}
```

