## Getting your changes to dev channel during cherry pick season

We batch up cherry picks and do pushes a number of times a week. For details, see the [branches and releases](https://github.com/dart-lang/sdk/wiki/Branches-and-releases) page.

If you have a cl that you think should make it to the current release, follow these steps.

Generally cherry picks should be limited to the following categories:

1. Bugs that have introduced regressions from the last stable release
2. Critical bugs in new features introduced in the current release

###Step 1
First, please validate that your cl merges cleanly and that relevant tests pass:

```
git new-branch --upstream origin/dev merge_my_awesome
git cherry-pick #HASH_FROM_MASTER_THAT_NEEDS_CHERRY_PICKING
```

###Step 2
Then, build and run the tests. 

If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl to rietveld by doing `git cl upload`, and link to the review in the merge request.

###Step 3
Finally, please file a [merge request issue](https://goo.gl/NmYzrr).