# Call functions via HTTP requests

[Call functions via HTTP requests](https://firebase.google.com/docs/functions/http-events)

```javascript
exports.date = functions.https.onRequest((req, res) => {
  // ...
});
```

```javascript
exports.helloWorldPublic = functions.https.onRequest((request, response) => {
  response.send("Hello World");
});
```

```javascript

```

