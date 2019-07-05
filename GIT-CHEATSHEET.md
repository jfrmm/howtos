[< Back](./README.md)

# Git cheatsheet

Git is a powerful versioning tool. This cheatsheet has some useful commands.

Git is a distributed versioning tool. All the repositories are copies of each other. But there's typically one that is chosen to be the one to which all the other repositories push and pull code, the **remote**. The local changes are **commited** to the **local** repository, and then **pushed** to the **origin**. Changes from other repositories are **pulled** from the **origin**.

- [Create](#create)
  - [New](#new)
  - [Clone](#clone)
- [Branches](#branches)
  - [List](#list)
  - [Checkout](#checkout)
  - [New branch](#new-branch)
  - [Delete branch](#delete-branch)
- [Local changes](#local-changes)
  - [Status](#status)
  - [Add all changes](#add-all-changes)
  - [Commit](#commit)
  - [Log](#log)
  - [Stash](#stash)
- [Push & pull](#push-&-pull)
  - [List remotes](#list-remotes)
  - [Fetch](#fetch)
  - [Pull](#pull)
  - [Push](#push)
- [Merging](#merging)
  - [Merge](#merge)
  - [Merge conflicts](#merge-conflicts)
- [Undo](#undo)
  - [Reset](#reset)
  - [Push to remote](#push-to-remote)

# Create

Typically you will either start a repository of your own for new/existing code, or get code from an existing repository.

## New

```
git init
```

## Clone

```
git clone <url-of-the-remote-repository>
```

> If you want to change the folder name, run

```
git clone <url-of-the-remote-repository> <folder-name>
```

# Branches

Branches store different developments of the app (features, bugfixes, releases, etc).

## List

```
git branch -av
```

## Checkout

Get the code of this branch.

```
git checkout <branch-name>
```

## New branch

```
git branch <branch-name>
```

## Delete branch

```
git branch -d <branch-name>
```

# Local changes

First you need to commit your changes to your local repository.

## Status

```
git status
```

## Add all changes

```
git add .
```

## Commit

Commit your local changes to your local repository.

```
git commit -am '<commit-message>'
```

## Log

```
git log
```

## Stash

Save your local changes, without commiting.

```
git stash save '<message>'
```

List the stashes.

```
git stash list
```

Retrieve and delete the stash.

```
git stash pop <stash@{#}>
```

> You can preserve the stash by using `apply` instead of `pop`

# Push & pull

## List remotes

```
git remote -v
```

## Fetch

Know the changes made in the remote.

```
git fetch <remote>
```

## Pull

```
git pull <remote> <branch>
```

## Push

```
git push <remote> <branch>
```

# Merging

## Merge

Merge a branch to the current branch.

```
git merge <branch>
```

## Merge conflicts

Resolve merge conflicts.

```
git mergetool
```

# Undo

## Reset

Discard all local changes

```
git reset --hard HEAD
```

Discard all local changes and go to a previous commit

```
git reset --hard <commit-id>
```

## Push to remote

Push your local reset to the remote

```
git push --force-with-lease
```

> `--force-with-lease` will push only if no one has pushed to the remote after your reset. Use `--force` only when you are sure of what you're doing!

Copyright 2019 jfrmm.
