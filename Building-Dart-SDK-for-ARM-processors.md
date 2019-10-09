# Introduction

The Dart VM runs on a variety of ARM processors on Linux and Android. This document explains how to build the Dart VM and SDK to target these platforms. More detailed platform-specific information can be found under [Android](https://github.com/dart-lang/sdk/wiki/Building-Dart-SDK-for-Android) and [Raspberry Pi](https://github.com/dart-lang/sdk/wiki/Building-Dart-SDK-for-Raspberry-Pi).

# Cross-compiling

If you are building natively on the device you will be running on, you can skip this step.  The build scripts download a cross-compilation toolchain using clang, so you do not need to install a cross-compiler yourself.

## Linux

If you are running Ubuntu, you can obtain a cross-compiler by doing the following:	

```	
$ sudo apt-get install g++-arm-linux-gnueabihf  # For 32-bit ARM (ARMv7)	
$ sudo apt-get install g++-aarch64-linux-gnu    # For 64-bit ARM	
```	

## Android

Follow instructions under "One-time Setup" under Android

# Building

## Linux

On Ubuntu, simply do:

```
$ ./tools/build.py -m release -a arm create_sdk
```

You can also produce only a Dart VM runtime, no SDK, by replacing `create_sdk` with `runtime`. This process involves also building a VM that targets ia32, which is used to generate a few parts of the SDK.

You can use a different toolchain using the -t switch. For example, if the path to your gcc is /path/to/toolchain/prefix-gcc, then you'd invoke the build script with:

```
$ ./tools/build.py -m release -a arm -t /path/to/toolchain/prefix create_sdk
```

The default clang cross-compiler toolchain will target ARMv7 processors. The Dart VM also supports ARMv6 (e.g. RaspberryPi), and has experimental support ARMv5TE processors (e.g. Lego Mindstorm). For those processors, you will need to provide your own compiler toolchain.

You can also build a Dart VM and SDK supporting arm64 processors:

```
$ ./tools/build.py -m release -a arm64 -t /path/to/arm64/toolchain/prefix create_sdk
```

## Android

The standalone Dart VM can also target Android. For ARM devices:

```
$ ./tools/build.py -m release -a arm --os=android create_sdk
```

and ARM64 devices:

```
$ ./tools/build.py -m release -a arm64 --os=android create_sdk
```

For all of these configurations, the runtime only can be built using the runtime target as above.

## Debian Packages

You can create Debian packages targeting ARM as follows:

```
$ ./tools/create_tarball.py
$ ./tools/create_debian_packages.py -a {armhf, armel} [-t /path/to/cross/toolchain/prefix]
```

If you are on Ubuntu, and no -t switch is given, you'll get a Debian package targeting ARMv7. The argument to the -a switch should be appropriate for the platform you are targeting.