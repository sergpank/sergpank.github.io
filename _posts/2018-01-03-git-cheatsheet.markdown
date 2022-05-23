---
layout: post
title: GIT cheatsheet
date: 2018-01-03
---

## Here is a list of *very basic* commands to start the work with git:

| Command | Description |
|---------|-------------|
| `git clone <link-to-repository>` | Clone specific git repository to your machine |
| `git init`                       | Initialize a new repository in the current directory |
| `git remote add origin  <link-to-repository>` | Connect local repository to remote repository |
| | |
| `git status`                     | Check the status of your repository |
| `git add <path-to-file>`         | Add specific file to local GIT repository |
| `git checkout -- <file>`         | Discard changes in specific file (works for file that is not added to GIT) |
| `git commit -m "commit-message"` | Commit changes into local GIT repository |
| `git reset HEAD <path-to-file>`  | Unstage added file from local GIT repository |
| `git push`                       | Push changes to remote repository |
| `git pull`                       | Pull changes from remote repository |
| | |
| `git log since..until`           | Show unpushed commits. Ex: git log origin/master..master |
| | |
| `git config --global user.email "you@example.com"` | Set your git account |
| `git config --global user.name "Your Name"`        | Set your git username |

## Here is a list of git commands I use the most

| Command                                            | Description
|----------------------------------------------------|------------
| `git branch`                                       | List all local branches
| `git branch --all`                                 | List all local and remote branches
| `git branch -v`                                   | List verbose all branches (with SHA and latest commit)
| `git branch -vv`                                   | List verbose all branches and their upstream branch
| |
| `git pull --all`                                   | Pull all changes from the repository
| |
| `git branch <branch_name>`                         | Crate a new branch
| `git checkout <branch_name>`                       | Checkout created branch
| `git checkout -b <branch_name>`                    | Create and switch to new branch
| |
| `git push --set-upstream origin <new-branch-name>` | Push created branch to repository (if no changes were made in branch after creation)
| |
| `git branch -d <branch_name>`                      | Delete branch
| `git branch -D <branch_name>`                      | Force delete branch (even if it has unmerged changes)
| `git push origin --delete <branch_name>`           | Delete branch in repository
| `git fetch --prune origin`                         | Prune (remove) all unreacheable object from DB (in case if branch is removed on remote origin by somebody else)
| |
| `git merge <branch_name>`                          | Merge <branch_name> into current branch
| |
| `git branch -m new_name`                           | Rename current brach
| `git branch -m old_name new_name`                  | Rename a different branch
| `git push origin :old_name new_name`               | Rename branch in repository
| `git push origin -u origin/new_name`               | Update upstream for renamed local branch
| |
| `git cherry-pick <commit-hash>`                    | Merge specific commit into current branch
| `git cherry-pick -m 1 <commit-hash>`               | Merge specific merge-commit into current branch
| |
| `git tag`                                          | List all tags
| `git tag --list release_1.*`                       | List tags that match pattern
| `git tag -a <tag_name> -m <tag_description>`       | Create tag
| `git tag -a <tag_name> <commit_hash>`              | Create tag for an old commit
| `git push origin <tag_name>`                       | Push tag to repo
| `git push origin --tags`                           | Push all tag to repo
| `git checkout tags/<tag_name>`                     | Checkout specific tag
| `git tag -d <tag_name>`                            | Remove tag localy ...
| `git push origin :refs/tags/<tag_name>`            | ... and push the removal to repo
| |
| `git rm --cached <file_name>`                      | Stop tracking file and remove it from git index

## Disable git log pager

For current repo only:
`git config pager.log false`

For your git installation (i. e. all repos on your machine):
`git config --global pager.log false`

## Working with remote (when you have a damn fork)

| Command                              | Description 
|--------------------------------------|------------
| `git remote -v`                      | List currently configured remote repositories 
| `git remote add upstream <repo-url>` | Specify a new remote upstream repository that will be synced with the fork
| `git pull upstream <branch>`         | Pull latest code from `upstream` branch 
| `git checkout --track coworker/foo`  | Checkout a branch from another fork 
