## Branches and work flows

We have a rather simple branch setup for the dart project. Normally, everything is developed on the master branch. Features that are not yet ready for prime time are hidden under a flag, and enabled when sufficient stability has been proven. We occasionally create feature branches for landing big and disruptive changes. In addition to that, we have our dev and stable branches used for releasing the sdk.

In summary, we have in three main branches:

   * **[master](https://github.com/dart-lang/sdk/blob/master/tools/VERSION)**:
     Used for "everyday" development; land your CLs here. 

   * **[dev](https://github.com/dart-lang/sdk/blob/dev/tools/VERSION)**:
     Populated from the master branch via full pushes, usually weekly, or via [cherry picks](https://github.com/dart-lang/sdk/wiki/Cherry-picks-to-dev-channel). Don't land cls here. Released via [dev channel builds](https://www.dartlang.org/install/archive#dev-channel).

   * **[stable](https://github.com/dart-lang/sdk/blob/stable/tools/VERSION)**:
     Our main release branch, populated from the dev branch via full pushes when a release is ready, or via [cherry picks](https://github.com/dart-lang/sdk/wiki/Cherry-picks-to-stable-channel). Don't land cls here. Released via [stable channel builds](https://www.dartlang.org/install/archive#stable-channel).


## Release cycle
Our normal release cycle is roughly 2 months long, but we don't make any guarantees, i.e., we may ship early if we feel that the stability is good or we may ship late if it is not. We don't normally follow a feature driven release cycle, but in some cases for larger changes to the language we may postpone a release to get all tools in sync.

On the normal 2 month cycle we use the first ~6 weeks doing full merges of master to dev, basically releasing a green build from master on dev roughly once a week. In case of bugs found quickly we do a patch release or another full push.

We then spend the last ~3 weeks stabilizing the dev channel, only cherry picking critical fixes. People continue working on the master with normal changes. In this 3 week period we do multiple batches of cherry picks a week, and release those on dev as we have them build. We call this cherry-pick season.

Once we have something that looks good we merge it to stable and release it there. During the 2 months we do patch security, crash and critical bug releases on stable based on the last stable version.

## Dev branch

### Getting your changes to dev channel during cherry pick season

See the [cherry pick to dev](https://github.com/dart-lang/sdk/wiki/Cherry-picks-to-dev-channel) page for all the details on how to get a change cherry picked.

### Pushing to dev
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

### Cherry picking to dev
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
## Stable branch
The way we handle stable releases is more or less equal to the way we handle dev. When we have a good dev channel build after 3 weeks of cherry-picking, we merge that over to the stable branch:

### Getting your changes to a new stable patch release

First of, before doing any patch releases on stable the commits should already have been on dev for a number of days without reported issues (preferably more than a week). Also, before releasing it, we need more manual validation and sanity checking. Please send out a mail internally requesting for feedback and ask the people responsible for the patches going in to validate functionality.

We normally simply cherry pick the changes from master, and there is no difference in how we do this compared to dev channel, except, we don't increase PRERELEASE_PATCH, we increase PATCH in the tools/VERSION file.

Once you are ready, create a cherry pick issue, see [Cherry picks to stable](https://github.com/dart-lang/sdk/wiki/Cherry-picks-to-stable-channel).

### Doing a new full stable build
Assume that the dev channel build we want to base this of is #DEV_HASH_TO_BASE_RELEASE_OFF
```
git new-branch --upstream origin/stable release
git merge --no-commit #DEV_HASH_TO_BASE_RELEASE_OFF
```
There may very well be merge conflicts if we did patch releases on stable in the prior release. Always just solve these by taking the dev version. Because of this, always do a sanity check:
```
git diff #DEV_HASH_TO_BASE_RELEASE_OFF
```
This should **only** give a difference in the version file. If not, checkout the file(s) from #DEV_HASH_TO_BASE_RELEASE_OFF.

Update tools/VERSION (for these full pushes reset PRERELEASE_PATCH to 0, reset PRERELEASE to 0, reset PATCH to 0, set MINOR to the version from DEV (i.e., if we are doing this to release 1.12.0 set it to 12). As before, we call the version $THE_VERSION_BEING_PUSHED, on stable this is more simple, e.g., 1.12.0.

```
git push
```
Tag the new release:
```
git tag -a $THE_VERSION_BEING_PUSHED -m $THE_VERSION_BEING_PUSHED
git push --tags
```

Finally, make sure everything on the [release checklist](https://github.com/dart-lang/sdk/wiki/Release-checklist) is completed.
