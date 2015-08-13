We have a rather simple branch setup for the dart project. Normally, everything is developed on the master branch. Features that are not yet ready for prime time are hidden under a flag, and enabled when sufficient stability has been proven. We occasionally create feature branches for landing big and disruptive changes. In addition to that, we have our dev and stable branches used for releasing the sdk.

## Release cycle
Our normal release cycle is roughly 2 months long, but we don't make any guarantees, i.e., we may ship early if we feel that the stability is good or we may ship late if it is not. We don't normally follow a feature driven release cycle, but in some cases for larger changes to the language we may postpone a release to get all tools in sync.

On the normal 2 month cycle we use the first ~6 weeks doing full merges of master to dev, basically releasing a green build from master on dev roughly once a week. In case of bugs found quickly we do a patch release or another full push.

We then spend the last ~3 weeks stabilizing the dev channel, only cherry picking critical fixes. People continue working on the master with normal changes. In this 3 week period we do multiple batches of cherry picks a week, and release those on dev as we have them build. We call this cherry-pick season.

Once we have something that looks good we merge it to stable and release it there. During the 2 months we do patch security, crash and critical bug releases on stable based on the last stable version.

## Getting your changes to dev channel during cherry pick season

See the [cherry pick to dev](https://github.com/dart-lang/sdk/wiki/Cherry-picks-to-dev-channel) page for all the details on how to get a change cherry picked.

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