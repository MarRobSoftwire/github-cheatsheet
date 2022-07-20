# Git cheatsheet

General list of all commands used regularly, possibly some errors here and there.

` git clone <repo-link> ` to create local copy of remote repo 

`git status` to check the staging area & more before add/commit/pushing

`git add <file>`to add to staging area
 - `-A` to add all changes, probably bad practice though

`git commit -m <message>` to add message

`git push` to push to github

 - `-f` to force push **ONLY USE WHEN REBASING**

 - `--set-upstream origin <branch-name>` to create new upstream branch

`git log` to view log of commits - q to exit

`git pull` to update local files

`git stash` to Store current commit & switch to last commit

- `apply` to apply the stash

- `drop` to delete the stash


`git checkout <branch-name>` to move branch

 - `-b` to make new branch

    To checkout a previous commit, use git log and checkout the long commit hash (e.g. `git checkout e32172ac35d1fa0f0270236d53bf2bddc617ffa4`)

`git branch`to see branches

`git merge <brange-name>` to merge branches

`git rebase <new-base>` to rebase current branch

  - `--continue` to continue the rebase after fixing merge conflicts


## Adding to remote github

- `add`

- `commit`

- `push`

## Rebase a branch

- `checkout <branch-to-rebase>` 

- `rebase <new-base>`

- _Fix conflicts_

- `rebase --continue`