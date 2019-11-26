# ffmpeg transcoding video

in a  Google Cloud Function that is triggered by the upload Google Cloud Storage bucket.

Interesting how it uses stream to save the output of ffmpeg back into the bucket

```javascript
const storage = require('@google-cloud/storage')();
const ffmpegPath = require('@ffmpeg-installer/ffmpeg').path;
const ffmpeg = require('fluent-ffmpeg');

const transcodedBucket = storage.bucket('transcode');
const uploadBucket = storage.bucket('upload');
ffmpeg.setFfmpegPath(ffmpegPath);

exports.transcodeVideo = function transcodeVideo(event, callback) {
  const file = event.data;

  // Ensure that you only proceed if the file is newly created, and exists.
  if (file.metageneration !== '1' || file.resourceState !== 'exists') {
    callback();
    return;
  }

  // Open write stream to new bucket, modify the filename as needed.
  const remoteWriteStream = transcodedBucket.file(file.name.replace('.webm', '.mp4'))
    .createWriteStream({
      metadata: {
        metadata: file.metadata, // You may not need this, my uploads have associated metadata
        contentType: 'video/mp4', // This could be whatever else you are transcoding to
      },
    });

  // Open read stream to our uploaded file
  const remoteReadStream = uploadBucket.file(file.name).createReadStream();

  // Transcode
  ffmpeg()
    .input(remoteReadStream)
    .outputOptions('-c:v copy') // Change these options to whatever suits your needs
    .outputOptions('-c:a aac')
    .outputOptions('-b:a 160k')
    .outputOptions('-f mp4')
    .outputOptions('-preset fast')
    .outputOptions('-movflags frag_keyframe+empty_moov')
    // https://github.com/fluent-ffmpeg/node-fluent-ffmpeg/issues/346#issuecomment-67299526 
    .on('start', (cmdLine) => {
      console.log('Started ffmpeg with command:', cmdLine);
    })
    .on('end', () => {
      console.log('Successfully re-encoded video.');
      callback();
    })
    .on('error', (err, stdout, stderr) => {
      console.error('An error occured during encoding', err.message);
      console.error('stdout:', stdout);
      console.error('stderr:', stderr);
      callback(err);
    })
    .pipe(remoteWriteStream, { end: true }); // end: true, emit end event when readable stream ends
};
```

from [Transcoding Videos from Google Cloud Storage using FFMPEG in Google Cloud Functions](https://kpetrovi.ch/2017/11/02/transcoding-videos-with-ffmpeg-in-google-cloud-functions.html)

