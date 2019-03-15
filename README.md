# Git Note

## Lecture -01 (Initialize)

### Start 

```bash
$ git init
$ vim README.md
$ git add README.md
$ git commit
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
$ git commit
```

### Change  Configuration

```bash
$ git config --global core.editor vim
$ git config --global merge.tool vimdiff
$ git config --global color.ui true 
$ git config --global alias.st status
$ git config --global alias.ck checkout
$ git config --global alias.rst reset HEAD
```

#### We store the configuration in 

```bash
$ vim ~/.gitconfig
```



##  Lecture -02 (Easy step to commit)

```bash
$ git st
# change some files
$ git ck <THE_CHANGE_FILES>
# change is discarded

# add Not checkout files
$ git add UNADD_FILES
# change files already exist
$ git add -p load.cpp

# git commit (50/72 rule)
$ git commit
```

### Commit

#### DO NOT add and commit these files

1. Large files

2. Generated files

3. Files with secret data 

```bash
$ vim .gitignore
# We can choose some files we don't want to see
$ git add .gitignore

$ git reset HEAD .gitignore
# uncheck .gitignore (turn back to the status before we add)
```

#### Reset

1. reset modification made by add/commit
2. All change become 'unstaged'
3. Use reset --hard carefully

![alt text](https://raw.githubusercontent.com/fjordyo0707/GitNote/master/doc/file_status.png)



## Lecture -03 (Log information)

### Hash Value

#### Every commit has a unique hash value

```bash
# See the log start at SOME_HASH_VALUE and the number before will display 1
# --Oneline mean display only one line
$ git log SOME_HASH_VALUE -n1 --oneline
```

git manage references to commit : HEAD, Branch, Tag, Remote

We can indicate commit by ^, ~

```bash
$ git log -n1 <ONE_HASH_VALUE_BEFORE_HEAD> --online
$ git log -n1 HEAD^ --online
$ git log -n1 HEAD~1 --oneline
# The above command is same

$ git log <SOME_FILE> --oneline
# display the commit relate to SOME_FILE

$ git log --stat <SOME_FILE>
$ git log --patch <SOME_FILE>
# --stat display the CHANGE_FILE and change line
# --patch display the code which is changed

$ git diff
# Display what is changedon current commit (not add to stage)
$ git diff --staged
# Display what is changedon current commit (already stage)
$ git diff master
# Display what is changed compared to master
```



## Lecture -04 (Add file)

```bash
# Always use it
$ git add -p
# Will display Stage this hunk[y,n,q,a,d,/,K,j,J,g,e,?]?
```

s : slice the section thinner
e : edit , go inside the editor 
K : go previous
J : go next
q : quit
a : add all the hunk after it

```bash
$ git commit 
# change subtle
$ git commit --amend
# replace the current HEAD with new HEAD (Not delete, just move to parent and create new commit)
```

## Lecture -05 (Branch and Merge)

#### checkout: move HEAD

```bash
$ git checkout <commit>
# Move to HEAD
$ git checkout <path or file>
# discard change of this path or file
```

#### Branch

```bash
$ git branch
# list the branch now
$ git branch <some_branch>
# create <some_branch>
$ git checkout <some_branch>
# switch to <some_branch>

$ git checkout -b <another_branch>
# (same as the above commands)
# create <another_branch> and switch to <another_branch>

$ git log --graph
# show the current commmit in easy graph

$ git branch -d <some_branch>
# deleted branch <some_branch>
```

#### Merge

```bash
$ git ck master
$ git merge <some_branch> --no-ff
# if just use git merge <some_branch>, git may just use <some_branch> as reference
# may appear conflict
```

**Note:**

1. Prevent very long development

2. Split source code clearly

## Lecture -06 (Rebase)

#### Rebase: Move your branch to another place

![diagram](https://raw.githubusercontent.com/fjordyo0707/GitNote/master/doc/rebase_diagram.png)

![image](https://raw.githubusercontent.com/fjordyo0707/GitNote/master/doc/rebase1.png)

```Bash
$ git rebase <new_base>
# Rebase current branch to <new_branch>

$ git rebase --onto <new_branch> <ref_commit> <>branch
# Rebase from common ancestor of <ref_commit> and <branch> 
```

#### Conflict

If conflict happen, you have to fix it and use 

```bash
$ git st
# check which files need to be fixed,=
$ git add <both modified file>
$ git rebase --continue

$ git rebase --abort
# if out of control
```

#### Rebase -i

```bash
$ git rebase -i <some_branch>^^^^ <some_branch>
# As the previous four commit as base and put on <some_branch>
```

More explanation and reference: https://blog.yorkxin.org/2011/07/29/git-rebase.html

## Lecture -07 (Remote and Github)

#### Clone and Remote

```bash
$ git clone <repo_url>

$ git branch -a
# show all the branch form the remote

$ git remote show origin
# show the information of the orgin

$ git ck origin/<some_branch_from_remote>
# will error since it is the detached HEAD state
# use below
$ git ck -b <some_branch_from_remote>
```

#### Update Status

#### Fetch or Pull

```Bash
$ git fetch <remote_name>
$ git ck master
# Use fast-forwarded
$ git rebase <remote_name>/master
```

#### Push

```bash
$ git push <remote> <local_branch>:<remote_branch>
# Push local branch to remote branch
# --set-upstream
$ git push <remote> :<branch>
# Delete remote branch
```

#### Basic Workflow

1. First fork to your repository
2. Clone

```bash
$ git clone <url>
```

3. Add <upstream>

```Bash
$ git remote add upstream <url>
$ git fetch upstream
$ git ck develop
$ git rebase upstream/develop
# Doing backup
$ git reset HEAD^
$ git ck -b <some_fixing_branch>
```

4. Start to fix or development

5. Local add and commit

```bash
$ git add <file>
$ git commit
```

6. Push 

```bash
$ git push origin <some_fixing_branch>
```

7. Go to project and **Pull Request**

8. If you success, update the project

```bash
$ git ck develop
$ git rebase upstream/develop
```

9. Update your fork github

```bash
$ git push origin develop
```

10. Delete the branch in your repo

```bash
$ git push origin :<some_fixing_branch>
```

## Lecture -08 (SSH on PC and Github)

#### Read following document
Generate a new ssh key and add it to ssh agent: https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
Add ssh key to your own github account: https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account
