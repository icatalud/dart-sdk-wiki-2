# Building #

AddressSanitizer (ASan) support requires building with Clang:

    export CXX="third_party/clang/linux/bin/clang++ -fsanitize=address -fPIC"
    export GYP_DEFINES="asan=1"
    gclient runhooks
    ./tools/build.py -m debug -a x64 runtime

# Running #

Example command-line (currently detects some leaks, see https://github.com/dart-lang/sdk/issues/24467 ):

    export ASAN_SYMBOLIZER_PATH=`pwd`/third_party/clang/linux/bin/llvm-symbolizer
    ASAN_OPTIONS=handle_segv=0:detect_leaks=1 out/DebugX64/dart foo.dart

The handle_segv=0 is only crucial when running through the test suite, wherein several tests are expected to segfault, and will fail if ASan installs its own segfault handler.

# Development #

AddressSanitizer works at the C++ language level, hence does not automatically know about code that Dart VM's JIT compiler generates, nor about low-level direct manipulation of the stack from C++ code.

Use the macro `ASAN_UNPOISON(ptr, len)` to explicitly inform ASan about a region of the stack that has been manipulated outside normal portable C++ code.

## Stack pointer ##

When running with detect_stack_use_after_return=1, ASan will return a fake stack pointer when taking the address of a local variable. Use `Isolate::GetCurrentStackPointer()` to get the real stack pointer (calls a tiny pregenerated stub).