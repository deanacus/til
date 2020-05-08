# Get A Single File From Stash

Work out which stash your change is it:

```
git stash list
```

then checkout the file from the stash:

```
git checkout stash@{0} -- path/to/file
