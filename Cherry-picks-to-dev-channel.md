## Getting your changes to dev channel during cherry pick season

We batch up cherry picks and do pushes a number of times a week. For details, see the [branches and releases](https://github.com/dart-lang/sdk/wiki/Branches-and-releases) page.

If you have a cl that you think should make it to the current release, follow these steps.

Generally cherry picks should be limited to the following categories:

1. Bugs that have introduced regressions from the last stable release
2. Critical bugs in new features introduced in the current release

### Step 1
First, please validate that your cl merges cleanly and that relevant tests pass:

```console
$ git fetch
$ git new-branch --upstream origin/dev merge_my_awesome
$ git cherry-pick #HASH_FROM_MASTER_THAT_NEEDS_CHERRY_PICKING
```

### Step 2
Then, build and run the tests. 

If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl to rietveld by doing `git cl upload`, and link to the review in the merge request.

### Step 3
Finally, please file a [merge request issue][new-template].

[new-template]: https://github.com/dart-lang/sdk/issues/new?title=Please%20merge%20%23HASH%20into%20dev%20channel&labels=merge-to-dev&body=%2A%2Acommit%28s%29+to+merge%2A%2A%3A+COMMIT-HASH%0A%0A%2A%2Amerge+instructions%2A%2A%3A+clean+merge+or+patch+CL%0A%0A%2A%2Areason%2A%2A%3A+a+sentence+or+two.%0A%0A%2Fcc+%40dgrove+%40kevmoo+%40mit-mit+%40whesse+%40athomas+%0A%0A--------%0AThank+you+for+filing+a+merge-to-dev+request%2C+please%3A%0A++%2A+fill+in+the+data+above%0A++%2A+set+a+milestone+and+area+label+%28e.g.+area-dart2js+for+a+dart2js+fix%29+%0A++%2A+attempt+to+do+the+merge+%28%60git+checkout+origin%2Fdev%3B+git+cherry-pick+HASH%60%29+to+ensure+the+merge+instructions+are+accurate.%0A++%2A+delete+this+comment+and+examples+%3A%29%0A%0AEXAMPLE+1%3A%0A%3E+%2A%2Acommit+to+merge%2A%2A%3A+a7e511f+%28Should+match+the+hash+in+the+title.+It%27s+OK+to+have+more+than+one+commit%2C+but+please+explain.%29%0A%3E%0A%3E+%2A%2Amerge+instructions%2A%2A%3A+merges+cleanly+in+%60origin%2Fdev%60%0A%3E%0A%3E+%2A%2Areason%2A%2A%3A+fixes+bug+with+nsm-forwarders+with+named+arguments.+Affects+customers+unit+tests+that+use+mockito.%0A%0AEXAMPLE+2%3A%0A%3E+%2A%2Acommit+to+merge%2A%2A%3A+a7e511f%0A%3E%0A%3E+%2A%2Amerge+instructions%2A%2A%3A+patch+%5BCL%2Fxyz%5D%28link%29+%28merge+had+conflicts%2C+CL+resolves+them%29.%0A%3E%0A%3E+%2A%2Areason%2A%2A%3A+fixes+bug+with+nsm-forwarders+with+named+arguments.+Affects+%3E+customers+unit+tests+that+use+mockito.
[old-template]: https://goo.gl/NmYzrr