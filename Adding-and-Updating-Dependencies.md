# Introduction

Since Dart is an open-source project we rely on a number of other open-source projects, as tools
for building and testing, and as libraries compiled into the Dart SDK. Please follow these guidelines when working with third-party dependencies in both source and binary form.


# Third-party source dependencies

When adding new third party source dependencies please be very careful. Make sure that the license for the software is compatible with our license - a large set of licenses are more restrictive when you actually link in the source instead of just using the tools.
For example, a compiler's license might allow using it to compile our binary, but could be too restrictive to allow using its source code as part of the source code for a Dart SDK tool.
You must add a README.google if you check in third-party code, and note any local changes that have been applied to the code (see below for a template). Please cc whesse@google.com on any third-party additions. 

If you need to discuss license issues to make sure that a license is compatible please reach out to the legal team at google or talk with whesse@google.com who can guide you to the right
person. Please refrain from speculating about licenses or make assumptions about what you may and may not do. If in doubt you should ask. If you are not a Google employee contact a Dart team member who is.

We have a few rules to follow,
to make sure that we can continue to build older versions of the sdk even if people delete or rewrite the history of repositories containing our third-party dependencies. There are two options for using repositories not hosted under https://github.com/dart-lang/ or https://github.com/google/
The first option is to simply get the repo moved to the dart-lang org, or fork it there. Forking an repo is not very nice since you get fragmented versions. If you do so, please state in the README that this is used for pulling as a DEPS, don't make local changes, and be sure not to publish to pub from the dart-lang fork.

The second option is to get the github repo mirrored on the chromium git servers, and pull the dependency from there. This is the same way we do normal dependencies from dart-lang (see below), with one exception:
**IMPORTANT**: For security reasons we require all code that we pull in to build the Dart SDK be reviewed by Google employees, no matter where it comes from. This means that you, or a Google employee, need to do an initial review of all code up to and including the commit you put in the DEPS file for all external packages.

All external packages must be pinned to a fixed commit (revision) in the DEPS file. If you update the DEPS to pin a newer version, you need to do another review, of the changes between the two revisions. You can upload the review (of the changes in the dependency, not just the change in the DEPS file) to https://codereview.chromium.org for easy reviewing, and upstream any fixes in this dependency, before pinning the new version of the dependency.

For dependencies on https://github.com/dart-lang/ the setup is less complicated, but we still need a mirror. Go ahead and add the dependency to the DEPS file for local development, but please add the dependency directly on http://github.com/dart-lang/REPO.git, don't use the the github_mirror variable from the DEPS file. You can file an issue requesting a mirror on github. Give the issue the label 'area-infrastructure'. The mirrors go to https://chromium.googlesource.com/external/github.com/dart-lang/REPO.git. When the mirror is set up, commit your changes to the DEPS file pointing at the github mirror.

For dependencies on non-dart-lang github repositories, please get the mirror up before adding the dependency to the DEPS file, and use the mirror to pull from.

# Third-party binaries and binary data in general

We occasionally need add binary versions of tools to make our testing/build/distribution infrastructure easier to maintain. These can be tools derived from our own source code (like fixed stable versions of the Dart standalone binary), or can be open source tools (like the standalone Firefox binary). Please be absolutely sure that the binary is compatible with our license. If your binary is not needed for building/distributing the core dart tool-chain you should consider other options (e.g., lazily fetching the files from the source or from Google Cloud Storage). In any case, always be vigilant about the license and please cc whesse@google.com on any third party additions.

# Where do they go
We put third party binaries on Google Cloud Storage and fetch them using a DEPS hook. We pin the DEPS entries to the hashes of specific versions, and we never delete old entries from the bucket where they are located, since that would destroy our ability to do old builds. This allows us to only have a single location holding the binaries for bleeding edge, dev branch, and stable releases.

Uploading binary data is done using upload_to_google_storage.py (http://src.chromium.org/svn/trunk/tools/depot_tools/upload_to_google_storage.py )
and a corresponding hook is added to the DEPS file. Please note that only a select set of people have access to uploading. If you feel that you need access please contact whesse@

# README.google templates

When adding third party dependencies you must always add a README.google file describing what you put in, which license, which revision if applicable, the repository/page where you obtained the source/binary and any local modifications. You should also always make sure that there is a LICENSE file with the actual license. The following is a verbatim copy of the README.google for the firefox javascript shell, you can use that as a template.

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
