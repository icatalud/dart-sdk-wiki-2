# Handy global aliases
```
[alias]
  pr = "!f() { git fetch origin refs/pull/$1/head:pr/$1; } ; f"
  patch-cl = !sh -c 'git cl patch -b cl$1 $1' -
```