# App Preview Action
* Creates a new preview channel (and its associated preview URL) for every PR on your GitHub repository.
* Adds a comment to the PR with the preview URL so that you and each reviewer can view and test the PR's changes in a "preview" version of your app.
* Updates the preview URL with changes from each commit by automatically deploying to the associated preview channel.

## Usage
```yaml
name: Post to App Preview comment

on:
  pull_request:
    # Optionally configure to run only for specific files. For example:
    # paths:
    # - "app/**"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      # Any build step. For example building a Flutter app: 
      - uses: actions/checkout@v3
      - uses: subosito/flutter-action@v2
      - run: flutter build apk
      
      # Upload your builds to GitHub Artifacts
      uses: actions/upload-artifact@v3
      with:
        name: android-artifact
        path: build/app/outputs/flutter-apk/app-debug.apk.
  
  post_comment:
    runs-on: ubuntu-latest
    # Waiting until the build job is finished and the URLs to the artifacts become available.
    needs: [build]
    steps:
      - uses: SharezoneApp/app-preview-action@v0
        with:
          android-artifact: "android-artifact"
```

## Why GitHub Artifacts are not a solution
This would be a great and simple solution. Unfortunately, there are some limitations:
* [Artifact download URL only work for registered users (404 for guests)](https://github.com/actions/upload-artifact/issues/51)
* [Download artifacts of latest build](https://github.com/actions/upload-artifact/issues/21)
* A complete workflow needs to be finished until the artifact is available
* Files are **always** formatted as zip: https://github.com/actions/upload-artifact#zipped-artifact-downloads