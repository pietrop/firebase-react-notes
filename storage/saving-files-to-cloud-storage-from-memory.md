---
description: Eg saving a json
---

# Saving files to cloud storage from memory

docs

{% embed url="https://googleapis.dev/nodejs/storage/latest/File.html\#save" %}

```javascript
const bucket = admin.storage().bucket(testEnv.storageBucket);
const file = bucket.file(mockObject.name);
const contents = JSON.stringify(mockObject, null, 2);

file.save(contents, function(err) {
  if (!err) {
    // file written
  }
});
```

{% embed url="https://github.com/googleapis/google-cloud-node/issues/2334\#issuecomment-466032478" %}



