# Git cheatsheet

## Commands
- `git help <command>` get docs
- ` git clone <repo-link> ` to create local copy of remote repo 
- `git status` to check the staging area & more before add/commit/pushing
- `git add <file>`to add to staging area
  - `-A` to add all changes, probably bad practice though
- `git commit -m <message>` to add message
- `git push` to push to github
  - `-f` to force push **ONLY USE WHEN REBASING**
  - `--set-upstream origin <branch-name>` to create new upstream branch
- `git log` to view log of commits - q to exit
- `git pull` to update local files
- `git stash` to append current commit in a stack & switch to last commit
  - `pop` applies the latest in the stack
  - `apply` to apply the stash but leave it in the stack
  - `drop` to delete the most recent in the stack
  - `delete` to delete the whole stack
- `git checkout <branch-name>` to move branch
  - `-b` to make new branch
  - To checkout a previous commit, use git log and checkout the long commit hash (e.g. `git checkout e32172ac35d1fa0f0270236d53bf2bddc617ffa4`)
  - `checkout <file-name>` resets the file to the previous commit 
- `git branch`to see branches
  - `-a` to see remote branches too
  - `-m <new-name>` renames the current branch
- `git fetch` to get all new commits & update list of remote branches
- `git merge <dev-brange-name>` to merge branches
  - You typically should be on the main branch
- `git rebase <new-base>` to rebase current branch
  - `--continue` to continue the rebase after fixing merge conflicts
  - `-i` is an interactive rebase _see below_
- `git reset --soft <commit-hash>` revert to commit & stash all changes upto this point
  - **REMEMBER**`--soft` to stash changes. **This is a change of history! Avoid using!**
- `git revert` appends a new commit that undoes the previous - this is not a change of history
- `git diff` shows difference between current files & latest commit
  - `-staged` compares staged changes & latest commit
- `git cherry-pick` takes a commit (usually from another branch) and append it to the current working HEAD
- `git bisect` runs a binary search to isolate where a breaking change has been implemented - see below

## Walkthroughs

### Initialise empty repo
Create a remote on github, a local directory & run the following
```shell
$ git init
$ git add <new-files>
$ git commit -m "init"
$ git remote add origin <remote-link>
$ git push -u origin main
```

### Adding to remote repo
```shell
$ git add <files>
$ git commit -m "updates"
$ git push
```
### Rebase a branch

```shell
$ checkout <branch-to-rebase>
$ `rebase <new-base>
```
   _Fix conflicts_
```bash
$ rebase --continue
```
And repeat

### Rebasing onto main after branching off a feature
Branch B was branched off A. To rebase branch B onto main:
```shell
$ git checkout main
$ git pull
$ git checkout A
$ git pull
$ git checkout B
$ git rebase A
$ git rebase --onto main A
```

### Interactive Rebase
Opens a text editor (vim, nano, etc.) in order to carry out a rebase.
The top lines are the instructions for a rebase, in the order that they will be applied. For example
```
<command> <hash> <commit-message>
pick 1558262 Update README.md
```
Below this is a list of commands and a short explanation. The important ones are
- `pick` uses the commit as it was initally applied
- `reword` changes the commit message only
- `edit` pauses the rebase at this point to allow edits to the code
- `squash` like pick, but squashes this commit into the previous

### Bisect
```shell
$ git bisect start
$ git bisect good <commit>
$ git bisect bad <commit>
Bisecting: n revisions left to test after this (roughly n steps)
[hash] <commit message>
```
This begins a binary search, to isolate the commit where a bug appears. At each step declare either good or bad & a new commit will be provided to test
### Setting up a public and private github repo
Adapted from this [guide](https://24ways.org/2013/keeping-parts-of-your-codebase-private-on-github/) 

Firstly create two github repositories `repo-name` and `private-repo-name`

As normal set up `main` to track the public repo
```shell
$ git remote add origin git@github.com:[user]/repo-name.com.git
$ git push -u origin main
```
Then set up the `dev` branch to track the private repo
```shell
$ git checkout -b dev
$ git remote add private git@github.com:[user]/private-repo-name.com.git
$ git push -u private dev
```
This will give one local repo with two branches that push to different remote repos.

### Pull Request Refs
When you make a PR on github, a merge commit is created at the ref `refs/remotes/pull/PR-NUMBER/merge` on origin. In order to access this you can
- Pull the refs with `git fetch origin +refs/pull/*:refs/remotes/origin/pr/*`
- Checkout the commit at `origin/pr/PR_NUMBER/merge`
Github Actions will use this commit by default, so any CI tests will be running on this commit, not your branch.
