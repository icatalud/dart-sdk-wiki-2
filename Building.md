# How to Build and Test the Dart Project

# Building for various Linux distros

Some Linux distros (like Ubuntu 10.04) require special instructions.
TODO(move the below wikis)
  * [Building Dart for Ubuntu 10.04](https://code.google.com/p/dart/wiki/BuildDartSDKOnUbuntu10_04)
  * [Building Dart for Debian](https://code.google.com/p/dart/wiki/BuildingOnDebian)
  * [Building Dart for CentOS, RedHat, Fedora and Amazon Linux AMI](https://code.google.com/p/dart/wiki/BuildingOnCentOS)

# Configuring your machine

Follow the instructions in the [PreparingYourMachine](PreparingYourMachine) page.

# Building

Follow the steps in [GettingTheSource](GettingTheSource) to retrieve everything. You will end up with a tree that looks like this:
```
sdk/
  client/
  pkg/
  runtime/
  tools/
  ...
```

## Building everything

From the `sdk` directory use the `build.py` script under `tools/`:
```bash
$ cd sdk
$ ./tools/build.py -m release --arch=ia32
```

For 64 bit:

```bash
$ cd sdk
$ ./tools/build.py -m release -a x64
```

## Building just the SDK

```bash
$ cd sdk
$ ./tools/build.py -m release create_sdk
```

You can also build the SDK for a specific architecture:

```bash
$ cd sdk
$ ./tools/build.py -m release -a x64 create_sdk
```

## Tips

By default the build and test scripts select the debug binaries. You can build and test the release version of the VM by specifying `--mode=release` or both debug and release by specifying `--mode=all` on the respective `build.py` and `test.py` command lines.  This can be shortened to `-mrelease` or `-m release`, and the architecture can be specified with `--arch=x64` or `-a ia32`, the default.  Other architectures, like 'arm' and 'mips', are also supported.

We recommend that you use a local file system at least for the output of the builds. The output directory is `out` on linux, `xcodebuild` on Mac OS, and `build` on Windows.  If your code is in some NFS partition, you can link the `out` directory to a local directory:
```bash
$ cd sdk/
$ mkdir -p /usr/local/dart-out/
$ ln -s -f /usr/local/dart-out/ out
```

### Notification when build is done

The environment variable `DART_BUILD_NOTIFICATION_DELAY` controls if `build.py` displays a notification when the build is done. If a build takes longer than `DART_BUILD_NOTIFICATION_DELAY` a notification will be displayed.

A notification is a small transient non-modal window, for now, only supported on Mac and Linux.

## Building with clang on linux

After doing a "gclient sync", clang can be used for compiling the runtime with
```bash
$ cd sdk
dart $ rm -r out
dart $ export CC=third_party/clang/linux/bin/clang
dart $ export CC_host=third_party/clang/linux/bin/clang
dart $ export CXX=third_party/clang/linux/bin/clang++
dart $ export CXX_host=third_party/clang/linux/bin/clang++
dart $ export C_INCLUDE_PATH=third_party/clang/linux/lib/clang/3.4/include/
dart $ export CPLUS_INCLUDE_PATH=third_party/clang/linux/lib/clang/3.4/include/
dart $ gclient runhooks
dart $ ./tools/build.py -mrelease -a all runtime
```

## Special note for Windows users using Visual Studio Express:
Your Visual Studio executable command has a different name from the standard Visual Studio installations. You can specify the name for that executable by passing the additional flag "--executable=$VS\_EXECUTABLE\_NAME" to build.py. The executable name will probably be something like "VSExpress.exe".

## Building on Windows with Visual Studio 2012 or 2013
Before you do "gclient runhooks" set environment variable gyp\_msvs\_version to 2012 or to 2013 depending on version of Visual Studio you are using.

For example, this will produce Visual Studio 2013-compliant solution files:
```bash
set gyp_msvs_version=2013
gclient runhooks
```

By default(when gyp\_msvs\_version environment variable is not defined) generated .sln-file is created in Visual Studio 2010 format.

# Testing

All tests are executed using the `test.py` script under `tools/`.  You may need to build everything once before testing, using `build.py` as described above. E.g.
```bash
$ ./tools/build.py --mode release --arch ia32
```

Now you can run all tests as follows (Safari, Firefox, Chrome, and IE must be installed, if you want to run the tests on them.  Dartium, and drt (content\_shell) are automatically downloaded by the test scripts.):
```bash
$ ./tools/test.py -mrelease --arch=ia32 --compiler=none,dart2js --runtime=vm,d8,drt,chrome,dartium,ff,[safari,ie]
```
Specify the compiler used (optional -- only necessary if you are compiling to JavaScript (required for most browsers), the default is "none") and a runtime (where the code will be run).

You can run a specific test by specifying its full name or a prefix. For instance, the following runs only tests from the core libraries:
```bash
$ ./tools/test.py -mrelease --arch=ia32 --runtime=vm corelib
```
The following runs a single test:
```bash
$ ./tools/test.py -mrelease --arch=ia32 --runtime=vm corelib/ListTest
```

Make sure to run tests using the release VM if you have built the release VM, and for the same architecture as you have built.

See also TestingDart2js for dart2js specific examples.

## Complex examples

Adjust the flags to your needs: appropriate values for `--arch` and `--tasks` will depend on your system.  The below examples are taken from a 12 core x86\_64 system.

Dart analyzer tests example:
```bash
./tools/test.py \
  --compiler dartanalyzer \
  --runtime none \
  --progress color \
  --arch x64 \
  --mode release \
  --report \
  --time \
  --tasks 6 \
  language
```

VM tests example:
```bash
./tools/test.py \
  --compiler none \
  --runtime vm \
  --progress color \
  --arch x64 \
  --mode release \
  --checked \
  --report \
  --time \
  --tasks 6 \
  language
```

Dart2JS example:
```bash
./tools/test.py \
  --compiler dart2js \
  --runtime chrome \
  --progress color \
  --arch x64 \
  --mode release \
  --checked \
  --report \
  --time \
  --tasks 6 \
  language
```

## Troubleshooting and tips for browser tests
  1. To debug a browser test failure, you must start a local http server to serve the test.  This is made easy with a helper script in dart/tools/testing.  The error report from test.py gives the command line to start the server in a message that reads:
```bash
To retest, run:  /usr/local/[...]/dart/tools/testing/bin/linux/dart /usr/local/[...]/dart/tools/testing/dart/http_server.dart ...
```
After starting the server, you can run the browser using the command line starting with `Command[[browser name]]`.  The debugging tools in the browser can be used to set Javascript or Dart breakpoints, and the reload button should rerun the test.
    * **Tip:** In the Sources tab of Chrome's developer tools, a "stop sign" button in the upper right tells the browser to break when an exception is thrown.

Some common problems you might get when running tests for the first time:
  * Some fonts are missing. Chromium tests use `DumpRenderTree`, which needs some sets of fonts. See [LayoutTestsLinux](http://code.google.com/p/chromium/wiki/LayoutTestsLinux) for instructions in how to fix it.
  * No display is set (e.g. running tests from an ssh terminal). `DumpRenderTree` is a nearly headless browser. Even though it doesn't open any GUI, it still must have an X session available. There are several ways to work around this:
    * Use `xvfb`:
```bash
xvfb-run ./tools/test.py --arch=ia32 --compiler=dart2js --runtime=drt ...
```
    * Other options include using ssh X tunneling (`ssh -Y`), NX, VNC, setting up `xhost` and exporting the DISPLAY environment variable, or simply running tests locally.

# Building the standalone VM only

You can tell the build script to build the runtime targets only with the command:
```bash
$ ./tools/build.py runtime
```