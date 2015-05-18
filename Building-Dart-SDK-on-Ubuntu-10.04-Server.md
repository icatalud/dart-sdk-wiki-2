# Building Dart SDK on Ubuntu 10.04 Server

(You can also build for Debian or CentOS).

This document assumes you have a new Ubuntu 10.04 server, and you want to build Dart SDK. If you have a modern Linux distro, or Mac or Windows, you can follow these instructions instead.

The document also assumes you have a 64 bit machine and you want to build both 32 bit and 64 bit SDKs.

Install git and svn:

`sudo apt-get install git-core subversion git-svn`

Update apt-get

`sudo apt-get update`

Install java

`sudo apt-get install openjdk-6-jdk`

Install build tools

`sudo apt-get install build-essential`

Get the chromium build scripts

`cd to where you can put depot_tools`
`svn co http://src.chromium.org/svn/trunk/tools/depot_tools`

Add depot_tools to your path

`export PATH=$PATH:where/you/installed/depot_tools`

Install the 32 bit libs

`sudo apt-get install libc6-dev-i386 g++-multilib`

Install GCC 4.6

```
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gcc-4.6
sudo apt-get install g++-4.6
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 20
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
gcc --version # should show 4.6
```

[Get the source](https://github.com/dart-lang/sdk/wiki/GettingTheSource)

Build

```
cd dart
tools/build.py -m release -a x64 create_sdk # creates just the 64 bit SDK
```

Get the compiled bits

The SDK should be in out/ReleaseX64?/dart-sdk

Issues?

Did we miss a step? Need to clarify something? Please open a bug on dartbug.com with suggestions. Adding a comment below will probably be missed, so please open a bug.