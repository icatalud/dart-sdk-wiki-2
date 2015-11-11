# Handy global aliases


```
# Add these to $HOME/.gitconfig
[alias]
  # For use with a GitHub pull request
  # Usage: git pr 123 (where 123 is the pull request number)
  #   Creates a new branch: pr/123 containing the contents of the pull request
  pr = "!f() { git fetch origin refs/pull/$1/head:pr/$1; } ; f"
  # TODO: kevmoo to add details
  patch-cl = !sh -c 'git cl patch -b cl$1 $1' -
```