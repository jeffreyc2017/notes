
# Cheatsheet

## Linux

### Ubuntu

#### Shut down the computer

```bash
sudo shutdown now
sudo poweroff
sudo halt -p
```

## MacOS

### ESC

To invoke Escape, press Command + period (.) on the keyboard.

## git

[git](https://git-scm.com/)

```sh
# Get the list of local branches
git branch

# Get the list of remote branches
git branch -r

# The list of branches with their remote tracking branch
git branch -vv

# fetches all the branches and commits from the remote repository. It updates the local repository, and the -p flag tells git to remove remote-tracking references (i.e., origin/branch-name) that no longer exist on the remote repository.
git fetch -p

# Delete git branches that do not exist on remote
# https://www.wisdomgeek.com/development/delete-git-branches-that-do-not-exist-on-remote/
git fetch -p && git branch -vv | awk '/: gone]/{print $1}' | xargs git branch -d

# Commit
git commit -m "Updated env"
git push

# Merge write-db into main branch
git status
git checkout main
git merge write-db
git push

# Delete local and remote branches
git branch -d write-db
git push origin --delete write-db
```
