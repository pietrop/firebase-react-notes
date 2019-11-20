# Twitter sign in





You need to create a twitter app, on twitter to use this authentication method.

[Can I use Firebase simple login to access Twitter API?](https://stackoverflow.com/questions/16072282/can-i-use-firebase-simple-login-to-access-twitter-api)

> The Firebase Simple Login JavaScript client now returns both the Twitter access token and access token secret in the response payload from Twitter authentication.
>
> You can access each of these via the user object, under the attributes accessToken and accessTokenSecret as shown below:

```javascript
var firebaseRef = new Firebase('https://<MY-FIREBASE-NAME>.firebaseIO.com');
var authClient = new FirebaseAuthClient(firebaseRef, function(error, user) {
  if (user) {
    var accessToken = user.accessToken,
        accessTokenSecret = user.accessTokenSecret;
  }
});

...

// Sample jQuery click event.
$myButton.on('click', function(event) {
  authClient.login('twitter');
});
```

