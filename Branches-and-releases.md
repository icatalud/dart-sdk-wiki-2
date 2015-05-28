We have a rather simple branch setup for the dart project. Normally, everything is developed on the master branch. Features that are not yet ready for prime time is hidden under a flag, and enabled when sufficient stability has been presented. We do occasionally create feature branches for landing big and disruptive changes. In addition to that, we have our dev and stable branches used for releasing the sdk.

## Release cycle
Our normal release cycle is roughly 2 months long, but we don't make any guarantees, i.e., we may ship early if we feel that the stability is good or we may ship late if it is not. We don't normally follow a feature driven release cycle, but in some cases for larger changes to the language we may postpone a release to get all tools in sync.
On the normal 2 month cycle we use the first ~6 weeks doing full merges of master to dev, basically releasing a green build from master on dev roughly once a week. In case of bugs found quickly we do a patch release or another full push.
We then spend the last ~3 weeks stabilizing the dev channel, only cherry picking critical fixes. People continue working on the master with normal changes. In this 3 week period we do multiple batches of cherry picks a week, and release those on dev as we have them build.
Once we have something that looks good we merge it to stable and release it there. During the 2 months we do patch security, crash and critical bug releases on stable based on the last stable version.

## Pushing to dev
Can be done by doing
```
git new-branch --upstream origin/dev release
git merge --no-commit #MASTER_HASH_TO_BASE_RELEASE_OFF
```
Update tools/VERSION (for these full pushes reset PRERELEASE_PATCH to 0, increase PRERELEASE by 1)
```
git commit -a
```
# Update commit message, should read:
```
Version $THE_VERSION_BEING_PUSHED

Merge commit '#MASTER_HASH_TO_BASE_RELEASE_OFF' into dev
```
Note the s/release/dev in the last line
