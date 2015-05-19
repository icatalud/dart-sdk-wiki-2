# Introduction

The Dart VM runs on a variety of ARM processors on Linux and Android. This document explains how to build the Dart VM to target these platforms. More detailed, platform specific information can be found under Android, AndroidContentShell, and RaspberryPi.

# Getting a Cross-compiler

## Linux

If you are building natively on the device you will be running on, you can skip this step. If you are running Ubuntu, you can obtain a cross-compiler by doing the following:

```
$ wget http://src.chromium.org/svn/trunk/src/build/install-build-deps.sh
$ ./install-build-deps.sh --no-chromeos-fonts --arm
```

If you have already run the script, but without --arm, simply rerun with the --arm option.

If you are using a different Linux distribution, follow the instructions for your distribution for obtaining an ARM cross-compiler.

## Android

Follow instructions under [Getting The Source](https://github.com/dart-lang/sdk/wiki/Getting-The-Source), and under "One-time Setup" under Android

# Building

## Linux

On Ubuntu, after installing the default arm cross-compiler, simply do:

```
$ ./tools/build.py -m release -a arm runtime
```

You can also produce a Dart SDK by replacing runtime with create_sdk. This process involves also building a VM that targets ia32, which is used to generate a few parts of the SDK.

You can use a different toolchain using the -t switch. For example, if the path to your gcc is /path/to/toolchain/prefix-gcc, then you'd invoke the build script with:

```
$ ./tools/build.py -m release -a arm -t /path/to/toolchain/prefix runtime
```

The default Ubuntu cross-compiler will target ARMv7 processors. The Dart VM also supports ARMv6 (e.g. RaspberryPi), and has experimental support ARMv5TE processors (e.g. Lego Mindstorm). For those processors, you will need to provide your own compiler toolchain.

If you have an arm64 toolchain, you can also build a Dart VM supporting arm64 processors:

```
$ ./tools/build.py -m release -a arm64 -t /path/to/arm64/toolchain/prefix runtime
```

As of yet, the build script does not go looking for any default arm64 cross-compiler.

## Android

The standalone Dart VM can also target Android. For ARM devices:

```
$ ./tools/build.py -m release -a arm --os=android runtime
```

and ARM64 devices:

```
$ ./tools/build.py -m release -a arm64 --os=android runtime
```

For all of these configurations, SDKs can be built using the create_sdk target as above.

## Debian Packages

You can create Debian packages targeting ARM as follows:

```
$ ./tools/create_tarball.py
$ ./tools/create_debian_packages.py -a {armhf, armel} [-t /path/to/cross/toolchain/prefix]
```

If you are on Ubuntu, and no -t switch is given, you'll get a Debian package targeting ARMv7. The argument to the -a switch should be appropriate for the platform you are targeting.