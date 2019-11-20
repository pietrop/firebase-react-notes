# Deploying To Firebase Hosting

We are almost ready to deploy our react app to Firebase hosting. First we need to add a line to firebase.json to automatically build our application before deploying. Otherwise you will forget to do it. Add the following line to the hosting section:

```javascript
"predeploy": ["npm --prefix \"$RESOURCE_DIR/..\" run build"]
```

In context

```javascript
{
  "functions": {
    "predeploy": ["npm --prefix \"$RESOURCE_DIR/..\" run build"]
  },
  "hosting": {
    "public": "build", 
    "predeploy": ["npm --prefix \"$RESOURCE_DIR/..\" run build"]
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**",
      "**/functions/**",
      "**/public/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

Now we can deploy:

```bash
firebase deploy
```

React will build and a link will be provided in the console. You should be able to click on that link your React application will load!

