# Create React App + Firebase

## Run create-react-app

If you already have an application then you can skip these and go to installing Firebase.

To make some steps easier let's pick a name for our new application which we will save as an environment variable to reuse.

```bash
export REACT_APP=hello-firebase; # replace with your application's name
```

Use npx and create-react-app to boilerplate a new react application. I recommend using Typescript to enable type-checking

```bash
npx create-react-app ${REACT_APP} --typescript
```

Wait a little bit while create-react-app gets ready. When finished enter the application directory:

```bash
cd ${REACT_APP}
```

## Install firebase-tools

In order to work with firebase you will need the firebase command line tool. The easiest way to get it is to install globally with npm:

```bash
npm i -g firebase-tools
```

