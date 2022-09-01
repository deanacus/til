# Handy GitConfig Tricks

What follows is a collection of handy snippets for your `.gitconfig` file that
can make your life with git a little bit easier.

## Conditional Includes

At my day job, our git setup is different enough from the way I like to use git
on my own projects, that it makes sense to manage the config for them
separately. I could, of course, run the requisite `git config` commands in each
repo, however it gets tiring after a while, and it is inevitable that I will
forget to do it at some point. Thankfully, since git 2.13, the ability to
conditionally include config files based on the `$GIT_DIR` path has existed.

Given that I already keep my work repos separate from my personal ones (`~/work`
and `~/dev`), it was pretty simple to add a single line to my config to include
a work specific configuration file that inherited from the base config, and
adjusted anything that needed to be changed.

```
[includeIf "gitdir:~/work/"]
  path = ~/dotfiles/git/gitconfig.work
```

I can then keep my personal git habits separate from my work habits.

## URL Substitions and Shortcuts

One thing that we do at work is enforce all interactions with the remote host to
happen via `ssh`. Personally, I like it a lot, but it seems that a great number
of people seem to prefer https, which means that copying `git clone` commands
from various places means that I need to rewrite them for a subset of repos. But
with the `url.<base>.insteadOf` setting, I can just tell git to always enforce
ssh, regardless of what I type/paste:

```
[url "git://"]
  insteadOf = https://

[url "git@github.com:"]
  insteadOf = https://github.com/

[url "git@bitbucket.org:"]
  insteadOf = https://bitbucket.org/
```

It also can be a bit tedious to always type out the complete URL to a repo to
clone it, so some shortcuts are handy to have around. Using the same
configuration options, we can set up some handy shortcuts, like being able to
clone my own repos by typing `git clone me:<repo>`:

```
[url "git@github.com:deanacus/"]
  insteadOf = me:
```

I even have these set up for different hosts, such as github, bitbucket, and
even one for work:

```
[url "git@github.com:"]
  insteadOf = gh:

[url "git@bitbucket.org:"]
  insteadOf = bb:

[url "git@bitbucket.org:<work>/"]
  insteadOf = work:
```

Now I can clone any github repo by typing `git clone gh:<user>/<repo>`.

## Handy Aliases

By this point, I'm relatively sure that everyone and their dog knows about the
commong git aliases like `st` for status and `co` for checkout, but you can make
them a low more useful than just saving keystrokes.

Something that I've run into from time to time is needing to cleanup merged
branches that people have forgotten (or neglected) to delete when their PR has
been merged. To get a list of branches that have been merged into master, you
can try to remember how to type
`git for-each-ref --sort=-committerdate --merged=master refs/remotes --format=<whatever_format>`,
or you can make that an alias and just type `git merged master`:

```
[alias]
  merged = "for-each-ref --sort=-committerdate --format=\"%(refname:lstrip=3)%(color:dim) - %(authorname) (%(authordate:short))\" refs/remotes --merged"
```
