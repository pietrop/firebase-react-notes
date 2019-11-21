---
description: On the backend you can use custom claims to group user's permissions
---

# Custom claims

> What I'd recommend is to separate the auth from the access by using [Custom Claims](https://firebase.google.com/docs/auth/admin/custom-claims). Allow any one to create a user, but [attach a Cloud Function](https://firebase.google.com/docs/auth/extend-with-functions#trigger_a_function_on_user_creation) to the user create event. If the user matches one on the white list, set a [custom user claim](https://firebase.google.com/docs/auth/admin/custom-claims) \(this just launched recently!\)
>
> Finally, in your rules, check for that use property before giving access to the data:

```javascript
{
  "rules": {
    "adminContent": {
      ".read": "auth.token.admin === true",
      ".write": "auth.token.admin === true",
    }
  }
}
```

from [Firebase Authentication with whitelisted email addresses ](https://stackoverflow.com/questions/46552886/firebase-authentication-with-whitelisted-email-addresses)

