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
