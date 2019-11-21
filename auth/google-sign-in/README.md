---
description: Firebase Gmail Auth
---

# Google sign in

 [https://firebase.google.com/docs/auth/web/google-signin](https://firebase.google.com/docs/auth/web/google-signin)



```javascript
// Add the Firebase services that you want to use
import "firebase/auth";
```

### React

[`react-firebaseui`](https://github.com/firebase/firebaseui-web-react)

> Firebase Auth has provided a parameter hd to limit accounts shown in Google OAuth UI:

```javascript
const provider = new firebase.auth.GoogleAuthProvider();
provider.setCustomParameters({
    hd: "example.com"
});
```

From [Firebase limit access to certain domains](https://blog.jimmycai.com/p/firebase-limit-access-to-certain-domains/) and [`setCustomParameters`](https://firebase.google.com/docs/reference/js/firebase.auth.GoogleAuthProvider)

> Don't rely on this UI optimization to control who can access your app, as client-side requests can be modified. Be sure to validate that the returned ID token has an hd claim value that matches what you expect \(e.g. mycolledge.edu\). Unlike the request parameter, the ID token claim is contained within a security token from Google, so the value can be trusted.

from [Authentication URI parameters](https://developers.google.com/identity/protocols/OpenIDConnect#authenticationuriparameters)

However it is unclear if this is possibel with `react-firebaseui` or `firebaseui`. I raised issue [Limit auth to a certain domain \#78](https://github.com/firebase/firebaseui-web-react/issues/78) to find out.

Meanwhile to do gmail auth `CustomNavbar.js`

```javascript
import React, {  useEffect } from "react";
import StyledFirebaseAuth from "react-firebaseui/StyledFirebaseAuth";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import NavDropdown from "react-bootstrap/NavDropdown";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faMicrophone,
  faPhone,
  faCalendarAlt,
  faHeadphones,
  faFileAudio,
  faFileUpload,
  faSignOutAlt
} from "@fortawesome/free-solid-svg-icons";
import Container from 'react-bootstrap/Container';
import CustomNotice from '../CustomNotice';

function CustomNavbar(props) {

  const { firebase } = props;

  // eslint-disable-next-line react-hooks/exhaustive-deps
  useEffect(() => {
    firebase.auth().onAuthStateChanged(user => {
      if (user) {
        props.handleUserChange(true);
      } else {
        props.handleUserChange(false);
      }
      //   this.props.handleUserChange(user)
    });

    // https://reactjs.org/docs/hooks-effect.html#example-using-hooks-1
    return function cleanup() {
       // TODO: add some clean up logic here for logout?
      };
  }, []);

  const uiConfig = {
    // Popup signin flow rather than redirect flow.
    signInFlow: "popup",
    // Redirect to /signedIn after sign in is successful. Alternatively you can provide a callbacks.signInSuccess function.
    signInSuccessUrl: '/',
    // We will display Google and Facebook as auth providers.
    signInOptions: [firebase.auth?firebase.auth.GoogleAuthProvider.PROVIDER_ID : null]
  };

  return (
    <>
      <Navbar sticky="top" bg="light" expand="md">
        <Navbar.Brand href="/#">
          {" "}
          <FontAwesomeIcon icon={faHeadphones} size="1x" />
          Transcriber App
        </Navbar.Brand>
        <Navbar.Toggle aria-controls="basic-navbar-nav" />
        <Navbar.Collapse id="basic-navbar-nav">
          <Nav className="mr-auto">
          </Nav>
          <Nav>
            <Nav.Link href="#/list-recordings">
              <FontAwesomeIcon
                icon={faFileAudio}
                title="Transcripts"
                size="1x"
              />{" "}
              Transcripts
            </Nav.Link>
          </Nav>
          <Nav>
          {firebase.auth().currentUser ? (
            <>
              <Nav.Link
                onClick={() => firebase.auth().signOut()}
                title={`sign out ${firebase.auth().currentUser.displayName}`}
              >
                {" "}
                {firebase.auth().currentUser.email}{" "}
                <FontAwesomeIcon icon={faSignOutAlt} />
              </Nav.Link>
            </>
          ) : (
            <StyledFirebaseAuth
              uiConfig={uiConfig}
              firebaseAuth={firebase.auth()}
            />
          )}
        </Nav>
        </Navbar.Collapse>
      </Navbar>

      {navigator.onLine? null: <Container><br/><CustomNotice variant={'warning'} title={'Offline'} description={'You are offline'} /></Container>}
    </>
  );
}

export default CustomNavbar;
```

And here is an example using it another parent component

```javascript
import React, { useState } from "react";

import Container from "react-bootstrap/Container";
import Row from "react-bootstrap/Row";
import Col from "react-bootstrap/Col";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faMicrophone,
  faPhone,
  faCalendarAlt,
  faFileAudio,
  faFileUpload
} from "@fortawesome/free-solid-svg-icons";
import { Link } from "react-router-dom";
import CustomFooter from "../../Components/CustomFooter";
import CustomNavbar from "../../Components/CustomNavbar";
import CustomNotice from "../../Components/CustomNotice";
import firebase from "../../Firebase.js";

function Home() {
  const [user, setUser] = useState(null);

  const handleUserChange = isUserSignedIn => {
    setUser(isUserSignedIn);
  };

  return (
    <>
      <CustomNavbar  firebase={firebase} handleUserChange={handleUserChange}  />
      {user ? (
      <Container>
      <br/>
        <h1 className={"text-center"}>Make an audio recording</h1>
        <p className={"text-center text-muted"}>
          Make a recording to generate an automated transcription draft.
        </p>
        <br />
        <br />
        <Row className="d-flex justify-content-center">
          <Col xs={12} sm={3} className={"text-center"}>
            <Link to="/new-mic-recording">
              <FontAwesomeIcon icon={faMicrophone} size="3x" />
            </Link>
            <br />
            <br />
            <p className={"text-muted"}>Record using your microphone </p>
          </Col>
        </Row>
        <br />
        <hr />
        <br />
        <Row>
          <Col className={"text-center"}>
            <Link to="/list-recordings">
              <FontAwesomeIcon icon={faFileAudio} size="3x" />
            </Link>
            <br />
            <br />
            <p className={"text-muted"}> See recordings transcriptions</p>
          </Col>
        </Row>
        <br />
        <br />
        <br />
        <CustomFooter />
      </Container>)
      : (
        <Container>
          <br/>
         <CustomNotice
         variant={"info"}
         title={"Login"}
         description={"You need to login"}
       />
       </Container>
         )}
    </>
  );
}

export default Home;
```

### Other Notes

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

#### limit auth to certain domain only

[Firebase limit access to certain domains](https://blog.jimmycai.com/p/firebase-limit-access-to-certain-domains/)

```javascript
var provider = new firebase.auth.GoogleAuthProvider();
provider.setCustomParameters({
    hd: "example.com"
});
```

[`firebase.auth.GoogleAuthProvider`](https://firebase.google.com/docs/reference/js/firebase.auth.GoogleAuthProvider)

~[Restrict Firebase authentication to a certain domain](https://stackoverflow.com/questions/46614476/restrict-firebase-authentication-to-a-certain-domain)~

```text
{
  "rules": {
    ".read": "auth.token.email.matches(/.*@wsj.com$/)",
    ".write": "auth.token.email.matches(/.*@wsj.com$/)"
   }
}
```

#### Firestore domain auth

[How do I create firestore rules based on users email domain?](https://stackoverflow.com/questions/49174634/how-do-i-create-firestore-rules-based-on-users-email-domain)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /podcasts/{podcast=**} {
      function isValidEmail(){
       return (request.auth.token.email.matches('.*@wsj[.]com$') && 
        request.auth.token.email_verified)
      }
      allow read, write: if isValidEmail();
    }
  }
}
```

