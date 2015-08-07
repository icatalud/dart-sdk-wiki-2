## Where are they

Archived builds of the SDK and Dartium, and other products, are stored in Google cloud storage in the dart-archive bucket.  These can be browsed with the (unsupported) gsdview app, at [http://gsdview.appspot.com/dart-archive/channels/](http://gsdview.appspot.com/dart-archive/channels/).

The supported way to access and download a file is with the Google storage api, but this does not let you browse directories easily.  For example, the latest Windows SDK from bleeding-edge (the master branch on github) is downloaded from
https://dart-archive.storage.googleapis.com/channels/dev/raw/latest/sdk/dartsdk-windows-x64-release.zip.

It is possible to browse a directory with the storage api, getting an XML result listing the contents, by adding the delimiter and prefix parameters to the query:
https://dart-archive.storage.googleapis.com/?delimiter=/&prefix=channels/dev/raw/. You can even filter on the beginning of the filenames or subdirectory names by putting it in prefix; the following link finds the revisions beginning with '1.11': https://dart-archive.storage.googleapis.com/?delimiter=/&prefix=channels/dev/raw/1.11.

## Naming scheme

Dev channel and stable channel builds are uploaded using their version as name, starting with version 1.11.0, and earlier versions are labeled with SVN revision numbers.  The released versions are copied to the "release" subdirectory, and all uploaded builds are in the "raw" subdirectory. With the dropping of the standalone editor, in favor of plugins, the "signed" directory is no longer used.

Bleeding-edge builds are uploaded using a artificial revision number, which is the count of git commits in the commit's history, plus 100000.  To find the revision number of a commit with hash $HASH, run the command

    git rev-list $HASH --count

in a git checkout containing the commit, then add 100000.

The hash can be found from a revision number by downloading the VERSION file from channels/be/raw/[revision]/VERSION, and opening it.

## Which commits are in which versions?

The commit message for each full push to dev includes the hash of the last bleeding-edge commit merged to it.  This hash can then be used to find the revision number for the bleeding-edge archived builds.  There are other ways to see which commits are in which version.  The tool gitk, a graphical interface to git, will show lots of information about each commit, including which branches it is on, and can show the commits between two tags:

    gitk 1.12.0-dev.4.0..1.12.0-dev.5.0

will show all the commits between those two dev channel versions.

Selecting any commit in any gitk view will show a "Precedes:" field for it, which shows the first tagged build containing it, which in our case is the first dev version containing it.  This is equivalent to the  command

    git describe --contains $HASH

which shows the first tag containing $HASH.
