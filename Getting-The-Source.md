This page tells you how to get and build the Dart source code.
You can also [browse](https://github.com/dart-lang/sdk) the repository or list the [changes](https://github.com/dart-lang/sdk/commits/master).

# Machine setup

If your machine is already set up to build Chromium, then you should have all the prerequisites for building Dart. Otherwise prepare your machine according to these instructions: [Preparing Your Machine](Preparing-your-machine-to-build-the-Dart-SDK)

# Getting the source

Create a directory into which you want to checkout the Dart sources and change into that directory.

Be sure to install the prereqs by following [Preparing Your Machine](PreparingYourMachine.md)

One time setup:
```bash
gclient config https://github.com/dart-lang/sdk.git
```

From within your source checkout run this command to update to the latest sources:
```bash
gclient sync
```

### For committers

You can commit changes directly from the checkout created above, but if you are doing a lot of commits it is easier if you setup a ssh keypair for github, [this](https://help.github.com/articles/generating-ssh-keys/) guide explains how to do that. If you don't use a key pair you will need to provide your username and password when you land a change.

If you have a key pair setup, you can instead do
```bash
gclient config git@github.com:dart-lang/sdk.git
gclient sync
```
# Building

For instructions on how to build and test, refer to [Building](Building.md)

# Landing changes

Please see [Contributing](Contributing.md)