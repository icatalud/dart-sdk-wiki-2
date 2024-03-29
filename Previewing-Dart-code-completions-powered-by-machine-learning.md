We have a new Dart developer productivity feature ready for preview. The feature augments traditional Dart code completion with new predictions based on machine learning. 

The predictions are based on a local model — powered by [TensorFlow Lite](https://www.tensorflow.org/lite) — so the predictions are made locally and offline. The training dataset is [open-source Dart code from GitHub](https://console.cloud.google.com/marketplace/details/github/github-repos).

## Required build
We recommend using the [Flutter dev channel, v1.10.0 or later](https://github.com/flutter/flutter/wiki/Flutter-build-release-channels), or a [Dart dev channel](https://dart.dev/tools/sdk/archive#dev-channel) if you want to preview it to ensure you get our most recent performance and stability improvements during the preview.

## Enabling the preview

### VS Code

* Enable the following settings in `settings.json` (this can be your global `settings.json`, or a workspace specific `settings.json`)
  * `"dart.analyzerAdditionalArgs": ["--enable-completion-model"]`<br>This enables the ML completions experiment.

  * `"editor.suggestSelection": "first".`<br>This ensures that Code shows the first available completion (Code has different default where it tried to remember past completions).

    ![image4](https://user-images.githubusercontent.com/13644170/64239870-4de0b600-cf01-11e9-9c2f-eb9acf261cd9.png)


### IntelliJ / Android Studio

* Open the **Registry* from the **Actions menu**

  ![image5](https://user-images.githubusercontent.com/13644170/64240092-a31cc780-cf01-11e9-96d6-fa2cc1452c52.png)


* Edit the **dart.server.additional.arguments** configuration value to include the command-line argument: `--enable-completion-model`

  ![image3](https://user-images.githubusercontent.com/535859/64433292-04b07380-d073-11e9-9655-b65c0dee5952.gif)


## Providing feedback
If you have feedback, please log a bug [using this link](https://github.com/dart-lang/sdk/issues/new?labels=area-analyzer,analyzer-completion&assignees=lambdabaa). When reporting a bug, please include the following:

1. Detailed repro steps
1. Version output from `flutter --version` / `dart --version`
1. A detailed description or screenshot of the completions seen
