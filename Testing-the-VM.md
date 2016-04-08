## JIT

```
./tools/build.py -mdebug,release runtime
./tools/test.py -mdebug,release
./tools/test.py -mdebug,release --checked
```

## Noopt

```
./tools/build.py -mdebug,release runtime
./tools/test.py -mdebug,release --noopt
```

## Precompilation

```
./tools/build.py -mrelease runtime
./tools/test.py -mrelease -cprecompiler -rdart_precompiled
```

## App snapshots

```
./tools/build.py -mproduct runtime
./tools/test.py -mproduct -cdart2app -rdart_product
```