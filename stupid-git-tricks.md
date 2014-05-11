# Stupid Git Tricks

You know those git commands you keep needing to lookup? Yeah, that's this.

The [Git Cheat Sheet](http://byte.kde.org/~zrusin/git/git-cheat-sheet-medium.png) is another good place to look.

## Truncate Git History

Truncate all history prior to a commit. Taken from: http://bogdan.org.ua/2011/03/28/how-to-truncate-git-history-sample-script-included.html

```sh
git checkout --orphan temp $REFSPEC
git commit -m "Truncated history"
git rebase --onto temp $REFSPEC master
git branch -D temp
```

where $REFSPEC is either a commit's hash or a tag.

## Add Deleted Files

```sh
git add -u
```

## Add Specific Lines From a File

```sh
git add -p $FILE
```

This will scroll through the file, stopped at each "hunk" of changed lines and allowing you to perform a few actions (including "y" for add). So you can commit unrelated sets of changes that happened at the same time in a single file.

## Discard Changes in Working Directory

Git already tells you this in the `git status` message but just in case:

```sh
git checkout -- $FILE...
```

This essentially means "checkout copies of the listed files from the current branch" which necessarily means any changes made since the last commit will be reverted.

## Revert to Previous Commit

Lose all local changes, go back to $REFSPEC (commit's hash):

```sh
git reset --hard $REFSPEC
```

Other ways to revert with more explanation at http://stackoverflow.com/questions/4114095/git-revert-to-previous-commit-how

## Creating a Github Page

See https://help.github.com/articles/creating-project-pages-manually

```sh
# checkout an orphan branch named gh-pages, then clear the working directory
git checkout --orphan gh-pages
# first commit goes here...
git push origin gh-pages
```

## Change Last Commit Message

```sh
git commit --amend -m "new message"
```

Only do _before pushing_ to remote repo.

## Put an Exclamation Point in a Commit Message

`!` is a special character in the Bash shell so it'll get interpreted if you write it inside double quotes.

```sh
git commit -m 'use single quotes!'
# or
git commit -m "mix your quotes"'!'
```

## Merging

### Merge Changes from Specific Files on Another Branch

```sh
git checkout $BRANCH filename1.txt filename2.txt
```

This checks out not the `$BRANCH` itself, but just those specific files so you can easily import the changes into another branch. Easy way to keep things synchronized across branches that are divergent elsewhere.

### Only Fast-Forward

A fast-forward merge means, essentially, there are no conflicts. The branch being merged in has all the changes from the other branch, it just has a few additional commits on top. You can safely `git merge` only a fast-forward merge with:

```sh
git merge --ff-only $BRANCH
```

where `$BRANCH` is the branch you're merging into the branch you're currently on.

### Force a Merge Commit

On the other hand, maybe you want a merge commit no matter what, for historical purposes.

```sh
git merge --no-ff $BRANCH
```

### Rebase

Perhaps the best way to sync a feature branch with changes in master is using `rebase`:

```sh
git checkout feature
git rebase master
# rewinds HEAD, replays changes from master & then new commits on feature
```

Squashing multiple commits together with an interactive rebase is also a very useful tool.

## Diffs

By words & not by lines:

```sh
git diff --word-diff
```

So instead of a lot of +/- lines which were actually just modified, you see which words were changed on a line.

You can use `git show #REFSPEC` to see the full diff for commit `$REFSPEC`.

## Logs

The `--decorate` flag will annotate each log with which branches & tags were involved.

The `-p` flag adds diffs to each log entry.

For the commits involving a particular file (& its diff, if you use `-p`) just pass the file(s) as a parameter at the end the command.

To see logs & trees in a web view, run:

```sh
git instaweb -d webrick --start && open http://localhost:1234
```

which will open a browser window for viewing your project. The `-d` flag specifies which HTTP daemon to use, by default it's lighttpd but Mac's can use webrick which comes with Ruby. To stop serving files, run the `instaweb` command but with the `--stop` flag.
