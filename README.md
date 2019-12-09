# Git bisect demo

## Setup

We have two scripts in this repo.

- app.sh (this is our app, if it works it outputs something otherwise it outputs nothing)
- bisect-script.sh (this is the script we'll use to find the bug!)

## Instructions

At the current commit, our app is in a broken state, we've made lots of changes since the last time it was working and we don't want to look through all those commits manually to find out what broke it.

We'll use git bisect to help us.

We then need to give git bisect two informations, a good commit and a bad commit.

If we look at our git history (using **git log**)

```
commit 469f767f3b9bb5b6834a2318048bdb15f8ac3e8a (HEAD -> master)
Author: Antonio <redacted@gmail.com>
Date:   Mon Dec 9 18:32:03 2019 +0100

    Adds bisect script

commit ed3c23ba90be7f53790d6527bf64e7492817652d
Author: Antonio <redacted@gmail.com>
Date:   Mon Dec 9 18:28:05 2019 +0100

    Adds an useless commit

commit 86dc2360424dda19e3b60ac5af7e7dc2bdb9dcac
Author: Antonio <redacted@gmail.com>
Date:   Mon Dec 9 18:27:47 2019 +0100

    Adds a readme

commit bf11e29d8dccf14d19bcdba399c9cff844f009b4
Author: Antonio <redacted@gmail.com>
Date:   Mon Dec 9 18:27:17 2019 +0100

    Break the app

commit 9b0ca8a81736f7b5f6ea1fde53728820de72cfaf
Author: Antonio <redacted@gmail.com>
Date:   Mon Dec 9 18:26:51 2019 +0100

    First commit
```

we can find the hash of a working commit (first commmit: 9b0ca8a8173)
and the hash of a broken commit, let's say "Adds bisect script: 469f767f3b9bb5"

Let's run a few simple commands to start playing with git bisect

Let's create a script to automate git bisect.

This script needs to do two things:
- Exit 0 if the commit is "good"
- Exit 2 if the commit is "bad"

```Bash
#!/usr/bin/env bash

OUTPUT=$(./app.sh)

if [ -z "$OUTPUT" ]; then
  # The app is broken since the output is empty
  echo "This commit is broken"
  exit 2
else
  # The app works just fine
  echo "Everything works!"
  exit 0
fi;
```
