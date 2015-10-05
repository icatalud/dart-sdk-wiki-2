# Building #

AddressSanitizer support requires building with Clang:

    export CXX="third_party/clang/linux/bin/clang++ -fsanitize=address -fPIC"
    export ASAN_SYMBOLIZER_PATH=`pwd`/third_party/clang/linux/bin/llvm-symbolizer
    gclient runhooks
    ./tools/build.py -m debug -a x64 runtime