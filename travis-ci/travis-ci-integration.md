# Travis CI integration

[Which Google Cloud services work with Firebase? - \#AskFirebase - 2.43min in](https://youtu.be/gpiDUAirCXQ?t=163)

Firebase CLI, can give you a key for continuous integration like travis.

```text
firebase login:ci
```

In travis config, can add ENV `FIREBASE_TOKEN`.

In travis yml

```text
...

install
 - nom install -g firebase-tooltos

after_success:
 - firebase deploy --token $FIREBASE_TOKEN
...
```

