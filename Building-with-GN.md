This page describes how to build the Dart SDK using GN and Ninja.

# Prerequisites

* Follow the instructions at [Building](Building) under "Dependencies" and "Getting the source"
* (Optional) If you are a Googler, for faster builds install and configure Goma.

# Building

## Linux, Mac

Set the environment variable `DART_USE_GN=1`, then proceed as usual, e.g.:

```
$ export DART_USE_GN=1
$ gclient runhooks
$ ./tools/build.py -m debug,release -a ia32,x64,simarm runtime create_sdk
```

If you don't want to set an environment variable, you can also do this:

```
$ ./tools/gn.py -m all -a all
$ ./tools/build.py --gn -m debug,release -a ia32,x64,simarm runtime create_sdk
```

Note the addition of `--gn` to the command line of `build.py`

## Windows

### Googlers

You may be prompted to authenticate when running `gclient runhooks` or `gn.py` depending on whether `DART_USE_GN` is defined. This is so that `depot_tools` can retrieve the Visual Studio toolchain. Otherwise, the flow is the same, e.g.:

```
$ python .\tools\gn.py -m all -a all
$ python .\tools\build --gn -m debug,release -a ia32,x64 runtime create_sdk
```

For Googlers, there is no need for a full install of Visual Studio, but it is still possible if you would like to use the IDE.

### Non-Googlers

Visual Studio 2015 must be installed in advance, and the following environment variables should be set:

```
DEPOT_TOOLS_WIN_TOOLCHAIN=0
GYP_MSVS_OVERRIDE_PATH=C:\Program Files (x86)\Microsoft Visual Studio 14.0
```

or wherever your Visual Studio install lives. Then do `gclient sync` and proceed as above.

# Using GN

GN has an extensive online help system, which can be accessed by running `gn help`. Also take note of the [full documentation](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/), and [style guide](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/style_guide.md).

## Hints, suggestions, etc.

* We're using GN's formatter. Presubmit checks will complain if your GN is not formatted correctly. Follow the instructions emitted by the presubmit check to rectify any problems.
* In contrast with gyp, GN has a built-in macro system. Use it to avoid duplication.
* Avoid using absolute paths (i.e. paths beginning with "//") so that our `BUILD.gn` files can be consumed by e.g. Flutter and Fuchsia.
* Along the same lines, to make our build files easier to consume, flags should have a flexible default value, which the standalone build may override. See [gn.py](https://github.com/dart-lang/sdk/blob/master/tools/gn.py). `dart_use_fallback_root_certificates` defaults to `false`, for example.
* Last but not least! **A successful GN build produces no output.**