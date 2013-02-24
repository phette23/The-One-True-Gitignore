# Stupid Git Tricks

You know those git commands you keep needing to lookup? Yeah, that's this.

## Truncate Git History

Truncate all history prior to a commit. Taken from: http://bogdan.org.ua/2011/03/28/how-to-truncate-git-history-sample-script-included.html

```bash
git checkout --orphan temp $REFSPEC
git commit -m "Truncated history"
git rebase --onto temp $REFSPEC master
git branch -D temp
```

where $REFSPEC is either a commit's hash or a tag.

## Add Deleted Files

```bash
git add -u
```

## Discard Changes in Working Directory

Git already tells you this in the `git status` message but just in case:

```bash
git checkout -- <file>...
```

This essentially means "checkout copies of the listed files from the current branch" which necessarily means any changes made since the last commit will be reverted.

## Revert to Previous Commit

Lose all local changes, go back to $REFSPEC (commit's hash):

```bash
git reset --hard $REFSPEC
```

Other ways to revert with more explanation at http://stackoverflow.com/questions/4114095/git-revert-to-previous-commit-how

## Merge Changes from Specific Files on Another Branch

```bash
git checkout $BRANCH filename1.txt filename2.txt
```

This checks out not the `$BRANCH` itself, but just those specific files so you can easily import the changes into another branch. Easy way to keep things synchronized across branches that are divergent elsewhere.

## Creating a Github Page

See https://help.github.com/articles/creating-project-pages-manually

```bash
# checkout an orphan branch named gh-pages, then clear the working directory
git checkout --orphan gh-pages && g rm -rf .
# first commit goes here...
git push origin gh-pages
```
