# Developer notes for working on kernel

## How to test changes on pkg/kernel:

Run unit tests of kernel, front_end, and dart2js that are directly affected:
```
tools/test.py pkg/kernel -mrelease --checked
tools/test.py pkg/front_end -mrelease --checked
tools/test.py dart2js/kernel -mrelease --checked
```

Run end-to-end tests using dartk + VM:
```
/tools/test.py -m release -c dartk language
```

Optionally (this is slow) run end-to-end tests using AOT:
```
tools/build.py dart_precompiled_runtime
tools/test.py -cdartkp -rdart_precompiled language co19
```

Comparing the output of compiling dart2js before and after the change. This [script][1] can make it easier to compare the results.

[1]: https://gist.github.com/asgerf/adde37ed58fe984d53b82d362187c777
