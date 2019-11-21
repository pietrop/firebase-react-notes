# Google Sign in custom domain

[`firebase.auth.GoogleAuthProvider`](https://firebase.google.com/docs/reference/js/firebase.auth.GoogleAuthProvider)

[Firebase limit access to certain domains](https://blog.jimmycai.com/p/firebase-limit-access-to-certain-domains/)

> Sets the OAuth custom parameters to pass in a Google OAuth request for popup and redirect sign-in operations. Valid parameters include 'hd', 'hl', 'include\_granted\_scopes', 'login\_hint' and 'prompt'. For a detailed list, check the [Google](https://goo.gl/Xo01Jm) documentation. Reserved required OAuth 2.0 parameters such as 'client\_id', 'redirect\_uri', 'scope', 'response\_type' and 'state' are not allowed and will be ignored

From [setCustomParameters](https://firebase.google.com/docs/reference/js/firebase.auth.GoogleAuthProvider#set-custom-parameters) firebase docs 

{% hint style="warning" %}
The domain in `hd` needs to be associated with [G Suite domain](https://gsuite.google.com/)
{% endhint %}

```javascript
 const provider = new firebase.auth.GoogleAuthProvider();
    provider.setCustomParameters({
      hd: "example.com"
    });
```

Wrapped in a function inside a react hook component

```javascript
const signIn = () => {
    const provider = new firebase.auth.GoogleAuthProvider();
    provider.setCustomParameters({
      hd: "wsj.com"
    });

    firebase
      .auth()
      .signInWithPopup(provider)
      .then(function(result) {
        // This gives you a Google Access Token. You can use it to access the Google API.
        var token = result.credential.accessToken;
        // The signed-in user info.
        var user = result.user;
        console.log("signed-in", user);
        // ...
      })
      .catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        console.log("error", error);
        // ...
      });
  };
```

in context in a react hook component

```javascript
import React, { useEffect } from "react";
import StyledFirebaseAuth from "react-firebaseui/StyledFirebaseAuth";
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import NavDropdown from "react-bootstrap/NavDropdown";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import {
  faMicrophone,
  faHeadphones,
  faFileAudio,
  faSignOutAlt,
  faSignInAlt,
  faUser
} from "@fortawesome/free-solid-svg-icons";
import Container from "react-bootstrap/Container";
import CustomNotice from "../CustomNotice";

function CustomNavbar(props) {
  const { firebase } = props;
  // eslint-disable-next-line react-hooks/exhaustive-deps
  useEffect(() => {
    const unsubscribe = firebase.auth().onAuthStateChanged(user => {
      if (user) {
        props.handleUserChange(true);
      } else {
        props.handleUserChange(false);
      }
    });

    // clean up
    // https://reactjs.org/docs/hooks-effect.html#example-using-hooks-1
    return function cleanup() {
      // TODO: add some clean up logic here for logout?
      unsubscribe();
    };
  }, []);

  const signIn = () => {
    const provider = new firebase.auth.GoogleAuthProvider();
    provider.setCustomParameters({
      hd: "wsj.com"
    });

    firebase
      .auth()
      .signInWithPopup(provider)
      .then(function(result) {
        // This gives you a Google Access Token. You can use it to access the Google API.
        var token = result.credential.accessToken;
        // The signed-in user info.
        var user = result.user;
        console.log("signed-in", user);
        // ...
      })
      .catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
        // The email of the user's account used.
        var email = error.email;
        // The firebase.auth.AuthCredential type that was used.
        var credential = error.credential;
        console.log("error", error);
        // ...
      });
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
          {firebase.auth().currentUser ? (
            <Nav>
              <Nav.Link href="#/new-mic-recording">
                <FontAwesomeIcon
                  icon={faMicrophone}
                  title="New Mic Record"
                  size="1x"
                />{" "}
                New
              </Nav.Link>
            </Nav>
          ) : null}
          <Nav className="mr-auto"></Nav>
          <Nav>
            {firebase.auth().currentUser ? (
              <>
                <Nav.Link href="#/list-recordings">
                  <FontAwesomeIcon
                    icon={faFileAudio}
                    title="Transcripts"
                    size="1x"
                  />{" "}
                  Transcripts
                </Nav.Link>
                <Nav.Link style={{cursor: 'default'}}>
                  {firebase.auth().currentUser.email}{" "}
                </Nav.Link>
                <Nav.Link
                  onClick={() => firebase.auth().signOut()}
                  title={`sign out ${firebase.auth().currentUser.displayName}`}
                >
                  <FontAwesomeIcon icon={faSignOutAlt} />
                </Nav.Link>
              </>
            ) : (
              <Nav.Link onClick={signIn} title={`sign In`}>
                {"Sign in "}
                <FontAwesomeIcon icon={faSignInAlt} />
              </Nav.Link>
            )}
          </Nav>
          <></>
        </Navbar.Collapse>
      </Navbar>

      {navigator.onLine ? null : (
        <Container>
          <br />
          <CustomNotice
            variant={"warning"}
            title={"Offline"}
            description={"You are offline"}
          />
        </Container>
      )}
    </>
  );
}

export default CustomNavbar;

```

