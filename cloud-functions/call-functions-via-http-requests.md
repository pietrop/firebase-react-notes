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

[https://firebase.google.com/docs/functions/local-emulator\#serve\_http\_functions\_from\_the\_command\_line](https://firebase.google.com/docs/functions/local-emulator#serve_http_functions_from_the_command_line)



