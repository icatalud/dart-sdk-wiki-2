**Tip:** Don't want to build Dart from source? Grab [binaries of the Dart SDK](https://www.dartlang.org/tools/sdk/), or [Dartium](https://www.dartlang.org/tools/dartium/).

Dart is built using much of the same tools as Chromium, so the steps here are similar to what you'll find [for setting that up](http://www.chromium.org/developers/contributing-code). If your machine is already setup to build Chromium you should be good to go and can skip the steps below.


# Linux

Install the [Linux build prerequisites](https://chromium.googlesource.com/chromium/src/+/master/docs/linux_build_instructions_prerequisites.md). In particular you might need to install the 32-bit libraries on a 64-bit Linux system by following the [Automated Setup instructions](http://code.google.com/p/chromium/wiki/LinuxBuildInstructionsPrerequisites#Automated_Setup).

If you run Ubuntu, you can try this script that should install all of the dependencies for you. This script is optional; you can always install all the build tools by hand.

You might need to get GCC 4.6 to compile the 64 bit version of the VM. These instructions helped me, when on Ubuntu 10.04: http://www.faqoverflow.com/superuser/310809.html

```
$ wget http://src.chromium.org/svn/trunk/src/build/install-build-deps.sh
OR
$ svn co http://src.chromium.org/chrome/trunk/src/build; cd build
Then,
$ chmod u+x install-build-deps.sh
$ ./install-build-deps.sh --no-chromeos-fonts
```
  
If using 64-bit Ubuntu, install the 32-bit development libraries.

```
$ sudo apt-get install libc6-dev-i386 g++-multilib
```

Install depot tools, adding it to your `PATH` environment variable:

```
$ cd <directory where you want the depot_tools to live>
$ git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
$ export PATH=$PATH:`pwd`//depot_tools
```

Other notes:

1. If cross-compiling for ARM, also pass --arm to install-build-deps.sh
1. If the include directory /usr/include/openssl/ is missing, you need to install the libssl-dev package (sudo apt-get install libssl-dev).
1. More details can be found in [chromium's build instructions for linux](http://code.google.com/p/chromium/wiki/LinuxBuildInstructions).
1. You might way to try [this script to install depot\_tools.](https://github.com/jankeromnes/cr)

# Mac

1. Install [Xcode](http://developer.apple.com/tools/xcode/) 
1. Install depot\_tools by following the [Chrome Mac Build Instructions](http://code.google.com/p/chromium/wiki/MacBuildInstructions). Or, try this [script to install depot\_tools](https://github.com/jankeromnes/cr).

# Windows

1. Install any version of Visual Studio.
    * Follow the instructions [here](http://www.chromium.org/developers/how-tos/build-instructions-windows) to setup chromium specific dependencies.
1. Install [depot\_tools](http://dev.chromium.org/developers/how-tos/depottools).
1. If you are planning on running the test suite it is beneficial to disable crash pop ups, see [here](http://msdn.microsoft.com/en-us/library/windows/desktop/bb513638(v=vs.85).aspx)