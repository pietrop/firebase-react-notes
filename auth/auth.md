# Auth

Firestore Auth

[https://firebase.google.com/docs/firestore/quickstart](https://firebase.google.com/docs/firestore/quickstart)

See next steps in firestorm [https://firebase.google.com/docs/firestore/quickstart\#next\_steps](https://firebase.google.com/docs/firestore/quickstart#next_steps)

[Handle the sign-in flow with the Firebase SDK](https://firebase.google.com/docs/auth/web/google-signin?authuser=0#handle_the_sign-in_flow_with_the_firebase_sdk)



[https://firebase.google.com/docs/auth/web/start](https://firebase.google.com/docs/auth/web/start)

[firebaseui](https://firebase.google.com/docs/auth/web/firebaseui)

[Using FirebaseUI Auth, on the Web - Firecasts](https://youtu.be/hb85pYZSJaI)

[Role-Based Authorization with Firestore](https://youtu.be/1PEdd2rtG30)

[7 tips on Firebase security rules and the Admin SDK](https://firebase.googleblog.com/2019/03/firebase-security-rules-admin-sdk-tips.html)

[Controlling Data Access Using Firebase Auth Custom Claims \(Firecasts\)](https://youtu.be/3hj_r_N0qMs)

[Integrating back-end systems with Firebase for better app management \(Firebase Summit 2018\)](https://youtu.be/Hlxby1Mmic8)

> Create a form that allows new users to register with your app using their email address and a password. When a user completes the form, validate the email address and password provided by the user, then pass them to the createUserWithEmailAndPassword method:

Can use Firebase UI instead?

[github comment, redirect sign in url](https://github.com/firebase/firebaseui-web/issues/3#issuecomment-222750104)

```javascript
 signInSuccessUrl: function(currentUser, credential, redirectUrl) {
      // No redirect.
      return false;
    },
```

