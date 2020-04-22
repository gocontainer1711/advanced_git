
## visualise git online 

[https://git-school.github.io/visualizing-git/#free-remote](https://git-school.github.io/visualizing-git/#free-remote)


## Setting up VScode for git diff

`code desktop/workdir` to open up a directory in VSCode


`git config --global -e` to view your global config file
`git config --global core.editor code` to set the core git editor as vs code

```
[user]
	name = xyz
	email = xyz@gmail.com
[core]
	excludesfile = /Users/anandujjwal/.gitignore_global
	editor = code -w
[difftool "sourcetree"]
	cmd = opendiff \"$LOCAL\" \"$REMOTE\"
	path = 
[mergetool "sourcetree"]
	cmd = /Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\" -ancestor \"$BASE\" -merge \"$MERGED\"
	trustExitCode = true

```



**diff tool setup**

`git config --global --get diff.tool`

`git config --global diff.tool default-difftool`

```
[diff]
   tool = default-difftool
[difftool "default-difftool"]
   cmd = code --wait --diff $LOCAL $REMOTE  
```


To turn off the prompt while git difftool

`git config --global difftool.prompt false`

Now to see diff between two commits 

`git difftool a379faa  a6f8fva`


To see the latest diff of a file


`git difftool index.html`


To see in terminal

`git diff HEAD`
`git diff index.html`


**merge tool setup**

`git config --global merge.tool code`

`git config --global mergetool.code.cmd "code --wait $MERGED"`

`git config --global mergetool.prompt false`

`git config --global mergetool.keepbackup false`





## Setting up existing local repository to remote repo at github


 
`git init`

`git remote -v` to check the remote

`git remote add origin {repo-url}`





## Other commands inspect more

```
git reset -- hard
```

```
git reset -- hard abcxyz12

```

```
git lg
``` 

```

git remote -v
```

```
git log --oneline
```


```
git fetch origin --prune
```


```
git checkout --orphan assets # without any prior commits
```

```
git add xyz.js

git reset xyz.js
```

```
git commit -m "test" --allow-empty

# will result in a new commit id that is without any changes
```


```
git commit -am "check this option out"

```

```
git log --oneline --graph --color --all --decorate
```

```
git status -s
```


```
git diff
```


```
git diff HEAD~1 HEAD   
```

```
git diff HEAD~1 HEAD filename.js
```


```
git diff --staged   
```

```
git diff --cached   
```

```
git diff --cached  --submodule 
```


```
git commit --amend
```

```
git commit --amend --no-edit 
```

```
git log --pretty=oneline

git log --oneline --graph --decorate
```

```
git push
```

```
git fetch
```


```
git init new_repo
```





#Difference between clone and fork and pull request and merge request ??
# Branching strategy, industry standards?
# Difference between fetch and pull?
# Adding and managing multiple origins
# Difference between a submodule and subtree
# SSH to git from a mac
# More on git ignore
# What is index? difference between staging and index
# garbage collection in git
#Post receive hook and others too.
#Difference between cherrypick and rebase
# Git reset ??
# cherry pick to pick just one commit only ?
#Local tool to visualise git
#Aliases without git config with zsh basically
#Simulate a diversion while practising rebase




<hr>
<hr>

# Git Amend

**When you need to modify something(code or message) of last commit. Use amend.**

Note:- Do it on your own branch. Never do it on public branches


To edit the message

```
git commit -m 'edited the last commit message' --amend

```

To edit/add/delete some file

```
git add .
git commit -m 'added a new file' --amend

```


Due to amend some dangling commits are created that are unreachable but still accessible in the git history.

Dangling commits can still be accessed using reflogs.

<hr>
<hr>

# Git reflogs

To see all the things done. (recorded history)

```
git reflog
```

To point the HEAD to a dangling commit

```
git checkout {dangling-commit-id-from-reflog}
```

**Starting the reflog from custom point**

```
git reflog HEAD@{20}

git reflog HEAD@{3.days.ago}

git reflog HEAD@{6.weeks.ago}

git reflog master@{6.weeks.ago}


git reflog master@{2.minutes.ago}

git reflog master@{2019-08-28}

```



Exploring reflog

```
git reflog --oneline
```


```
git diff HEAD@{2} HEAD@{5}

git difftool HEAD@{2} HEAD@{5}

git diff HEAD@{1.minute.ago} HEAD@{1.week.ago}
```


### Garbage cleaning unreachable commits

expire and Garbage collection

```
git reflog expire --expire-unreachable=now --all

git gc --prune=now
```


## Precautions with Squash and merge

Squash and merge is a very effective tool to compact your commits while merging your pull requests.
Precautions:

* Delete the merged branch immediatedly after squash and merge
* If you continue to work on the same branch , it may cause the commit history to diverge.
* You may see already merged commits in the future merge requests even when there is no file change creating a lot of confusion within the team.
* Squashed changes and un deleted branches may cause conflicts.

```
git fetch origin --prune
```

<hr>
<hr>

# Git Stash 

Stash is local and saved on hdd and can't be pushed to remote.

## Stash and pop without any option (default being save)

```
git stash

git pop
```

stash uses commits and refs under the hood ?

* **pop pulls one stash out and drops it from stash list**
## Stashing but keeping the index

##!_Need more theory_
When You wish to stash only the non staged changes.

```
git stash --keep-index

git pop
```
or

```
git stash -k
```
Try more ??

```
git stash -u

git stash -a

```

#### see details of a particular stash

```
git stash show stash@{0}

# notice how untracked stashed files react to above command
```

## Stashing the untracked files

```
git stash --include-untracked

git pop
```
or

```
git stash -u
```

## Listing all the stashes

```
git stash list
```

## Adding a comment to a stash

```
git stash save "custom message to describe this stash"

git stash save -u "custom message to describe another stash"
```

## Applying one stash from a list

Here 1 is the id of the stash

Note apply won't drop the stashed commit from the stack. You need to clean explicitly

```
git stash apply stash@{1}
```

## Stashing to a branch

This will switch to a new branch 'new_feature' and re apply the stashed changes.

This drops the stash from the list.

```
git stash save -k -u
git stash branch new_feature
git stash branch new_feature_2 stash@{2}
```

### Dropping stash one at a time

```
git stash drop stash@{1}

```

### Clearing the stash

```
git stash clear

```

Note: the dropped stash may be recovered from reflogs till you have pointers to them.

<hr>
<hr>

# Hard reset

```
git reset --hard {commit-id}
```

```
git reset --hard filename.txt
```

# Branching basic


## List all the branches
```
git branch
```

```
git branch --list
```


```
git lg
```

```
git branch --merge
git branch --no-merge
```

```
git checkout -b new-branch-to-work
```
or 

```
git branch new-branch-to-work
git checkout new-branch-to-work
```


# Head and other names

**What's a branch ?**


A branch is a labelled commit. Also When we talk about a branch , its not just the last commit but also all the commits prior to it.


**What's HEAD ?**

A head is just a pointer managed by git when we checkout to branches or do commits.


**What is a detached head ?**

When the head is no more linked to a branch, its caleed a detached head.When you checkout to a specific commit, you point to a detached head.


**Branches on a file system?**

Inspect .git folder and inspect files and play with them.


##To delete a branch

```
git branch -D name-to-be-deletd

```

##To rename a branch

```
git checkout branch-to-be-renamed

git branch -m "new-name-of-branch"
```

##To track the remote version of a branch

```
git checkout develop
git remote -v # to see the origin
git branch -u origin develop
```

##To untrack the remote version of a branch

```
git checkout develop
git branch --unset-upstream
```

##To list all the branches that contains a specific commit

```
git branch --contains {commit_id}
```

 <hr>
 <hr>
 
#Git merging under the hood

## Fast-forward or 3-way merge

**When Git creates a fast-forward merge?**

Simplest merge.

When you're merging branch feat into master.
It moves the label pointer of master from commit-x to commit-y.
Where commit-x is the ancestor of commit-x.


Suppose you are currently on master

```
git checkout -b feat
git commit -m 'testing' --allow-empty
git checkout master
git merge feat
```


**When Git creates a 3-way merge?**

```
git merge -s recursive branch1 branch2
```

[c0] <- [c1] <- [c2] <- [c4] 


[c0] <- [c1] <- [c2] <- [c3] <- [c5]  

master -> c4

feat -> c5

common Ancestor -> c2

Snapshot to merge into -> c4

Snapshot to merge in -> c5


```
gco -b 'feat'
git commit -m "feat-1" --allow-empty
gco master
git commit -m "master-1" --allow-empty


git log --oneline --graph --color --all --decorate


* 1e6b1d5 (HEAD -> master) master-1
| * fbb536f (feat) feat-1
|/   
| * aad5c7d index on master: 1f6ff37 Initial commit
|/  
* 1f6ff37 (origin/master, origin/HEAD) Initial commit



git merge feat
# We need a merge commit and a commit message was asked.
# Merge made by the 'recursive' strategy.
#This is notva fast forward merge

```



**When a 3-way merge causes conflicts?**

Changes in same line in the same file in case of a 3-way merge attempt may lead to conflicts which has to be resolved manually.



**Merging with different strategies**

Note:- We can configure the merging strategy.

`-s starategy`
`-X option`


ignore-all-space option would resolve white space conflicts automatically.
Risky with languages like python, yaml.

```
git merge -s recursive -X ignore-all-space
```

**Ours**

```
git merge -s ours branch1 branch2 branchN
```

The Ours strategy operates on multiple N number of branches. The output merge result is always that of the current branch HEAD. The "ours" term implies the preference effectively ignoring all changes from all other branches. It is intended to be used to combine history of similar feature branches.


**patience**

This option spends extra time to avoid mis-merges on unimportant matching lines. This options is best used when branches to be merged have extremely diverged.

and many more...


**Abort a merge**

```
git merge --abort
```


##Types of Git Merge Strategies

###Explicit Merges
Explicit merges are the default merge type. The 'explicit' part is that they create a new merge commit. This alters the commit history and explicitly shows where a merge was executed. The merge commit content is also explicit in the fact that it shows which commits were the parents of the merge commit. Some teams avoid explicit merges because arguably the merge commits add "noise" to the history of the project.

###implicit merge via rebase or fast-forward merge
Whereas explicit merges create a merge commit, implicit merges do not. An implicit merge takes a series of commits from a specified branch head and applies them to the top of a target branch. Implicit merges are triggered by rebase events, or fast forward merges. An implicit merge is an ad-hoc selection of commits from a specified branch.

###Squash on merge, generally without explicit merge
Another type of implicit merge is a squash. A squash can be performed during an interactive rebase. A squash merge will take the commits from a target branch and combine or squash them in to one commit. This commit is then appended to the HEAD of the merge base branch. A squash is commonly used to keep a 'clean history' during a merge. The target merge branch can have a verbose history of frequent commits. When squashed and merged the target branches commit history then becomes a singular squashed 'branch commit'. This technique is useful with git workflows that utilize feature branches.




## Signing a merge with GPG

#### Installing a GPG and creating your key

#### Signing merge and commits with public key

```
brew install gpg
gpg --list-keys
gpg --gen-key
```

```
gpg --list-secret-keys --keyid-format LONG


git config --global user.signingkey key
# git knows which key to use every time we commit

git commit -S -m "signed commit"
git merge -S other-branch

git log -n1 --show-signature
```

 <hr>
 <hr>

# Visually Managing your repositories

#### You can use gitk to tag, seeing diff, cherry-pick wtc..

**gitk is installed along with git**

start with `gitk` `gitk --all` for all the branches


 <hr>
 <hr>

# Rebase

###Differences between rebase and merge

Rebase is a way to change history. In rebase we mostly reapply our local changes to the top of the specified branch.
Rebase ensures a linear history.

Rebase creates new commits and old commits may now become dangling.

With git merge , we have two parents and the history is no more linear.
Suppose we are on develop and develop is behind the master.
Simplest thing to do would be 


```
git rebase {basebranch}

```

```
git rebase master
```
 
###Rebase with conflicts

**Things to understand :**

* Solving a conflict during a rebase
* Aborting a rebase
* Continuing a rebase

"git add/rm <conflicted_files>", then run
```
git rebase --continue
```

To abort and get back to the state before "git rebase", run
```
git rebase --abort
```

To skip this commit, run
```
git rebase --skip
```

Drawback of rebase:
Fix in all other commit and resolve conflict again and again.




###Rebase interactive - reword, squash, reorder and edit commits

Interactive rebase is very helpful only when you haven't pushed your code to the remote.
You can use this tool and push nice and clean changes.

**Things to understand :**

* Start using interactive rebase
* Changing message for old commits
* Editing old Commits



You just need to reorder the commit lines in the top of the rebase command output to actually reorder the commits.


```
git rebase -i {commit-id}

# here commit-id given must be the immediate parent commit of the commit you wanna fix (and not the immediate commit id)




# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.

```

 <hr>
 <hr>
 
 
# Cherry Picking

Its like rebase , brings in the changes of the referenced commit along with its ancestors.

```
git cherry-pick {commit-id}
```

If there are conflicts , you may resolve them and do a commit on top.

Cherrypick like rebase or revert creates new commit.



# Git tags

* Creating tags for important milestones
* Lightweight versus annotated tags
* Git tag options
* Git remote and git tag - do's and don'ts




Tag is like a bookmark for a special release event in time.


Command to create a tag

```
# git tag {tag-name}
git tag "v1.0.0"
```

###Difference between lightweight and annotated tag

Multiple tags with the same name can not exist.

**Light weight tag** contains tha name and the sha.

**Annotated tag** contains :

* name
* sha1
* message
* author
* date
* can be signed (gpg)



###Creating an annotated tag and Showing the message of an annotated tag


```
git tag -a "v2.0.0" -m "Rlease note: Has schedule API"

git tag  "v2.0.1" -m "Rlease note: Has schedule API 2.0.1"

git show "v2.0.0" # to see details of tag
```

to list tags by pattern

```
git tag -l "v1.0*"
```

### view tags
```
git tag
```
or
```
git tag --list
```

### delete tag

```
git tag -d "v1.0.0"
```


### update a tag

Note: Multiple tags with the same name can not exist.

```
git tag -f "v1.0.0"
```

### Fetch remote tags

```
git fetch --tags
```

### Push local tags
#### Always cross check before pushing tags

```
git push --tags
```

### Update already pushed tag

Note: A new tag should be pushed instead of update with a proper message to not use previous pushed faulty tag
beacuse some team member could already have fetched that tag and even if you update..
fetching it for other team members would be an issue ??

Same tag on different locals would start pointing to different commits , creating confusions.

```
git tag "v1.0.1" -m "Please use v1.0.1 instead of v1.0.0"
```

### You can checkout to a tag just like a branch

```
git checkout "v1.0.1"
```

### Delete a Remote tag

this syntax is bit weird 

```
git push origin  :v1.0.0
```

### create a tag from commit-id

```
git tag -a randomTag -m 'testing tag' dce70f5
```

### Config setting to automatically push tags on creation

```
git config --global push.followTags true
```



 <hr>
 <hr>

# Git Submodules

* Create and update a Submodule
* Use a repository with submodule
* Git Subtree -- adding a subtree and keeping it updated

### Why need a submodule

* Nested Repo
* Dependency Management
* Boiler plate helper code



### Adding a submodule to our repository

This adds a hidden file '.gitmodules'

```
git submodule add git@github.com:{rest-of-url}.git
```


### Clone and use a repository with submodules

on clone submodule folder will be empty
Run:

```
git submodule init
git submodule update
```

or clone with `--recursive flag`


### Updating the dependencies from the main repo

Change directory and update. The two are different git repos and hence their changes are tracked differently.

1. Change dir and update code in submodule
2. push from submodule directory itself
3. Come out to main repo
4. Commit the ref and push it inside the main repo.

In case you want to update the submodule folder recently updated by some other team member in your local.

1. git pull in main repo , submodule refs will update
2. run git submodule update

 <hr>
 <hr>

# Git Subtree

### Adding the subtree

```
git clone {main-repo-git-url}.git
cd main-repo
git remote add -f subtree-remote-name {subtree-url}.git
git subtree add --prefix subtree-folder-name subtree-remote-name master --squash

```

### Updating the subtree folder inside the main repo

```
git fetch subtree-remote-name master
git subtree pull --prefix subtree-folder-name subtree-remote-name master --squash

```

 <hr>
 <hr>


#Git aliases


Note : Using too much of aliases may lead you to forget commands and options.

```
git config --global alias.onelinegraph 'log --oneline --graph --decorate'
```


```
git config --global alias.exp-un-rch 'reflog expire --expire-unreachable=now --all'
```

```
git config --global alias.gc-unrch 'gc --prune=now'
```

common aliases

```
[alias]
	co = checkout
	st = status
	cmt = commit
	df = diff
	dft = difftool
	dfc = diff --cached
	branches = branch -a
	tags = tag
	stashes = stash list
	remotes = remote -v
	commits = log
	daily= !git log --since yesterday --pretty=oneline --author `git config user.email`
	lg = log -n10 --pretty=format:"%C(magenta)%h%Creset %C(blue)%ad%Creset %<(100)%s %C(cyan)[%an]%Creset" --date=short
	lgg --pretty=format:"%C(yellow)%h %Cblue%>(12)%ad %Cgreen%<(7)%aN%Cred%d %Creset%s" --date=short
```

# Set your system to always prune during fetch

```
git config --global fetch.prune true
```

# Performing soft reset

Only Unstages files, No lost of changes

```
git reset head
```

Partial reset

```
git reset xyz.js
```

Move back while keeping changes

```
git reset {commit-id}
```



# Performing Hard reset



#Garbage Collection

* Understanding the git objects
* Cleaning up the repo with git gc


Run to List all objects

```
git cat-file --batch-check --batch-all-objects
```

`git cat-file -t {SHA}` to get the type of the object in the .git/objects folder

**There are three types of git objects:**


1. **Blob** (contains the content of file) `git cat-file -p {sha-of-blob}`


2. **Tree** (contains reference to the blob and file names) `git cat-file -p {sha-of-tree}`


3. **Commit** (contains commit metadata) `git cat-file -p {sha-of-commit}`


Disadvantages of using blob approach ?


Garbage collection techiques

```
git gc --aggressive
```
What are pack and idx files in .git/objects folder?

```
git verify-pack -v .git/objects/pack/
```


#Common Git integrations

1. Continuous Integration
  *  Travis CI
  *  Circle CI

2. Code Coverage
  *  Codecov.io
  *  Coveralls

3. Quality Check
  *  Sonar
  *  Codacy


#Adding image in the readme.md file

```

git checkout --orphan assets

git add screenshot.png demo.gif logo.png

git commit -m "Added Assets"

git push origin assets

![Demo Animation](../assets/demo.gif?raw=true)

```


# Git flow


Gitflow Workflow is a Git workflow design that was first published and made popular by Vincent Driessen at nvie.

![git-flow](../assets/git-flow.png?raw=true)

A git extension that suppports :
[https://github.com/nvie/gitflow](https://github.com/nvie/gitflow)

```
brew install git-flow
```

Git flow can also be practised easily via source tree.


# git revert

rewinds the changes. adds a new revert commit on top.It does not changes the history.

check if it works for only latest commits / head ?
explore -- continue options

```
git revert {commit-id}
```

# git bisect

# Git hooks

* Git hook
* Improving commit messages with "prepare-commit-msg"
* Formatting your code before push with "pre-push"


 <hr>
 <hr>

#Git data Recovery

**Restoring data from:**

1. **reflog**
2. **fsck**
3. **git objects**


**reflog**


**fsck**


**git objects**





