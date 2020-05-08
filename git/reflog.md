# Undo An Unwanted/Bad Rebase

In my personal work, I like to avoid merge commits, and instead rebase when I do a git pull, merge a PR, etc.

But at work, we use merge commits. Which means when I forgot to override that configuration for a work repo, I sometimes need to revert a rebase.

## Enter the Reflog

The git reflog is a record of what the tip, or HEAD, of your local repo pointed to, every time you change it (commit, pull, chekcout, merge, etc). And you can use it to return to each of those states.

So, to get back to the HEAD of my repo prior to a rebase, I can simply run `git reflog` and find the last change to HEAD prior to where I made my boo-boo,then reset to that point via `git reset --hard HEAD@{2}` or whichever number it may be.

It's not perfect. If I've made other changes since I moved HEAD, I'll lose them, but more often than not, I realise my mistake as soon as I hit enter on the command, so I can simple reset immediately, and then move forward as needed.

## Summary

* `git reflog` is a history of HEAD
* You can get back to any of those with `git reset --hard HEAD@{2}`
