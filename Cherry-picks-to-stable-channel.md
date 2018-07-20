The **Stable channel** is where we release our stable releases. It is produced from the [stable branch](https://github.com/dart-lang/sdk/blob/stable/tools/VERSION).

We differentiate between two types of stable releases:

  * **Stable feature releases**:
    * For example, 1.21.0.
    * Our main new feature releases.
    * They contain new capabilities, and a larger amount of bug fixes.

  * **Stable patch releases**:
    * For example, 1.21.2.
     * Patches to feature releases.
     * They contain no new capabilities, and a very small amount of bug fixes.

The [Dart downloads page](https://www.dartlang.org/install/archive) lists the latest stable release (i.e., feature or patch release).

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

If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl by doing `git cl upload`, and link to the review in the merge request.

### Step 4
Finally, please file a [merge to stable request issue][new-template].

[new-template]: https://github.com/dart-lang/sdk/issues/new?title=Please%20merge%20%23HASH%20into%20stable%20channel&labels=merge-to-stable&body=%2A%2Acommit%28s%29+to+merge%2A%2A%3A+COMMIT-HASH%0A%0A%2A%2Amerge+instructions%2A%2A%3A+clean+merge+or+patch+CL%0A%0A%2A%2Areason%2A%2A%3A+a+sentence+or+two.%0A%0A%2Fcc+%40dgrove+%40kevmoo+%40mit-mit+%40whesse+%40athomas+%0A%0A--------%0AThank+you+for+filing+a+merge-to-stable+request%2C+please%3A%0A++%2A+fill+in+the+data+above%0A++%2A+set+a+milestone+and+area+label+%28e.g.+area-dart2js+for+a+dart2js+fix%29+%0A++%2A+attempt+to+do+the+merge+%28%60git+checkout+origin%2Fstable%3B+git+cherry-pick+HASH%60%29+to+ensure+the+merge+instructions+are+accurate.%0A++%2A+delete+this+comment+and+examples+%3A%29%0A%0AEXAMPLE+1%3A%0A%3E+%2A%2Acommit+to+merge%2A%2A%3A+a7e511f+%28Should+match+the+hash+in+the+title.+It%27s+OK+to+have+more+than+one+commit%2C+but+please+explain.%29%0A%3E%0A%3E+%2A%2Amerge+instructions%2A%2A%3A+merges+cleanly+in+%60origin%2Fstable%60%0A%3E%0A%3E+%2A%2Areason%2A%2A%3A+fixes+bug+with+nsm-forwarders+with+named+arguments.+Affects+customers+unit+tests+that+use+mockito.%0A%0AEXAMPLE+2%3A%0A%3E+%2A%2Acommit+to+merge%2A%2A%3A+a7e511f%0A%3E%0A%3E+%2A%2Amerge+instructions%2A%2A%3A+patch+%5BCL%2Fxyz%5D%28link%29+%28merge+had+conflicts%2C+CL+resolves+them%29.%0A%3E%0A%3E+%2A%2Areason%2A%2A%3A+fixes+bug+with+nsm-forwarders+with+named+arguments.+Affects+%3E+customers+unit+tests+that+use+mockito.
[old-template]: https://goo.gl/vcmz7o
