## Getting your changes to dev channel during cherry pick season

We batch up cherry picks and do pushes a number of times a week. if you have a cl that you think should make it to the current release, follow these steps.

First, please validate that your cl merges cleanly and that relevant tests pass:

```
git new-branch --upstream origin/dev merge_my_awesome
git cherry-pick #HASH_FROM_MASTER_THAT_NEEDS_CHERRY_PICKING
```

Then, build and run the tests. If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl to rietveld by doing git cl upload, link to the review from the merge request.

Then, please file a [merge request issue](https://goo.gl/A0WPwB)