# Travis CI integration

[Which Google Cloud services work with Firebase? - \#AskFirebase - 2.43min in](https://youtu.be/gpiDUAirCXQ?t=163)

Firebase CLI, can give you a key for continuous integration like `travis`. See [https://docs.travis-ci.com/user/deployment/firebase/](https://docs.travis-ci.com/user/deployment/firebase/)

```text
firebase login:ci
```

In travis config, can add ENV `FIREBASE_TOKEN`.

In  `.travis.yml`

```text
...

install
 - nom install -g firebase-tooltos

after_success:
 - firebase deploy --token $FIREBASE_TOKEN
...
```

### Travis CLI

Alternatively, after you generate the Firebase token from running the `login:ci` command, [install `travis` CLI](https://github.com/travis-ci/travis.rb#installation) and do:

```bash
travis encrypt "<FIREBASE_TOKEN>" --add
# This command may generate a warning ("If you tried to pass the name of the repository as the first argument, you probably won't get the results you wanted"). You can ignore it.
```

This will add the encrypted secure token to your Travis config file.

#### .env file with secure tokens

If you have a `.env` file that stores all your configuration, you should also do this:

```bash
travis encrypt-file .env --add
```

This will create an `.env.enc` file, and the step to decode is added to your Travis config file. You should commit the `.env.enc` to your Github repository. 

Note: that there is a [known bug](https://github.com/travis-ci/travis.rb/issues/711) with Travis version `1.8.11` , and you would need to install `1.8.10` instead.

### Deploying specific services \(hosting, functions, firestore\)

If you want to deploy only to specific services - e.g. hosting or otherwise - you need to get the `edge` version of the `dpl` tool in Travis. This just means you need to add the following to your config:

```yaml
deploy:
  provider: firebase
  edge:
    branch: master
    ...
  
```

This would then allow you to use Firebase CLI's `only` option:

```yaml
deploy:
  provider: firebase
  edge:
    branch: master
  project: dev # the default project will be selected from firebase.json
  only: hosting:digital-paper-edit-dev
```

{% code title=".travis.yml" %}
```yaml
language: node_js
node_js:
- '10'
cache:
  directories:
  - node_modules
before_script:
- yarn install
script:
- CI=false yarn build # ignore build warnings
deploy:
  provider: firebase
  edge:
    branch: master
  token:
    secure: cEpLS+5f3V1CZ24Tr7U+9p0ajX9jReRUEXfmAiXn4Pe4Cl4C7zLeoSRlPc/id0ux3VjhSFArkrhA2VYOUoyk+lkZgxhnP5yRxS/6E0vye5Y5eot36HHBuynFTPKv+JLJfaD9d0MX6CBoxZoU7kkXt+GDx5tvATHfmrXWEyETmAc7qKpinlQk9HWBFEouxlbzrY01pQiNLHf9+BFTaYwlqutLuy24i6cARdi+fXdaQyzFw2BMlQK1AHBhxdOIB96l8/eUvC89AE/uISP8+4lTNFMTAn8anOv+oUwa5hdxh5GdmjCj/AqdwyPThiTu1wNd7Uk/VPSHsG0lpmbCy7rr5d783+2qC0SG0KdX2NOkPqgWPSOVmO44suCtD2j9Mh5xOrs9Kib9VwSsqqKqItv/EX7BJyzS3ixA8CrSlLaqo6mGLreWkzTdDv19ArU1YfSIdUOwrybq5zpaooQnyNfgP7sT3cNcreBqI8tvhU+GDI/aWDgxwAqPEyEsWqSPSfvQuXda5BJAf6LyRIKIzJTtVRfnXI7AMeHYV7+m3vTcrIh5XXGXj+mMYTU+vliuqLNADmuqdepeIZ/Y6b2UNRKQu6jB5jYCftY0YJH8SHlDU2skevqEYjw0KwMP461uyLBnB26ydmkxx3ptz56jqL5odAoSzK3bwiaXYJ5UlrBVaS8=
  message: this is an automatic deployment to dev by travis - CI as a result of pushing
    to master
  project: dev
  only: hosting:digital-paper-edit-dev
before_install:
- openssl aes-256-cbc -K $encrypted_3c84dcdc6bbe_key -iv $encrypted_3c84dcdc6bbe_iv
  -in .env.enc -out .env -d # automatically added by encrypt-file
```
{% endcode %}

You can see the full file [here](https://github.com/bbc/digital-paper-edit-firebase/blob/master/.travis.yml).



