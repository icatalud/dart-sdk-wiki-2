We have a rather simple branch setup for the dart project. Normally, everything is developed on the master branch. Features that are not yet ready for prime time are hidden under a flag, and enabled when sufficient stability has been proven. We occasionally create feature branches for landing big and disruptive changes. In addition to that, we have our dev and stable branches used for releasing the sdk.

## Release cycle
Our normal release cycle is roughly 2 months long, but we don't make any guarantees, i.e., we may ship early if we feel that the stability is good or we may ship late if it is not. We don't normally follow a feature driven release cycle, but in some cases for larger changes to the language we may postpone a release to get all tools in sync.

On the normal 2 month cycle we use the first ~6 weeks doing full merges of master to dev, basically releasing a green build from master on dev roughly once a week. In case of bugs found quickly we do a patch release or another full push.

We then spend the last ~3 weeks stabilizing the dev channel, only cherry picking critical fixes. People continue working on the master with normal changes. In this 3 week period we do multiple batches of cherry picks a week, and release those on dev as we have them build. We call this cherry-pick season.

Once we have something that looks good we merge it to stable and release it there. During the 2 months we do patch security, crash and critical bug releases on stable based on the last stable version.

## Getting your changes to dev channel during cherry pick season

We batch up cherry picks and do pushes a number of times a week. if you have a cl that you think should make it to the current release, follow these steps.

First, please validate that your cl merges cleanly and that relevant tests pass:

```
git new-branch --upstream origin/dev merge_my_awesome
git cherry-pick #HASH_FROM_MASTER_THAT_NEEDS_CHERRY_PICKING
```

Then, build and run the tests. If it does not merge cleanly please supply a patch by fixing merge conflicts locally and uploading a cl to rietveld by doing git cl upload, link to the review from the merge request.

Then, please file a [merge request issue](https://github.com/dart-lang/sdk/issues/new?title=Please%20merge%20revision%20%23HASH%20into%20dev%20channel&body=@ricowind%20@whesse%20@kasperl%0A%3CFill%20in%20the%20labels%20for%20Milestone%20and%20Area%20to%20indicate%20what%20the%20merge%20applies%20to.%20E.g.%20if%20you%27re%20requesting%20a%20merge%20to%20fix%20an%20issue%20in%20Dart2JS,%20set%20the%20area%20to%20Area-Dart2JS.%20If%20this%20is%20a%20request%20to%20merge%20to%20stable%20please%20change%20the%20MergeToDev%20label%20to%20MergeToStable%3E%0A%0A%3CDescribe%20the%20problem%20this%20merge%20is%20fixing%20-%20include%20issue%20numbers%20if%20applicable%3E%0AEXAMLE:%20This%20fixes%20the%20promote%20script%20to%20not%20assume%20integer%20revisions%20after%20our%20github%20move%0A%0A%3CWhat%20revision(s)%20needs%20to%20be%20merged%20-%20please%20annotate%20revisions%20with%20reason%20if%20more%20than%20one%3E%0AEXAMLE:%206030dd9015dc11208d630c1739763f448e5332d7%0A%0A%3CPlease%20try%20out%20the%20merge%20yourself.%20If%20there%20are%20merge%20conflicts%20please%20resolve%20these%20and%20upload%20the%20cl%20to%20rietveld%20and%20provide%20a%20link%20here%3E%0AEXAMPLE:%20This%20merged%20cleanly&labels=MergeToDev)


## Pushing to dev
Find a suitable green build on master, by looking at the bots, we call this #MASTER_HASH_TO_BASE_RELEASE_OFF. We call the version we are pushing $THE_VERSION_BEING_PUSHED, this is based on the actual values in tools/VERSION, but looks like this "1.11.0-dev.3.0". 
```
git new-branch --upstream origin/dev release
git merge --no-commit #MASTER_HASH_TO_BASE_RELEASE_OFF
```
Update tools/VERSION (for these full pushes reset PRERELEASE_PATCH to 0, increase PRERELEASE by 1, the updated version is your $THE_VERSION_BEING_PUSHED)
```
git commit -a
```
Update the commit message, it should read like:
```
Version $THE_VERSION_BEING_PUSHED

Merge commit '#MASTER_HASH_TO_BASE_RELEASE_OFF' into dev
```
Note the s/release/dev in the last line

Sanity check:
```
git diff #MASTER_HASH_TO_BASE_RELEASE_OFF
```
should only give differences in the tools/VERSION file.

Push the changes:
```
git push
```
Tag the new release:
```
git tag -a $THE_VERSION_BEING_PUSHED -m $THE_VERSION_BEING_PUSHED
git push --tags
```

## Cherry picking to dev
Please don't just cherry pick your changes to dev, we normally batch these up. Instead file a merge request (see above).

We do this using two branches to get a nice looking history on the dev branch, and to not trigger builds with non working versions on the bots (the bot git poller will do git log --first-parent when figuring out commits.

$THE_VERSION_BEING_PUSHED has the same meaning as above. The #HASH_1 ... #HASH_N are the cls we want to cherry pick
```
git new-branch --upstream origin/dev cherry
git cherry-pick #HASH_1
.
.
.
git cherry-pick #HASH_N
```
Now, we have all the cherry picks on the cherry branch, create the branch we will use for pushing, merge the cherry picks in (to get a merge node similar to what we have for normal full pushes). We use --no-ff to get the merge node instead of fast forward commits.
```
git new-branch --upstream origin/dev dev
git merge --no-ff --no-commit cherry
```
Edit tools/VERSION, increase PRERELEASE_PATCH by 1
```
git commit -a
```
Commit message should read:
```
Version $THE_VERSION_BEING_PUSHED

Cherry-pick #HASH_1 to dev
.
.
.
Cherry-pick #HASH_N to dev
```
Sanity check the log, make sure all the cherry picks are there and that the Version commit has the right structure.

Push the changes:
```
git push
```

Tag the new release:
```
git tag -a $THE_VERSION_BEING_PUSHED -m $THE_VERSION_BEING_PUSHED
git push --tags
```
# Stable branch
TO come, we did not branch for stable yet