# Building #

AddressSanitizer (ASan) support requires compiling with additional flags. Run gn.py with --asan to set them:

    ./tools/gn.py -m release --asan
    ./tools/build.py -m release runtime

# Running #

Example command-line (currently detects some leaks, see https://github.com/dart-lang/sdk/issues/24467 ):

    export ASAN_SYMBOLIZER_PATH="$PWD/buildtools/linux-x64/clang/bin/llvm-symbolizer"
    export ASAN_OPTIONS=handle_segv=0:detect_leaks=1:symbolize=1
    ./out/ReleaseX64/dart foo.dart

The handle_segv=0 is only crucial when running through the test suite, wherein several tests are expected to segfault, and will fail if ASan installs its own segfault handler.

# Development #

AddressSanitizer works at the C++ language level, hence does not automatically know about code that Dart VM's JIT compiler generates, nor about low-level direct manipulation of the stack from C++ code.

Use the macro `ASAN_UNPOISON(ptr, len)` to explicitly inform ASan about a region of the stack that has been manipulated outside normal portable C++ code.

## Stack pointer ##

When running with detect_stack_use_after_return=1, ASan will return a fake stack pointer when taking the address of a local variable. Use `Isolate::GetCurrentStackPointer()` to get the real stack pointer (calls a tiny pregenerated stub).