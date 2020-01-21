# GIT COMMANDS

## KEY:

```bash
<remote> # the alias of your remote repo "origin", "upstream", etc...
<branch> # the name of the branch
<filePath> # the relative file path from your current directory ex: './server/index.js'
```

## Commands for moving data between remote and local repos

### Pull from remote repo

```bash
git pull <remote> <branch>
```

Example:
Get the latest dev branch from github

```bash
git checkout dev
git pull origin dev
```

### Push to remote repo

```bash
git push <remote> <branch>
```

## Branch Commands

### Move to a branch

```bash
git checkout <branch>
```

Note:
Checking out a file will replace the current file with the last commited version. Use this if you totally screwed up your code and want to reset it to how it was without nuking all the other files you changed.

```bash
git checkout <filepath>
```

You can checkout files/directories from other branch as well.

### Create a new branch and move to it

```bash
git checkout -b <branch>
```

### Delete a branch (make sure you're not on the branch)

```bash
git branch -D <branch>
```

### Delete branch on github

```bash
# the ':' is not a typo
git push <remote> :<branch>
or
git push <remote> --delete <branch>.
```

### Checkout a branch from github

```bash
git fetch <remote>
git checkout -t <remote>/<branch>
```

Example:
Your team mate pushes up a branch to the group repo and wants your help on it.

```bash
git fetch origin
git checkout -t origin/someBranch
```

## Combining Branches

Merge A Branch into another branch:
Navigate to the branch you want to merge into and run

```bash
git merge <branch>
```

Example:

```bash
git checkout -b myFeatureBugFix
# ...fix the bug and commit changes
git checkout myFeature
git merge myFeatureBugFix
```

This will merge the myFeatureBugFix into the myFeature branch.
A Pull Request on github is essentially a merge.

Rebase a branch on top of another branch:
This command will shove the commit history from another branch underneath the current working branch.

```
Before rebasing                           After rebasing

(Dev Branch HEAD)
C                             (Dev Branch HEAD) C --  D -- E (your branch HEAD)
|                                               |
B -- D -- E (your branch HEAD)                  B
|                                               |
A                                               A
```

```bash
git rebase <branch>
```

Example:
Say you are working on a feature and your team mates have pushed a bunch of commits to the dev branch.
You want to make sure that you have the latest code but you aren't ready to merge your code into the dev branch just yet.

```bash
# ...stash or commit your work

# get the latest dev branch from github
git checkout dev
git pull origin dev
# rebase your code
git checkout myFeatureBranch
git rebase dev
```

### NEVER USE REBASE ON A SHARED BRANCH (dev, master, etc..)

Don't use rebase unless you understand all of its implications.  
https://kolosek.com/git-merge-vs-rebase/  
https://brandonwamboldt.ca/understanding-git-rebase-1670/

You can also use rebase to squash your commit history using interactive mode:

```bash
git rebase -i HEAD~<number>
```

Example:
You want to squash the last 3 commits into one commit (retaining the most recently commited files)

```bash
git rebase -i HEAD~3
```

This message will pop up:

```
pick 0bd0f01 commit 1
pick 81ccc27 commit 2
pick 8e77f1e commit 3
```

change all but the first commit to 'squash'

```
pick 0bd0f01 commit 1
squash 81ccc27 commit 2
squash 8e77f1e commit 3
```

Once you squash the commits a new message will appear to create a new commit message. This command is helpful for tidying up commit history before merging your feature into the main code base.

### Stash:

Acts like cut/paste for unstaged code

```bash
# cut
git stash
# paste
git stash pop
```

Examples:

- You are working on a feature but you need to switch over to another branch to do something else and youre not ready to commit anything.
- You are working in the wrong branch. Stash your code, move to correct branch, and pop it in.

## Oopsie commands:

### Revert:

Use this command if a branch needs to be rolled back to a previous commit without modifying the previous commit history.

```bash
git revert HEAD
```

A - B - C (HEAD)
after revert
A - B - C - B' (HEAD)

Example:
Code wasnt properly tested before merging to dev and broke some api routes. The team doesnt have time to wait for a fix and needs to deploy other features. Use git revert on the dev branch and keep moving.

### Reset soft/hard

### NOTE: Never use reset on the dev or master branch

Soft reset will roll back to a previous commit while keeping your most recent changes staged (like you did a 'git add' but didnt commit yet)
hard reset will roll back to a previous commit and delete all changes made after that commit

```bash
git reset --soft HEAD~<number>
git reset --hard HEAD~<number>
```

Examples:

- Your last 2 commits are complete trash and you want to delete them from existence

```bash
git reset --hard HEAD~2
```

- You accidentally commited when you weren't ready to:

```bash
git reset --soft HEAD~1
```

### Reset staged files

```bash
git reset HEAD
or
git reset <filepath>
```

Example:

- You staged a bunch of files but havent commited yet. You want to unstage all the files:

```bash
git reset HEAD
```

- You accidentally staged a file that you wanted to ignore (like .env or node_modules):
  ```bash
  git reset ./.env
  ```

## VIM Basics

Vim is the default text editor git uses. It has 3 modes: Normal, Insert, and Visual.
The 2 modes you should know how to use are Normal and Insert.

To get to normal mode from another mode press esc.  
While in normal mode you can move your cursor using h,j,k,l

h = left  
j = down  
k = up  
l = right

When you move the cursor to where you want it press 'i' to switch to insert mode.  
You will see --INSERT-- at the bottom of your screen to indicate you are in insert mode.  
While in insert mode you can type and move the cursor using the arrow keys.
Press esc to go back to normal mode when you are done editing.

To exit vim press shift + : (while in normal mode) to enter a command. You will see a ':' on the bottom of the terminal.
Type one of the following to exit vim:  
q = quit without saving  
wq = save and quit
