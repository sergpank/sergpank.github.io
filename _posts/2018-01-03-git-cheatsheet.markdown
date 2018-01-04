---
layout: post
title: GIT cheatsheet
date: 2018-01-03
---

## Here is a list of git commands I use the most ##

| Command                                            | Description                                                                            
|----------------------------------------------------|---------------------------------------------------------------------------------------
| `git branch`                                       | List all local branches
| `git branch --all`                                 | List all local and remote branches
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
| |
| `git merge <branch_name>`                          | Merge <branch_name> into current branch
| |
| `git branch -m new_name`                           | Rename current brach
| `git branch -m old_name new_name`                  | Rename a different branch
| |
| `git cherry-pick <commit-hash>`                    | Merge specific commit into current branch
| `git cherry-pick -m 1 <commit-hash>`               | Merge specific merge-commit into current branch