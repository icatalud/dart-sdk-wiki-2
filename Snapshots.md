# Kernel Snapshots

Kernel snapshots allow a Dart programmer to package up an application into a single file and reduce startup time. 

```
$ dart --snapshot-kind=kernel --snapshot=hello.dart.snapshot hello.dart
$ dart hello.dart.snapshot arguments-for-use
Hello, world!
```

These snapshots contain binary form of [Kernel AST](https://github.com/dart-lang/sdk/tree/master/pkg/kernel), but lack parsed classes and functions and lack compiled code. This makes them portable between CPU architectures. But it also means the Dart VM must still convert AST into internal representation and compile functions that are used in each run, which can be a substantial portion of run time for large programs that process a small input.

# JIT Application Snapshots (app-jit)

Starting in 1.21, the Dart VM also supports *application snapshots* also known as *app-jit* snapshots, which include all the parsed classes and compiled code generated during a training run of a program.

```
$ dart --snapshot=hello.dart.snapshot --snapshot-kind=app-jit hello.dart arguments-for-training
Hello, world!
$ dart hello.dart.snapshot arguments-for-use
Hello, world!
```

When running from an application snapshot, the Dart VM will not need to parse or compile classes and functions that were already used during the training run, so it starts running user code sooner.

These snapshots are CPU architecture specific, so a snapshot created by an x64 VM cannot run on an IA32 VM or vice versa.

# AOT Application Snapshots (app-aot)

There is also an *app-aot* variant that does not use a training run, but compiles the entire program starting from main ahead-of-time (AOT). These snapshots can be generated using `dart2aot` script and can be run using `dartaotruntime` binary, both included with SDK starting from version 2.4. 

```
$ dart2aot hello.dart hello.dart.aot
$ dartaotruntime hello.dart.aot
Hello, world! 
```