Because the sdk repository on GitHub is a mirror of dart.googlesource.com/sdk.git,
pull requests at GitHub should not be landed directly on the GitHub repository.
Merging pull requests is disabled for everyone except administrators, but they
should not merge pull requests either.

We are working on a system that automatically creates Gerrit reviews at
dart-review.googlesource.com from GitHub pull requests, and will update
this page when it is completed.

Until then, you can check out a pull request branch locally, and make a
Gerrit CL with the "git cl upload" command, or contact dart-engprod@google.com
