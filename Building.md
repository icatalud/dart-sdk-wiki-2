[Dependencies](#dependencies)

[Getting the source](#source)

[Building](#building)

[Testing](#testing)

[Building the standalone VM only](#standalone)

<a name="dependencies"/>

# Dependencies

## C++11

Dart uses some new C++ features from C++11 (previously called C++0x).  Compilers that support these features
are GCC version 4.8 (4.6 is known not to work) and Microsoft Visual Studio 2015 (available in a free community edition for many users).  The clang compiler for linux, included in the Dart distribution, and the compilers used by Xcode on macOS (previously called OS X) are also known to work.

## Linux

Install build tools:

```bash
sudo apt-get install g++ git make python
sudo apt-get install g++-multilib  # If you want to build 32-bit binaries on a 64-bit host.
```

Install Chromium's [depot tools](http://dev.chromium.org/developers/how-tos/install-depot-tools):

```bash
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH="$PATH:$PWD/depot_tools"
```

## Mac OS X

Install XCode.

Install Chromium's [depot tools](http://dev.chromium.org/developers/how-tos/install-depot-tools):
```bash
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH="$PATH:$PWD/depot_tools"
```

## Windows

Install VisualStudio.

Install Chromium's [depot tools](http://dev.chromium.org/developers/how-tos/install-depot-tools).

<a name="source"/>

# Getting the source

```bash
mkdir dart-sdk
cd dart-sdk
gclient config https://github.com/dart-lang/sdk.git
gclient sync
```

Or if you have [ssh keys setup for GitHub](https://help.github.com/articles/generating-an-ssh-key/) (recommended for committers):

```bash
mkdir dart-sdk
cd dart-sdk
gclient config git@github.com:dart-lang/sdk.git
gclient sync
```

<a name="building"/>

# Building

Build the 64-bit SDK:

```bash
cd dart-sdk/sdk
./tools/build.py --mode release --arch x64 create_sdk
```

The output will be in `out/ReleaseX64/dart_sdk`, `xcodebuild/ReleaseX64/dart_sdk`, or `build/ReleaseX64/dart_sdk` on Linux, Mac or Windows respectively.

Build the 32-bit SDK:

```bash
cd dart-sdk/sdk
./tools/build.py --mode release --arch ia32 create_sdk
```
The output will be in `out/ReleaseIA32/dart_sdk`, `xcodebuild/ReleaseIA32/dart_sdk`, or `build/ReleaseIA32/dart_sdk` on Linux, Mac or Windows respectively.

See also [building for ARM](https://github.com/dart-lang/sdk/wiki/Building-Dart-SDK-for-ARM-processors).

## Tips

By default the build and test scripts select the debug binaries. You can build and test the release version of the VM by specifying `--mode=release` or both debug and release by specifying `--mode=all` on the respective `build.py` and `test.py` command lines.  This can be shortened to `-mrelease` or `-m release`, and the architecture can be specified with `--arch=ia32` or `-a x64`, the default.  Other architectures, like `arm`, `arm64`, and `mips`, are also supported.

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
cd sdk
rm -r out
export CC=third_party/clang/linux/bin/clang
export CC_host=third_party/clang/linux/bin/clang
export CXX=third_party/clang/linux/bin/clang++
export CXX_host=third_party/clang/linux/bin/clang++
export C_INCLUDE_PATH=third_party/clang/linux/lib/clang/3.4/include/
export CPLUS_INCLUDE_PATH=third_party/clang/linux/lib/clang/3.4/include/
gclient runhooks
./tools/build.py -m release -a all runtime
```

## Special note for Windows users using Visual Studio Community Edition:
Your Visual Studio executable command may have a different name from the standard Visual Studio installations. You can specify the name for that executable by passing the additional flag "--executable=$VS\_EXECUTABLE\_NAME" to build.py. The executable name will probably be something like "VSExpress.exe".

## Building on Windows with Visual Studio 2015
Gyp should autodetect the version of Visual Studio you have, and produce solution files in the correct format.
If this is not happening, then set environment variable gyp\_msvs\_version to 2015.

For example, this will produce Visual Studio 2015-compliant solution files:
```bash
set gyp_msvs_version=2015
gclient runhooks
```

<a name="testing"/>

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

See also [Testing Dart2js](https://github.com/dart-lang/sdk/wiki/Testing-Dart2js) for dart2js specific examples.

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
To debug a browser test failure, you must start a local http server to serve the test.  This is made easy with a helper script in dart/tools/testing.  The error report from test.py gives the command line to start the server in a message that reads:
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

<a name="standalone"/>

# Building the standalone VM only

You can tell the build script to build the runtime targets only with the command:
```bash
$ ./tools/build.py runtime
```