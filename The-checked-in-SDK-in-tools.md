## Purpose

Building the Dart observatory and running tests require a prebuilt Dart SDK, that is downloaded into the build checkout as part of the checkout process.  The SDK is downloaded into tools/sdks/[linux,win,mac]/dart-sdk from a gzipped tar archive in cloud storage by the download_from_google_storage script in depot_tools.  It will usually be a recently released stable version of Dart.

## Warning

Before introducing any steps in the build process that depend on this SDK, file an issue with label Area-Infrastructure, as this creates a circular dependency that prevents porting Dart to new architectures and platforms.  The observatory build has a backup method not depending on this SDK for this reason.

## Architectures

On the linux OS, multiple processor architectures are supported by adding (stripped) binaries of the Dart executable for those architectures, with names like dart-mips and dart-arm, to tools/sdks/linux/dart-sdk/bin.  The default binary, called dart, is compiled for ia32, and is also used on x64 platforms.
To use the downloaded SDK, the function CheckedInSdkFixedExecutable() in tools/utils.py must be called at least once, to copy the correct binary to 'dart'.  If this returns true, then the SDK is working, and CheckedInSdkExecutable() can be used to fetch the executable in subsequent code.  CheckedInPubPath() and CheckedInSdkPath() can also be used.

## Work in Progress

The test script tools/test.py still uses the previous set of checked-in binary executables at tools/testing/bin, and will be updated to used those in tools/sdks when the test scripts are updated
to the current stable version of Dart.
 


