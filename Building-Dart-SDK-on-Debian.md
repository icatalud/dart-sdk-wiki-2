(You can also build for CentOS or [Ubuntu 10.04 LTS](https://github.com/dart-lang/sdk/wiki/Building-Dart-SDK-on-Ubuntu-10.04-Server).)

The standard recipe for setting up an Ubuntu machine for building Dart is referring to the default setup for developing Chromium. However this setup includes a lot of stuff which is not needed for just building the Dart SDK.

The steps described here should be sufficient for building the 64-bit Dart SDK on Debian. It has been tested on a Debian 7.2 instance on Google Compute Engine.

## Install Subversion and the required build-tools

Install the following packages to be able to build the Dart SDK. If just building the Dart standalone executable then openjdk-6-jdk is not required.

```
$ sudo apt-get -y install subversion
$ sudo apt-get -y install make
$ sudo apt-get -y install g++
$ sudo apt-get -y install openjdk-6-jdk
```

## Get the depot_tools and add them to the path

The depot_tools from the Chromium project are required for checking out the source.

```
$ svn co http://src.chromium.org/svn/trunk/tools/depot_tools
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