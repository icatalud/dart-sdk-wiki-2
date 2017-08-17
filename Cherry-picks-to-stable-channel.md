The **Stable channel** is where we release our stable releases. It is produced from the [stable branch](https://github.com/dart-lang/sdk/blob/stable/tools/VERSION).

We differentiate between two types of stable releases:

   * Stable feature release: For example, 1.21. These are our main new feature releases, and contain new capabilities, and a larger amount of bug fixes.
   * Stable patch release: For example, 1.21.2. These are patches to feature releases, and contain no new capabilities, and a very small amount of bug fixes.

The [Dart downloads page](https://www.dartlang.org/install/archive) lists the latest stable release (i.e., either the latest major release, of if available the last stable patch release).

## Getting your changes to a new stable patch release

We batch up cherry picks and do pushes only when required. Stable patch releases are intended only to be used for

   1. Urgent security fixes
   1. Critical customer blocking issues

If you have a fix that resolves one of these, follow these steps.

### Step 1

Resolve the issue and land it as a cl against the master branch. Validate that the issue was resolved in master.

### Step 2
Check that your cl merges cleanly to the stable branch:

```console
$ git fetch
$ git new-branch --upstream origin/stable merge_my_awesome
$ git cherry-pick #HASH_FROM_MASTER_THAT_NEEDS_CHERRY_PICKING
```

### Step 3
Then, build and run the tests, and ensure that all tests are passing. 

If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl to rietveld by doing `git cl upload`, and link to the review in the merge request.

### Step 4
Finally, please file a [merge to stable request issue](https://goo.gl/vcmz7o).