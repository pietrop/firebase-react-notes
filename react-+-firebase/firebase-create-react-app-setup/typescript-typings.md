# Typescript typings

If you start using Firebase in your Typescript application you need to let Typescript know about this global firebase variable that you know have access to. First install firebase not to import but simply to get access to the types contained within:

```bash
npm i -D firebase
```

Now create a src/firebase.d.ts file and copy the following:

```javascript
import firebase from 'firebase'

declare global {
  export const firebase = firebase
}
```

Now you have full typescripts bindings available.

