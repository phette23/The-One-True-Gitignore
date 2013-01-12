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

Git already tells you this in the ```git status``` message but just in case:

```bash
git checkout -- <file>...
```

This essentially means "checkout copies of the listed files from the current branch" which necessarily means any changes made since the last commit will be reverted.
