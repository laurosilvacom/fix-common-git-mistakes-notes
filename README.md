# Fix Common Git Mistakes

These notes are intended to be used and studied in tandem with [Chris Achard](https://egghead.io/instructors/chris-achard)'s [course](https://egghead.io/courses/fix-common-git-mistakes). If you notice areas that could be improved please feel free to open a PR!

Download a PDF copy here! Enjoy!

## Course Summary

- Change commit messages
- Add or remove files from a commit
- How and when to stash changes
- What a "detached HEAD" is, and how to fix it
- Remove secrets from a codebase
- How to rewrite history

## Original Code Snippets

### 1.Change a Commit Message that Hasn't Been Pushed Yet

```bash
git commit --amend -m "New message"
```

### 2. Add More Files and Changes to a Commit Before Pushing

```bash
# only do this BEFORE you've pushed the commits
git add -A

git commit --amend -m "My new message"
```

### 3. Remove Files from Staging Before Committing

```bash
git reset HEAD filename
```

### 4. Remove Changes from a Commit Before Pushing

```bash
# reset back to a specific commit
git reset HEAD~1
# or
git reset [HASH]
```

### 5. The Different git Reset Options: --hard, --soft, and --mixed

```bash
# Resets the index and working tree
git reset --hard [commit]

# Does not touch the index file or the working tree at all
git reset --soft [commit]

# Resets the index but not the working tree
# Reports what has not been updated
# This is the default action
git reset --mixed [commit]
```

### 6. Recover Local Changes from `git reset --hard` with `git reflog`

```bash
# To look up the commit hash
git reflog

git reset --hard [HASH]
```

### 7. Undo a Commit that has Already Been Pushed

```bash
# NOTE: Once a commit is pushed, you do NOT want to use git reset
# make a "revert commit" which will "undo" a specific commit
git revert [HASH-TO-UNDO]
```

### 8. Push a New Branch to GitHub that Doesn't Exist Remotely Yet

```bash
git checkout -b new-branch

git push

#  set the upstream of the local branch at the same time
git push --set-upstream origin new-branch
```

### 9. Copy a Commit from One Branch to Another

```bash
git cherry-pick [HASH-TO-MOVE]
```

### 10. Move a Commit that was Committed on the Wrong Branch

```bash
# Get the commit we want
git cherry-pick [HASH-TO-MOVE]

# Remove the commit from the wrong  branch
git reset [HASH-TO-REMOVE]
```

### 11. Use `git stash` to Save Local Changes While Pulling

```bash
# Save the local changes,
git stash

# Get remote changes
git pull

# To apply the stashed changed
git stash pop

# You will need to  fix the merge conflict
# Then drop the change from the stash
git stash drop stash@{0}
```

### 12. Explore Old Commits with a Detached HEAD, and then Recover

```bash
# checkout the hash of an old commit
git checkout [HASH]

# we'll be in a "detached HEAD" state
# Save the work by creating a new branch
git checkout -b my-new-branch
```

### 13. Fix a Pull Request that has a Merge Conflict

```bash
git checkout -b conflicts_branch

# Add 'Line4' and 'Line5'

git commit -am "add line4 and line5"
git push origin conflicts_branch

git checkout master

# Add 'Line6' and 'Line7'`
git commit -am "add line6 and line7"
git push origin master
```

### 14.Cleanup and Delete Branches After a Pull Request

```bash
# use the github interface to delete the branch remotely

# Locally
# Confirm that remote is gone
git remote prune origin --dry-run
git remote prune origin

#clean up the feature branch
git branch -d feature-branch
```

### 15. Change the Commit Message of a Previous Commit with Interactive Rebase

```bash
git log --oneline

# start the interactive rebase
git rebase -i HEAD~3
# and then change pick to reword.
# We can now reword the commit message
```

### 16. git Ignore a File that has Already been Committed and Pushed

```bash
# We make a file and accidentally push it to github
# To remove it, add it to .gitignore file
# remove all of our files from our git cache
git rm -r --cached .

# add back all the files we want with
git add -A
```

### 17. Add a File to a Previous Commit with Interactive Rebase

```bash
git rebase -i HEAD~2

# during the interactive rebase, we can add the file, and amend the commi
git commit --amend --no-edit

git rebase --continue
```

### 18. Fix Merge Conflicts While Changing Commits During an Interactive Rebase

```bash
# ennter interactive rebase
git rebase -i HEAD~2

# Then we can fix that merge conflict like normal, but finish up the rebase
git rebase --continue
```

### 19. Squash Commits Before they are Pushed with Interactive Rebase

```bash
git rebase -i HEAD~3

# Make the changes in interactive rebase
# Make the commit message for that commit, and once we save the message
# we'll be left with just a single commit
```

### 20. Completely Remove a File from Pushed git History

```bash
# prune the entire history and garbage collect the remains
git reflog expire --expire=now --all && git gc --prune=now --aggressive

#  use git push to push that change to github, and remove the .env file from all of the history
```
