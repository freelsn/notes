---
layout: post
title: Git Basics
tags: [git, github]
---

# Basic Commands

```bash
# Configuration
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
# `--global` indicates the configurations apply to the entire local machine. However, it is possible to set different configurations in each repository.
git config --global --list  # checking

# Create Repository
git init

# Add Files
git add readme.txt
# To stage tracked files and deleting the previously tracked files
git add -u .

# Commit
git commit -m "initial commit"

# Check the Repository
git status
git diff readme.txt  # watch the changes
git diff HEAD -- readme.txt  # check the difference between working directory and repository
                             # HEAD indicates the latest version

# Check the commit history
git log
git log --pretty=oneline  # nice format

# Roll back to previous version
git reset --hard HEAD^
git reset --hard "commit id"  # go to the version with "commit id"

# Check commands history
git reflog

# Generate SSH key
ssh-keygen -t rsa -C "youremail@example.com"

# Link local and remote repositories
git remote add origin git@github.com:freelsn/learngit.git
# origin: the name of the remote repo

# Push to remote repo
git push -u origin master
# push to master branch
```

[Understanding *working directory* and *repository*](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013745374151782eb658c5a5ca454eaa451661275886c6000).

[Add local machine SSH key to Github account SSH key list](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000)

# Branch

[Theory behind branch](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840038939c291467cc7c747b1810aab2fb8863508000)

```bash
# Create "dev" branch
git checkout -b dev  # -b create and switch

# Check branches
git branch

# Merge branch "dev" to master
git merge dev

# Delete branch "dev"
git branch -d dev
```

# Link Local `repo` to New Remote `repo`

```bash
# check repo name
git remote
# show remote repo infomation
git remote show origin
# remove current remote repo
git remote rm origin
# add new remote repo
git remote add origin ...
```
