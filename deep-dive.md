# GIT Deep Dive

The GIT deep dive talk will showcase and cover the inner workings of GIT, in order to create a better understanding. In turn this should lead to more confidence in using GIT, both from the standpoint of using its tools as well as using it from the command line.

> Presentation note: for the full effect it is recommended to have multiple terminals open, one where the actual demo is held. Plus some which show additional data on the repository (using a watcher). Examples are, git status, filesystem, git log.

## Outline

### Setup

The setup is quick and simple, create a new 'deep-dive' directory and git init in here. Let the audience know that git repos can be nested, the first parent directory containing a `.git` dir will be used to determine which repo you are in.

```sh
rm -rf deep-dive && \
mkdir $_ && \
cd $_ && \
git init
```

At this point one might show proof of the existence of the `.git` directory.

### Commits are immutable

_Concept: Make a commit, show what it is, amend it, show the new commit is not the old commit._

```sh
echo "Hello World\!" > motd.txt
git add -Av && git commit -m "Add a message of the day file"
git log -1
echo "Goodbye instead\!" > motd.txt
git add -Av
git commit --amend --no-edit
git log -1
```

In between use the `git show` command to show the contents of the commits. After creating both commits, show both commits still exist. There's just only one showing in the log. The commit message is the same, so is the author, we didn't edit those. The hashes, timestamps and diffs do differ.

### Reflog

_Concept: Show the old commit still exists_

Show the ref log, the old commit hash can still be found there. The HEAD used to point to that commit and has been stored as being so. The current HEAD is the same as master and points to the new commit.

### Refs

_Concept: move the HEAD to the old commit, create a branch_

Show the contents of the .git/refs directory. Names should be familiar, heads being the heads or "end commits" of each branch. And tags being what they are, tags.  
Inside the heads directory should be a master ref, which is for the master branch.

```sh
git branch new-branch
```

Show the new branch exists (shows up in `git branch`) as well as in the git log (branch ref is on the same commit as HEAD). The branch can now be removed by removing the ref.  
So how do branches work. Each commit has a reference to its parent commit. This can be shown by using the command

```sh
git cat-file -p <hash>
```

> Maybe show the reflog again?

_Additional concept: manually change a branch_

```sh
git rev-parse <hash>
```

### Squashing and cherry-picking

_Concept: show how to rewrite history_


### Change sets

_Concept: how do change sets work? Create A, Create B, Create C. Go back to A, cherry-pick C. There's now A and C without B. Add content to A; make a patch from the change; undo; Commit different content to A. Revert A to earlier commit, commit this revert. Apply patch, should work as intended as change can be applied.
