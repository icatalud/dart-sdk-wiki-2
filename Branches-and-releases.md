We have a rather simple branch setup for the dart project. Normally, everything is developed on the master branch. Features that are not yet ready for prime time is hidden under a flag, and enabled when sufficient stability has been presented. We do occasionally create feature branches for landing big and disruptive changes. In addition to that, we have our dev and stable branches used for releasing the sdk.

## Release cycle
Our normal release cycle is roughly 2 months long, but we don't make any guarantees, i.e., we may ship early if we feel that the stability is good or we may ship late if it is not. We don't normally follow a feature driven release cycle, but in some cases for larger changes to the language we may postpone a release to get all tools in sync.

On the normal 2 month cycle we use the first ~6 weeks doing full merges of master to dev, basically releasing a green build from master on dev roughly once a week. In case of bugs found quickly we do a patch release or another full push.

We then spend the last ~3 weeks stabilizing the dev channel, only cherry picking critical fixes. People continue working on the master with normal changes. In this 3 week period we do multiple batches of cherry picks a week, and release those on dev as we have them build.

Once we have something that looks good we merge it to stable and release it there. During the 2 months we do patch security, crash and critical bug releases on stable based on the last stable version.

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
Update commit message, should read:
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
$THE_VERSION_BEING_PUSHED has the same meaning as above. The #HASH_1 ... #HASH_N are the cls we want to cherry pick
```
git new-branch --upstream origin/dev release
git cherry-pick #HASH_1
.
.
.
git cherry-pick #HASH_N
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
