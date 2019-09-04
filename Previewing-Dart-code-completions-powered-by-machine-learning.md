We have a new Dart developer productivity feature ready for preview. The feature augments traditional Dart code completion with new predictions based on machine learning. 

The predictions are based on a local model — powered by TensorFlow Lite — so the predictions are made locally and offline. The training dataset is Open-source Dart code from GitHub through BigQuery public datasets.


## Required build
We recommend using 

## Enabling the preview

### VS Code

* Enable the following settings in settings.json (this can be your global settings.json, or a workspace specific settings.json)
  * `"dart.analyzerAdditionalArgs": ["--enable-completion-model"]`<br>This enables the ML completions experiment.

  * `"editor.suggestSelection": "first".`<br>This ensures that Code shows the first available completion (Code has different default where it tried to remember past completions).

### IntelliJ / Android Studio

* Open the **Registry* from the **Actions menu**

* Edit the **dart.server.additional.arguments** configuration value to include the command-line argument: `--enable-completion-model`


## Providing feedback
If you have feedback, please log a bug [using this link](https://github.com/dart-lang/sdk/issues/new?labels=area-analyzer,analyzer-completion&assignees=lambdabaa). When reporting a bug, please include the following:

1. Detailed repro steps
1. Version output from flutter --version / dart --version
1. A detailed description or screenshot of the completions seen
