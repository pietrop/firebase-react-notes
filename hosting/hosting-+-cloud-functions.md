# Hosting + cloud functions

I think the order of the rules matter but not 100% sure.

For a CRA, client side react app, you need `/**` for the source, otherwise it won't be able to serve the static assets in the same folder.

```javascript
    "rewrites": [
      {
        "source": "/api",
        "function": "api"
      },
      {
        "source": "/**",
        "destination": "/index.html"
      }
    ]
```

Then In a cloud function. This example uses express, which is useful if you want to append multiple routes eg `/api/books` , `/api/projects` where books and projects can be express routes in the cloud function.  

but you don't have to use express. Eg you have one route you could just handle that directly. 

Express can be useful to add express middleware to the request, like cors or auth. 

```java
const functions = require('firebase-functions')
const express = require('express')
const app = express()
app.get('*', (req, res) => {
  res.send("Hello from the API")
})
exports.api = functions.https.onRequest(app)
```

full `firebase.json`

```javascript
{
  "hosting": {
    "public": "build",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "redirects": [],
    "rewrites": [
      {
        "source": "/api",
        "function": "api"
      },
      {
        "source": "/**",
        "destination": "/index.html"
      }
    ]
  },
  "firestore": {
    "rules": "firestore.rules",
    "indexes": "firestore.indexes.json"
  },
  "functions": {
    "predeploy": [],
    "ignore": [
      "**/examples/**"
    ],
    "source": "functions"
  },
  "storage": {
    "rules": "storage.rules"
  }
}
```

{% embed url="https://javascript.plainenglish.io/hosting-your-react-js-and-node-js-apps-for-free-with-firebase-6dc670564aca" %}



