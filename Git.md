# 快速开始

## VCS分类

### Local Version Control Systems

- [RCS](https://www.gnu.org/software/rcs/)

### Centralized Version Control Systems

- CVS(Concurrent Versions System)

- Subversion

- Perforce

### Distributed Version Control Systems

- Git
- Mercurial
- Darcs

**delta-based**

**stream of snapshots**

## Git Has Integrity

**SHA-1 hash**：**40-character** string composed of hexadecimal characters (0–9 and a–f)

## Three States

- modified：the working tree
- staged：the staging area(**index**)
- committed：the Git directory

## Git Setup

```
git config --list
git config --list --show-origin
git config user.name

# 查看某个值读取的哪个配置
git config --show-origin rerere.autoUpdate
```

**系统配置**：针对所有用户、所有仓库

```
# Git的安装目录
[path]/etc/gitconfig

git config --sytem
```

**用户配置**：针对某个用户下所有仓库

```
# Windows - C:\Users\$USER 
~/.gitconfig or ~/.config/git/config

git config --global


# 设置身份信息(每次commit提交时会使用)
git config --global user.name "xxx"
git config --global user.email "xxx"

# 设置系统默认的编辑器(当git需要输入信息的时候使用)
git config --global core.editor emacs
git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

# 设置默认分支(git2.28之后的版本支持)
git config --global init.defaultBranch main

# 设置是否需要rebase
git config --global pull.rebase "false"

# 避免通过https传输时,每次都需要输入账号密码
git config --global credential.helper cache
```

**仓库配置**：针对某个仓库

```
config file in the Git directory (that is, .git/config)

git config --local
```

## Git Help

```
git help <verb>
git <verb> --help
git <verb> -h
man git-<verb>
```

# Git Basics

```
git init
git add xxx
git commit -m "xxx"

git clone <url>

git clone https://github.com/libgit2/libgit2
git clone https://github.com/libgit2/libgit2 mylibgit


git status
# M:修改已有 ??:新文件 A:已经staged的文件
git status -s
git status --short

GitHub changed the default branch name from master to main in mid-2020

# 跳过stage阶段
git commit -a -m "xxx"
```

## 添加新文件

```
echo hello > README
git status

Untracked files:
  (use "git add <file>..." to include in what will be committed)
git add README

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
git restore --staged README

git commit -m "README"
```

## 修改已有文件

```
echo hello > README.md
git status

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)

git restore README.md

git add README.md
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)

git commit -m "README.md"
```

## .gitignore

常见[gitignore](https://github.com/github/gitignore)文件

```
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

## diff内容比较

```
# compares what is in your working directory with what is in your staging area
git diff

# compares your staged changes to your last commit
git diff --staged
```

## 删除文件

```
git rm xxx
git commit -m "xxx"

或者

del xxx
git add/rm xxx
git commit -m "xxx"
```

## 重命名文件

```
git mv EADME.md README
git commit xxx

相当于
mv README.md README
git rm README.md
git add README
git commit -m ”xxx"
```

## 查看git日志

```
git log

# 查看每次commit的path
git log --patch xxx
git log -p xxx
git log -p -2

# 查看每次commmit的简单状态信息
git log --stat

# 设置输出格式
git log --pretty=oneline
git log --pretty=format:"%h - %an, %ar : %s"

# 查看最近的2次提交
git commit -n 2

# 查看进两周的提交
git log --since=2.weeks
```

## Undoing Things

```
git commit --amend
```

## Working with Remote

```
git remote
git remote -v

git remote add <shortname> <url>
git remote add pb https://github.com/paulboone/ticgit

git fetch <remote>
git fetch pb

git push <remote> <branch>
git push origin master

git remote show <remote>
git remote show origin

git remote rename <oldname> <newname>
git remote rename pb paul

git remote remove <shortname>
git remote remove paul
```

## Tags

```
git tag
git tag -l
git tag --list
```

Git支持两种类型的Tag：**lightweight** 和 **annotated**

A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.

Annotated tags, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message.

### Annotated Tags

```
# 需要加-a参数
git tag -a v1.4 -m "my version 1.4"

git show v1.4
```

### Lightweight Tags

To create a lightweight tag, don’t supply any of the `-a`, `-s`, or `-m` options, just provide a tag name

```
git tag v1.4-lw

git show v1.4-lw
```

### Tagging Later

```
git log --pretty=oneline

git tag -a v1.2 9fceb02
```

### Sharing Tags

```
git push origin <tagname>

git push origin v1.5

# 本地所有tag推送到远程仓
git push origin --tags
```

### Deleting Tags

```
# 删除本地Tag
git tag -d <tagname>

git tag -d v1.4-lw

# 删除远端Tag方式一
git push <remote> :refs/tags/<tagname>

git push origin :refs/tags/v1.4-lw

# 删除远端Tag方式二
git push origin --delete <tagname>
```

### Checking out Tags

```
# You are in 'detached HEAD' state.不推荐
git checkout v2.0.0

# 推荐
git checkout -b version2 v2.0.0
```

## Git Aliases

```
git config --global alias.ci commit

git ci
```

# Git Branching

## Creating a New Branch

```
git branch testing

git log --oneline --decorate
```

## Switching Branches

```
git checkout testing

git log --oneline --decorate --graph --all

# 创建并切换分支
git checkout -b <newbranchname>

# From Git version 2.23 onwards you can use git switch instead of git checkout 
git switch testing-branch

git switch -c new-branch

git switch -
```

## Basic Branching and Merging

```
# 把hotfix分支合并到当前分支
git merge hotfix

# 删除hotfix分支
git branch -d hotfix
```

## Branch Management

```
git branch

git branch -v

# 已经合并到当前分支的分支
git branch --merged

# 未合并到当前分支的分支
git branch --no-merged
```

## Changing a branch name

```
git branch --move bad-branch-name corrected-branch-name

git push --set-upstream origin corrected-branch-name

git branch --all

git push origin --delete bad-branch-name

git branch --move master main
```

## Remote Branches

```
<remote>/<branch>

git ls-remote <remote>
git ls-remote origin

git remote show <remote>
git remote show origin

git fetch <remote>
git fetch origin

git clone -o booyah
booyah/master
```

### Pushing

```
git push <remote> <branch>
git push origin serverfix
等价于
git push origin serverfix:serverfix

# 本地的serverfix分支推送到远程的awesomebranch分支
git push origin serverfix:awesomebranch

git fetch origin
git merge origin/serverfix

git checkout -b serverfix origin/serverfix
```

### Tracking Branches

Checking out a local branch from a remote-tracking branch automatically creates what is called a “**tracking branch**” (and the branch it tracks is called an “**upstream branch**”). Tracking branches are local branches that have a direct relationship to a remote branch. If you’re on a tracking branch and type git pull, Git automatically knows which server to fetch from and which branch to merge in.

```
git checkout -b <branch> <remote>/<branch>
等价于
git checkout --track origin/serverfix

git checkout -b sf origin/serverfix
```

If you already have a local branch and want to set it to a remote branch you just pulled down, or want to change the upstream branch you’re tracking, you can use the `-u` or `--set-upstream-to` option to git branch to explicitly set it at any time

```
git branch -u origin/serverfix

git branch -vv

git fetch --all; git branch -vv
```

### Pulling

```
# git fetch + git merge
git pull
```

### Deleting Remote Branches

```
git push origin --delete serverfix
```

## Rebasing

In Git, there are two main ways to integrate changes from one branch into another: the `merge` and the `rebase`. 

```
# 1、切换到experiment分支
git checkout experiment

# 2、在当前分支(experiment)，基于住分支打一个patch，并移动指针
git rebase master

# 3、切换到主分支
git checkout master

# 4、合并experiment分支
git merge experiment
```

