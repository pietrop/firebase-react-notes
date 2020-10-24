# STT + Cloud Function



{% hint style="warning" %}
This implementation is not suitable for long files as [google cloud function times out after 1min to 9min ](https://cloud.google.com/functions/docs/concepts/exec#timeout)
{% endhint %}

But for shorter files, you can add your media to a google cloud storage and run google cloud STT inside a cloud function and save the result to firestore.

[Firestore events](https://firebase.google.com/docs/functions/firestore-events)

```javascript
// Goolge App Engine calls STT SDK
// this function returns null.
exports.createTranscript = functions.firestore
  .document("transcripts/{transcriptId}")
  .onCreate(async (change, context) => {
    // Get an object representing the document
    const newValue = change.data();
    // for now only run STT for uploaded files
    // eg if running from STT then don't run this
    if (newValue.transcriptionType === "upload") {
      // access a particular field as you would any JS property
      const storageRef = newValue.storageRef;
      console.log("storageRef", storageRef);
      // https://firebase.google.com/docs/storage/admin/start
      const storage = admin.storage();
      // https://github.com/firebase/firebase-tools/issues/1573#issuecomment-517000981
      const bucket = storage.appInternal.options.storageBucket;
      // // const gcsUri = 'gs://my-bucket/audio.raw';
      const gcsUri = `gs://${bucket}/${storageRef}`;
      console.log("gcsUri", gcsUri);
      //////// STT ////////
      // Creates a client for STT
      const client = new speech.SpeechClient();
      const encoding = "mp3"; //'Encoding of the audio file, e.g. LINEAR16';
      const sampleRateHertz = "48000"; //16000;
      // const languageCode = newValue.language.value;
      const languageCode = "en-US"; //'BCP-47 language code, e.g. en-US';

      const config = {
        encoding: encoding,
        sampleRateHertz: sampleRateHertz,
        languageCode: languageCode
      };

      const audio = {
        uri: gcsUri
      };

      const request = {
        config: config,
        audio: audio
      };

      //   // Detects speech in the audio file. This creates a recognition job that you
      //   // can wait for now, or get its result later.
      const [operation] = await client.longRunningRecognize(request);
      // Get a Promise representation of the final result of the job
      const [response] = await operation.promise();
      // TODO: Convert to DPE
      // TODO: Save DPE json format to firebase as collection

      // get text - tmp
      const transcription = response.results
        .map(result => result.alternatives[0].transcript)
        .join("\n");
      console.log(`Transcription: ${transcription}`);

      // Then return a promise of a set operation to update the document
      return change.ref.set(
        {
          // TODO: change transcription to transcript
          transcription: transcription
        },
        { merge: true }
      );
    } else {
      return null;
    }
  });
```

## links 

[https://github.com/GoogleCloudPlatform/nodejs-docs-samples/tree/master/functions/speech-to-speech](https://github.com/GoogleCloudPlatform/nodejs-docs-samples/tree/master/functions/speech-to-speech)

[Supercharging Firebase with Google Cloud Platform - Google I/O 2016](https://youtu.be/wOGfZ_aLGqM?t=852)

[A Serverless Audio Transcription Pipeline](https://medium.com/@jlaham/serverless-audio-transcription-515cd7f67e9c) - [audio-2-text](https://github.com/jlaham/audio-2-text)

[Transcribing videos](https://cloud.google.com/speech-to-text/docs/video-model)

[SpeechClient - SDK](https://googleapis.dev/nodejs/speech/latest/v1p1beta1.SpeechClient.html#longRunningRecognize)

[REST Resource: operations](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/operations#resource-operation)

[LongRunningRecognizeMetadata](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/LongRunningRecognizeMetadata)

[LongRunningRecognizeResponse](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/LongRunningRecognizeResponse)

[Method: speech.recognize](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/speech/recognize#SpeechRecognitionResult)

