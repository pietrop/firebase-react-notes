# Twilio firebase functions

[https://angularfirebase.com/lessons/sms-texting-with-twilio/](https://angularfirebase.com/lessons/sms-texting-with-twilio/)

```javascript
// firebase functions:config:set twilio.sid="YOUR_ACCOUNT_SID" twilio.token="YOUR_AUTH_TOKEN"
// https://angularfirebase.com/lessons/sms-texting-with-twilio/
exports.makePhoneCall = functions.https.onCall((data, context) => {
  if (!context.auth) {
    throw new functions.https.HttpsError(
      "failed-precondition",
      "The function must be called " + "while authenticated."
    );
  }
  const twillioPhoneNumber = "+12089532138";
  const toPhoneNumber = data.phoneNumber;

  const accountSid = functions.config().twilio.sid;
  const authToken = functions.config().twilio.token;
  const client = new twilio(accountSid, authToken);

  const main = async () => {
    try {
      const call = await client.calls.create({
        //  method: 'GET',
        //  statusCallback: 'https://www.myapp.com/events',
        //  statusCallbackEvent: ['initiated', 'answered', 'completed'],
        //  statusCallbackMethod: 'POST',
        // replace with https://www.twilio.com/console/twiml-bins/create,
        // https://www.twilio.com/docs/voice/make-calls#manage-your-outbound-call
        // record: true,
        url: "http://demo.twilio.com/docs/voice.xml",
        to: toPhoneNumber,
        from: twillioPhoneNumber
      });

      console.log(call);
      // should save callSid `call.sid` to firestore
      return {
        status: "OK",
        text: `calling ${toPhoneNumber} from ${twillioPhoneNumber}`,
        callSid: call.sid
      };
    } catch (e) {
      console.error(e);
      return { status: "error", error: e };
    }
  };

  return main();
});
```

