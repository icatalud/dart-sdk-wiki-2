# Introduction

Since dart is an open-source project we rely on a number of other open-source projects, both for integrating into our core tool chain and as utilities in compilation, testing and distribution. The following is a general set of guidelines for working with third-party dependcies in both source and binary form.


# Third-party source dependencies

When adding new third party source dependencies please be very careful. Make sure that the license for the software is compatible with our license - a large set of licenses are more restrictive when you actually link in the source instead of just utilizing the tools.
As an example using a compiler to produce a binary could in some cases be ok, while linking the source of the compiler into our code may not.
Please do add a README.google if you check in third-party code, and please note any local changes that have been applied to the code (see below for a template). If you add a dependency through an addition to the DEPS files all the same rules applies, you need to make sure that the license is compatible.  Please cc ricow@google.com on any third-party additions. If you need to discuss license issues to make sure that is is compatible please reach out to the legal team at google or talk with ricow@ who can guide you to the right
person. Please refrain from speculating about licenses or make assumptions about what you may and may not do, if in doubt you should ask. If you are not a Googler contact a Dart team member who is.

For DEPS pulled dependencies we have a few rules to follow.
To make sure that we can continue to build older versions of the sdk even in the case of people deleting or rewriting history of dependent repositories, there are two options for using repositories not hosted under https://github.com/dart-lang/ or https://github.com/google/
First option is to simply get the repo moved to the dart-lang org, or fork it there. This is normally not very nice since you get fragmented versions. If you do so, please state in the README that this is a for used for pulling as a DEPS, and don't publish conflicting versions to pub.

Second option is to get the github repo mirrored on the chromium git servers, and pull the dependency from there. This is the same way we do normal dependencies from dart-lang (see below), with one exception:
IMPORTANT: For security reasons we require all code that we pull in to build the sdk be reviewed by googlers no matter where it lives. This means that you need to do an initial review of all code up to and including the commit you put in the DEPS file for all external packages. If you roll the DEPS you need to do another review of the delta. You can upload the review to rietveld for easy reviewing and upstream any fixes before pulling the dependency.

For dependencies on https://github.com/dart-lang/ the setup is less complicated, but we still need a mirror. Go ahead and add the dependency to the DEPS file, but please add the dependency directly on http://github.com/dart-lang/REPO.git, don't use the the github_mirror var. You can file a bug against chrome infra or ask ricow@ for help on the mirror. The mirrors go to https://chromium.googlesource.com/external/github.com/dart-lang/REPO.git
We pull from the mirrors to have consistency between what devs see and what the bots see (and we do need the mirrors to not have 100+ bots constantly pull github)

# Third-party binaries and binary data in general

We occasionally need add binary versions of tools to make our testing/build/distribution infrastructure easier to maintain. This can be tools derived from our own source code (like fixed stable versions of the dart standalone binary) or can be open source tools (like the standalone firefox binary). Please be absolutely sure that the binary is compatible with our license. If your binary is not needed for building/distributing the core dart tool-chain you should consider other options (e.g., lazy fetching the files from the source or from Google Cloud Storage). In any case, always be vigilant about the license and please cc ricow@google.com on any third party additions.

# Where do they go
We put third party binaries on google cloud storage and fetch them using a DEPS hook. We restrict the DEPS entries to specific hash, and we never delete old entries from the bucket where they are located (since that would destroy our ability to do old builds). This allows us to only have a single entry point for both bleeding edge, trunk and branches.

Uploading binary data is done using upload_to_google_storage.py (http://src.chromium.org/svn/trunk/tools/depot_tools/upload_to_google_storage.py )
and a corresponding hook is added to the DEPS file. Please note that only a select set of people have access to uploading. If you feel that you need access please contact ricow@

# README.google templates

When adding third party dependencies you should always add a README.google file describing what you put in, which license, which revision if applicable, the repository/page where you obtained the source/binary and any local modifications. You should also always make sure that there is a LICENSE file with the actual license. The following is a verbatim copy of the README.google for the firefox javascript shell, you can use that as a template.

```
Name: Firefox command line javascript shell.
Short Name: js-shell
URL: http://ftp.mozilla.org/pub/mozilla.org/firefox/candidates/15.0.1-candidates/build1/
Version: 15.0.1
Date: September 5th 2012
License: MPL, http://www.mozilla.org/MPL

Description:
This directory contains the firefox js-shell binaries for Windows, Mac and
Linux. The files was fetched from:
http://ftp.mozilla.org/pub/mozilla.org/firefox/candidates/15.0.1-candidates/
on September 28th 2012.
The binaries are used for testing dart code compiled to javascript.
```
