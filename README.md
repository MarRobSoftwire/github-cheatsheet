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
- `git stash` to Store current commit & switch to last commit
  - `apply` to apply the stash
  - `drop` to delete the stash
- `git checkout <branch-name>` to move branch
  - `-b` to make new branch
  - To checkout a previous commit, use git log and checkout the long commit hash (e.g. `git checkout e32172ac35d1fa0f0270236d53bf2bddc617ffa4`)
- `git branch`to see branches
  - `-a` to see remote branches too 
- `git fetch` to get all new commits & update list of remote branches
- `git merge <dev-brange-name>` to merge branches
  - You typically should be on the main branch
- `git rebase <new-base>` to rebase current branch
  - `--continue` to continue the rebase after fixing merge conflicts
- `git reset --soft <commit-hash>` revert to commit & stash all changes upto this point
  - **REMEMBER**`--soft` to stash changes. **This is a change of history! Avoid using!**

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
