# Introduction

These are instructions for building and running the Dart standalone VM for an ARMv6 Raspberry Pi 1 device running the Raspbian distribution of Linux.

## Building

Building the Dart VM for the Raspberry Pi 1 requires a toolchain that targets it. To get it, clone [this repository](https://github.com/raspberrypi/tools).

To configure the build, run `tools/gn.py` specifying `armv6` for the architecture, the path to the toolchain, and an option indicating that the toolchain supports the hardfp ABI. For Googlers, builds will be faster if Goma is disabled.

```bash
$ ./tools/gn.py -m release -a armv6 \
  -t armv6=/toolchain/arm-bcm2708/gcc-linaro-arm-linux-gnueabihf-raspbian-x64/bin/arm-linux-gnueabihf- \
  --arm-float-abi hard --no-goma
```

Then build as usual:

```bash
$ ./tools/ninja.py -m release -a armv6 create_sdk
```

You'll find the built SDK under `out/ReleaseXARMV6/dart-sdk`.

# Run on Hardware

To run Dart programs on the Pi, upload this SDK to the device:

```bash
scp -r out/ReleaseXARM/dart-sdk pi@[raspberry pi ip address]:./dart-sdk
```

Then, you can add `~/dart-sdk/bin` to your path, and use the SDK as usual.

# Issues

This process is experimental, but please feel free to file an Issue with our tracker if you run into any problems.