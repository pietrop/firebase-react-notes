# Cloud Function + STT - long running audio files

[https://gigazine.net/gsc\_news/en/20180824-speech-to-text-gcp-cloud-mojiokoshi/](https://gigazine.net/gsc_news/en/20180824-speech-to-text-gcp-cloud-mojiokoshi/)

{% hint style="warning" %}
Based on what I found in the docs, see below, it seems like it could be possible to start the  STT operation via a cloud function, and then check on the results later.
{% endhint %}

  


> _Asynchronous speech recognition_ starts a long running audio processing operation. Use asynchronous speech recognition to recognize audio that is longer than a minute. For shorter audio, [Synchronous Speech Recognition](https://cloud.google.com/speech-to-text/docs/sync-recognize) is faster and simpler.
>
> You can retrieve the results of the operation via the [google.longrunning.Operations](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/operations#resource-operation) interface. Results remain available for retrieval for 5 days \(120 hours\). Audio content can be sent directly to Cloud Speech-to-Text or it can process audio content that already resides in Google Cloud Storage. See also the [audio limits](https://cloud.google.com/speech-to-text/quotas) for asynchronous speech recognition requests.

from [https://cloud.google.com/speech-to-text/docs/async-recognize](https://cloud.google.com/speech-to-text/docs/async-recognize)

> This resource represents a long-running operation that is the result of a network API call.

```javascript
{
  "name": string,
  "metadata": {
    "@type": string,
    field1: ...,
    ...
  },
  "done": boolean,

  // Union field result can be only one of the following:
  "error": {
    object (Status)
  },
  "response": {
    "@type": string,
    field1: ...,
    ...
  }
  // End of list of possible types for union field result.
}
```

> **done - boolean**

> If the value is `false`, it means the operation is still in progress. If `true`, the operation is completed, and either `error` or `response` is available.
>
> Union field `result`. The operation result, which can be either an `error` or a valid `response`. If `done` == `false`, neither `error` nor `response` is set. If `done` == `true`, exactly one of `error` or `response` is set. `result` can be only one of the following:

  
from [https://cloud.google.com/speech-to-text/docs/reference/rest/v1/operations\#resource-operation](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/operations#resource-operation)

```javascript
// Imports the Google Cloud client library
const speech = require('@google-cloud/speech');

// Creates a client
const client = new speech.SpeechClient();

/**
 * TODO(developer): Uncomment the following lines before running the sample.
 */
// const gcsUri = 'gs://my-bucket/audio.raw';
// const encoding = 'Encoding of the audio file, e.g. LINEAR16';
// const sampleRateHertz = 16000;
// const languageCode = 'BCP-47 language code, e.g. en-US';

const config = {
  encoding: encoding,
  sampleRateHertz: sampleRateHertz,
  languageCode: languageCode,
};

const audio = {
  uri: gcsUri,
};

const request = {
  config: config,
  audio: audio,
};

// Detects speech in the audio file. This creates a recognition job that you
// can wait for now, or get its result later.
const [operation] = await client.longRunningRecognize(request);
// Get a Promise representation of the final result of the job
const [response] = await operation.promise();
const transcription = response.results
  .map(result => result.alternatives[0].transcript)
  .join('\n');
console.log(`Transcription: ${transcription}`);
```

From this example using `await` it seems like there isn't a way to get response before the result is ready. Which would not work in a cloud function.

But in in the SDK Client Refernece, SpeechClient, code example [https://googleapis.dev/nodejs/speech/latest/v1p1beta1.SpeechClient.html\#longRunningRecognize-examples](https://googleapis.dev/nodejs/speech/latest/v1p1beta1.SpeechClient.html#longRunningRecognize-examples)

## SDK

Setup

```javascript
const speech = require('@google-cloud/speech');

const client = new speech.v1p1beta1.SpeechClient({
  // optional auth parameters.
});

const encoding = 'FLAC';
const sampleRateHertz = 44100;
const languageCode = 'en-US';
const config = {
  encoding: encoding,
  sampleRateHertz: sampleRateHertz,
  languageCode: languageCode,
};
const uri = 'gs://bucket_name/file_name.flac';
const audio = {
  uri: uri,
};
const request = {
  config: config,
  audio: audio,
};
```

The either

```javascript

// Handle the operation using the promise pattern.
client.longRunningRecognize(request)
  .then(responses => {
    const [operation, initialApiResponse] = responses;

    // Operation#promise starts polling for the completion of the LRO.
    return operation.promise();
  })
  .then(responses => {
    const result = responses[0];
    const metadata = responses[1];
    const finalApiResponse = responses[2];
  })
  .catch(err => {
    console.error(err);
  });

```

Or 

```javascript

// Handle the operation using the event emitter pattern.
client.longRunningRecognize(request)
  .then(responses => {
    const [operation, initialApiResponse] = responses;

    // Adding a listener for the "complete" event starts polling for the
    // completion of the operation.
    operation.on('complete', (result, metadata, finalApiResponse) => {
      // doSomethingWith(result);
    });

    // Adding a listener for the "progress" event causes the callback to be
    // called on any change in metadata when the operation is polled.
    operation.on('progress', (metadata, apiResponse) => {
      // doSomethingWith(metadata)
    });

    // Adding a listener for the "error" event handles any errors found during polling.
    operation.on('error', err => {
      // throw(err);
    });
  })
  .catch(err => {
    console.error(err);
  });

```



`initialApiResponse`example

```javascript
{ 
    name: '8700551237405696231', 
    metadata: null, 
    done: false 
}
```

Or

```javascript

// Handle the operation using the await pattern.
const [operation] = await client.longRunningRecognize(request);

const [response] = await operation.promise();
```

## operations.get

`operations.get`   


> Gets the latest state of a long-running operation. Clients can use this method to poll the operation result at intervals as recommended by the API service.

```javascript
GET https://speech.googleapis.com/v1/operations/{name=**}
```

  
from [https://cloud.google.com/speech-to-text/docs/reference/rest/v1/operations/get](https://cloud.google.com/speech-to-text/docs/reference/rest/v1/operations/get)

Where `name` is a string and is 

> The name of the operation resource.

## Running STT long recognise in cloud function

If the audio takes to long to transcribe and the cloud function times out, from the `name` attribute of the `initialApiResponse` it would be possible to do pooling and gets the latest state of a long-running operation. Including result when it's done. via the `operations.get`.

