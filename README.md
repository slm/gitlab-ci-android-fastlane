# gitlab-ci-android-fastlane-gcloud
This Docker image contains the Android SDK and most common packages necessary for building Android apps in a CI tool like GitLab CI (Android SDK, git, fastlane, gcloud). Make sure your CI environment's caching works as expected, this greatly improves the build time, especially if you use multiple build jobs.

Use generated public key as gitlab deploy key

```
docker run -it --rm slmyldz/gitlab-ci-android-fastlane-gcloud
cat ~/.ssh/id_rsa
```

A `.gitlab-ci.yml` with caching of your project's dependencies would look like this:

```
image: slmyldz/gitlab-ci-android-fastlane-gcloud

stages:
- build

before_script:
- export GRADLE_USER_HOME=$(pwd)/.gradle
- chmod +x ./gradlew

cache:
  key: ${CI_PROJECT_ID}
  paths:
  - .gradle/

build:
  stage: build
  script:
  - ./gradlew assembleDebug
  artifacts:
    paths:
    - app/build/outputs/apk/app-debug.apk
```
