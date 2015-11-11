There are several ways to contribute code in github, for example

1. forking and creating pull request from a forked repo (https://help.github.com/articles/fork-a-repo)
1. branching within the main repo, and doing pull requests from one branch to another.
1. working on a local branch, creating a code review using rietveld, push directly into master (this article).

## Step 1: Clone the project

    > mkdir components
    > git clone https://github.com/dart-lang/dart-web-components.git components

Tip: if you want to get automatic backups of an NFS directory, but take advantage of fast git performance put it on a local directory, you can get create the repo in NFS and use a mirror in a local directory, as follows:

    > cd ~/
    > mkdir components
    > git clone https://github.com/dart-lang/dart-web-components.git components
    > cd /usr/local/google/users/your_user_name/
    > git-new-workdir /home/your_user_name/dart/components/ components

Then work from the local directory. Files uncommitted are not backed up, but every commit you make is automatically backed up. The script 'git-new-workdir' is described in more detail here: http://nuclearsquid.com/writings/git-new-workdir/.
 
## Step 2: verify that git-cl is configured correctly.
 
This is a similar configuration to that used for code reviews on `dart_lang`. There is a `codereview.settings` file in the repo that would configure things automatically, but it never hurts to check:

    > git cl config
    Rietveld server (host[:port]) [https://chromiumcodereview.appspot.com/]:
    CC list ("x" to clear) [reviews@dartlang.org]:
    Tree status URL:
    ViewVC URL ("x" to clear) [https://github.com/dart-lang/dart-web-components/commit/]:

## Step 3: Create a branch for your new changes

Pick a branch name not existing locally nor in the remote repo, we recommend that you use your user name as a prefix to make things simpler.

    > cd /usr/local/google/users/your_user_name/components # the repo created above
    > git checkout -b uname_example                        # new branch

## Step 4: Do your changes and commit them locally in git

    > echo "file contents" > awesome_example.txt
    > git add awesome_example.txt
    > git commit -a -m "An awesome commit, for an awesome example."

## Step 5: Upload CL using 'git cl' (installed with gcl)

    > git cl upload origin/master

Then click on the `publish & mail` link to send email to the reviewers from the rietveld website.

## Step 6: Make code review changes and publish new versions of your code

    > echo "better file contents" > awesome_example.txt
    > git commit -a -m "An awesomer commit"
    > git cl upload origin/master

## Step 7: Sync up to latest changes

If new changes have been made to the repo, you need sync up to the new changes before submitting your code. You can do this in two ways:

There are two ways to sync up:
  * merging

        > git pull origin master
        > git cl upload origin/master

  * rebasing

        > git pull --rebase origin master
          (which is similar to first pull and merge in master, and then rebase:
            > git checkout master
            > git pull
            > git rebase master uname_example)
        > git cl upload origin/master

## Step 8: Submit your changes

We use git-cl also to submit, unlike the normal Dart repo (which lives on top of svn) we use 'git cl push', instead of 'git cl dcommit':

    > git cl land origin/master

This command will close the issue in Rietveld and submit your code directly on master.

## Step 9: Clean up the mess

After submitting, you can delete your local branch so that the repo is clean and tidy :)
 
    > git checkout master
    > git branch -D uname_example    # delete local branch

## Step 10: Goto step 3