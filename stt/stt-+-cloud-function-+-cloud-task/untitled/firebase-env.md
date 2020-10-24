# Firebase ENV

1.Getting the key , in the firebase dashboard 

Project Settings --&gt; General --&gt; `Web API Key`

2. Setting ENV in firebase project, in terminal

```text
firebase functions:config:set webapi.key="THE API KEY"
```

3. using the ENV API key in the cloud function

```javascript
const firebaseApiKey = functions.config().webapi.key; 
```

{% embed url="https://stackoverflow.com/questions/34442739/how-does-one-set-private-environment-variables-on-firebase-hosting" %}

This will take effect on next deployment of the firebase app.

