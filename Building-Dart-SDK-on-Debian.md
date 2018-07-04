(You can also build for [CentOS](https://github.com/dart-lang/sdk/wiki/Building-Dart-on-CentOS,-Red-Hat,-Fedora-and-Amazon-Linux-AMI) or [Ubuntu 10.04 LTS](https://github.com/dart-lang/sdk/wiki/Building-Dart-SDK-on-Ubuntu-10.04-Server).)

The standard recipe for setting up an Ubuntu machine for building Dart is referring to the default setup for developing Chromium. However this setup includes a lot of stuff which is not needed for just building the Dart SDK.

The steps described here should be sufficient for building the 64-bit Dart SDK on Debian. It has been tested on a Debian 7.2 instance on Google Compute Engine.

## Get the depot_tools and add them to the path

The depot_tools from the Chromium project are required for checking out the source.

```
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
$ export PATH=$PATH:`pwd`/depot_tools
```

## Get the Dart source and generate makefiles

[Follow these instructions](https://github.com/dart-lang/sdk/wiki/Getting-The-Source).

## Build

To build the Dart SDK use the following commands.

```
$ cd dart
$ tools/build.py --mode=release --arch=x64 create_sdk
```

The Dart SDK is now in the directory out/ReleaseX64/dart-sdk.

To only build the Dart standalone executable use these commands instead.

```
$ cd dart
$ tools/build.py --mode=release --arch=x64 runtime
```

The Dart standalone executable is now in out/ReleaseX64/dart.