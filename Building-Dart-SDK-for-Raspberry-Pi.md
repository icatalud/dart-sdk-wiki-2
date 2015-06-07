# Introduction

These instructions will let you build and run the Dart standalone VM for a Raspberry Pi device running the Raspbian distribution of Linux. For now, this process will likely only work on a Linux machine.

# Build

First, grab the Dart source as described in [Preparing Your Machine](https://github.com/dart-lang/sdk/wiki/Preparing-your-machine-to-build-the-Dart-SDK) and [Getting The Source](https://github.com/dart-lang/sdk/wiki/Getting-The-Source). 

## Cross-compile

This build will require a cross-compiler that you can obtain by cloning [this repository](https://github.com/raspberrypi/tools).

You can specify the cross-compiler to the build.py command using the --toolchain argument, as follows. From your Dart checkout, to build just the VM:

```bash
./tools/build.py -m release -a arm \
  --toolchain=/path/to/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf \
  runtime
```

You'll find the build products under `out/ReleaseXARM/`. You can optionally strip the dart binary to make it smaller:

```bash
/path/to/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf-strip \
  out/ReleaseXARM/dart
```

# Run on Hardware

To run Dart programs on the Pi, we'll create a dart-sdk:

```bash
./tools/build.py -m release -a arm \
  --toolchain=/path/to/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf \
  create_sdk
```

Then, we'll upload this sdk to the device:

```bash
scp -r out/ReleaseXARM/dart-sdk pi@[raspberry pi ip address]:./dart-sdk
```

Now, we're all set. Add `dart-sdk/bin` to your path and go to town!

# Run on an Emulator

  * Download a [Raspbian image](http://www.raspberrypi.org/downloads/).
  * Follow the instructions here [to run Raspberry Pi in qemu](http://xecdesign.com/qemu-emulating-raspberry-pi-the-easy-way/).
  * When you come to the step "First (proper) boot", append the flag "-redir tcp:2222::22" so that we can scp files to the guest OS.
  * Follow instructions as above, replacing the IP address of the Raspberry Pi device with 127.0.0.1. You'll also need to specify the port 2222 with ssh ({{{-p 2222}}}) and scp ({{{-P 2222}}}).

# Debian Package

If your Raspberry Pi is running a Debian Wheezy-based Raspbian, it's also possible to create a Debian package for the Dart SDK. First, create a tarball:

```bash
./tools/create_tarball.py
```

Then, use the `create_debian_packages.py` script, again specifying the cross-compiler:

```bash
$ ./tools/create_debian_packages.py -a armhf \
  -t /path/to/tools/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf
```

Transfer to the Pi, and install as usual.

# Issues

This process is experimental, but please feel free to file an Issue with our tracker if you run into any problems.