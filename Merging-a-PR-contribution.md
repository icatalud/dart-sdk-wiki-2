# Global aliases

Add these to the `[alias]` section of your [Git global config].

[Git global config]: https://git-scm.com/book/tr/v2/Customizing-Git-Git-Configuration

```
[alias]
  pl = log --pretty=format:'%h %s' --graph
  pr = "!f() { git fetch origin refs/pull/$1/head:pr/$1; } ; f"
```

# Cleanly merging in pull requests

1. Set up the `pr` alias from above
1. Start on the master branch. Make sure you've updated to latest.
1. Let’s say we’re dealing with Pull Request #42
1. Run `git pr 42`
    * You’ll notice a new branch created `pr/42`
1. Checkout `pr/42`
    * `git checkout p/42`
1. Rebase `pr/42` on top of master. This makes sure the history is replayed on latest.
    * `git rebase origin/master
1. Now checkout `master` and rebase to `pr/42`
    * `git checkout master`
    * `git rebase pr/42`
    * This updates your local `master` branch to be the same as `pr/42`
1. Push to origin.
    * `git push`