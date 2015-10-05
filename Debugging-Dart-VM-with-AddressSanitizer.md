# Building #

AddressSanitizer support requires building with Clang:

    export CXX="third_party/clang/linux/bin/clang++ -fsanitize=address -fPIC"
    gclient runhooks
    ./tools/build.py -m debug -a x64 runtime

# Running #

Example command-line (currently detects some leaks, see https://github.com/dart-lang/sdk/issues/24467 )

    export ASAN_SYMBOLIZER_PATH=`pwd`/third_party/clang/linux/bin/llvm-symbolizer
    ASAN_OPTIONS=handle_segv=0:detect_leaks=1 out/DebugX64/dart foo.dart

The handle_segv=0 is only crucial when running through the test suite, wherein several tests are expected to segfault, and will fail if AddressSanitizer 