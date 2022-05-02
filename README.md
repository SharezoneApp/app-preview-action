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
  build_and_preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      # Any build step. For example building a Flutter app: 
      - uses: uses: subosito/flutter-action@v2
      - run: flutter build apk
      
      # Upload your builds to GitHub Artifacts
      uses: actions/upload-artifact@v3
      with:
        name: android-artifact
        path: build/app/outputs/flutter-apk/app-debug.apk.
      
      - uses: SharezoneApp/app-preview-action@v0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          android-artifact: "android-artifact"
```
