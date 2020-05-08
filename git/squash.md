# Squashing Commits

It's generally a good idea to make sure your commit history makes sense
before you publish it somewhere. So, it's nice to squash related commits
into single commits that solve a specific problem. It means you can still
do atomic commints, but keep your public history readable.

Just do an interactive rebase on your local branch, going back to the first
commit you'd like to tidy up from:

```
git rebase -i HEAD~x
```

Where x is the number of commits you would like to go back.
