# Introduction
These are instructions to check out and build a modified version of Chromium 50 that embeds the DartVM (Dartium 50).

## Warning

The Chromium binaries generated via these instructions have not been tested for security. We highly discourage you from using these for general Web browsing.

# Set up your machine
Follow the instructions in the [PreparingYourMachine] page to get Chromium's dependencies.

## Initialize your source tree
The following instructions will create a new checkout containing a Dart-enabled version of Chromium and WebKit. We recommend that you use a local file system at least for the output of the builds. If your code is in some NFS partition, you can link the out directory to a local directory.

### Checkout for non-committers

```bash
# Create a directory to work in.
mkdir dartium
cd dartium

# Create a .gclient file.
gclient config --deps-file tools/deps/dartium.deps/DEPS --name=src/dart https://github.com/dart-lang/sdk.git

# Checkout all the sub-projects.
gclient sync

# Generate the build files for the current OS. (This is usually, but not always, run as part of gclient sync.)
gclient runhooks
```

## Checkout for committers
A Chromium / Dartium checkout pulls code from ~90 repositories into a single directory hierarchy. We do not need to modify the vast majority of these. Dartium development typically requires commit access to 3 repositories: the Chromium repository (src), the Blink repository (src/third_party/WebKit), and the Dart repository (src/dart).

For Chromium and Blink, we work off of Dart-specific branches that we maintain. These are branched off of beta branches of chromium.  They are stored in git repositories at https://chromium.googlesource.com/dart/dartium/src and https://chromium.googlesource.com/dart/dartium/blink.

### Using Git
```bash
# Check that we can authenticate with GitHub for the Dart SDK. Set up ssh keys with Github if this fails.
git ls-remote git@github.com:dart-lang/sdk.git

# Follow the steps above for non-committers, but use a git URL instead of an HTTPS URL to check out
mkdir dartium
cd dartium
gclient config --deps-file tools/deps/dartium.deps/DEPS --name=src/dart git@github.com:dart-lang/sdk.git

# Get latest version of all files
gclient sync

# Generate the build files for the current OS. (This is usually, but not always, run as part of gclient sync.)
gclient runhooks
```

# Build
You can either follow the standard Chromium build instructions or use the following convenience script:

```
cd src
./dart/tools/dartium/build.py --mode=Release
```
If your build fails, you may need to install dependencies such as libspeechd-dev. On Linux, this is most easily done by running:

```
./build/install-build-deps.sh
```

# Keeping up to date
```
cd src
```
You need to update the repository containing the DEPS file explicitly:

```
pushd dart; git pull --rebase; popd
gclient sync
gclient runhooks
```
and rebuild.

# SDK changes

Create a separate Dart enlistment (outside of the above Dartium enlistment e.g., dartium-git) from Dart's bleeding edge see https://code.google.com/p/dart/wiki/GettingTheSource

Enlist in the Dartium src tree (Chromium and WebKit are now in the same GIT repository)

```
git clone https://chromium.googlesource.com/dart/dartium/src src
cd src
git checkout -b 2661_work origin/releases/2661_work
cd ..
gclient sync -n && gclient runhooks
```
Build the Dart release see https://code.google.com/p/dart/wiki/Building

```
cd dart
./tools/build.py -m release --arch=ia32
```
Change a template file(s) in dart-src/dart/tools/dom/templates

Steps to regenerate the SDK:

```
cd dart-repo/dart/tools/dom/script
go.sh
```
Some of the SDK library files that are generated in the directory dart-repo/dart/sdk/lib are:

```
_blink/dartium/_blink_dartium.dart
html/dartium/html_dartium.dart
svg/dartium/svg_dartium.dart
web_gl/dartium/web_gl_dartium.dart

html/dart2js/html_dart2js.dart
...
```

Using 'git status' displays the entire list of changed/re-generated files (could be more or less than the files above).

Copy the above files to your dartium enlistment sdk directory (e.g., dartium-git/src/dart/sdk/lib).

Rebuild dartium.


# Run
Launch Dartium on Linux: `./out/Release/chrome`

Launch Dartium on Mac: `./out/Release/Chromium.app/Contents/MacOS/Chromium`

Launch Dartium on Windows: `.\out\Release\chrome.exe`

# Test
You can run Dart specific tests as follows:

```
./dart/tools/dartium/test.py --mode=Release
```
Try a local page with Dart content. In your checkout, you can find sample HTML files with Dart content under dartium/src/dart/client/samples. You can also create a page as follows:

```
<html>
  <body>
    <script type="application/dart">
      import 'dart:html';
      void main() {
         final div = new DivElement();
         div.innerHtml = 'Hello from Dart';
         document.body.append(div);
      }
    </script>
  </body>
</html>
```

If you want to run tests from your Dart client, use the --dartium flag.

From your Dart client, assuming it's a sibling of your Dartium client:

```
./tools/test.py --mode=release --compiler=none --runtime=dartium --dartium=../../dartium-git/src/out/Release/Chromium.app/Contents/MacOS/Chromium
```

# Profiling
See Profiling_the_Dart_VM_in_Dartium_on_Linux