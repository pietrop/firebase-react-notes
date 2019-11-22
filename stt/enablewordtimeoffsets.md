---
description: How to get word level timings
---

# enableWordTimeOffsets

worth noting that time stamps don't come as default, you need to add this attribute to config`enableWordTimeOffsets`  

same thing for punctuaiton with `enableAutomaticPunctuation`

and speaker diarization `enableSpeakerDiarization`

```text
const config = {
  enableWordTimeOffsets: true,
  enableAutomaticPunctuation: true,
  enableSpeakerDiarization: true,
  encoding: encoding,
  sampleRateHertz: sampleRateHertz,
  languageCode: languageCode,
};
```

more details and full code example see

[https://cloud.google.com/speech-to-text/docs/async-time-offsets](https://cloud.google.com/speech-to-text/docs/async-time-offsets)

[https://cloud.google.com/speech-to-text/docs/automatic-punctuation](https://cloud.google.com/speech-to-text/docs/automatic-punctuation)

[https://cloud.google.com/speech-to-text/docs/multiple-voices](https://cloud.google.com/speech-to-text/docs/multiple-voices)

