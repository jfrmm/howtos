[< Back](./README.md)

# GitFlow cheatsheet

[GitFlow](#https://nvie.com/posts/a-successful-git-branching-model/) is a branching model for [Git](#https://git-scm.com/). It uses a super-set of Git commands, and it's intended for easing the workflow between team members, and to scale nicely.

One will typically set it with the defaults, start developing a small bugfix on **develop**, develop a new feature from **develop**, and release tags on **master**.

- [Init](#init)
- [Features](#features)
  - [Start](#start)
  - [Finish](#finish)
- [Releases](#releases)
  - [Start](#start)
  - [Finish](#finish)
- [Hotfixes](#hotfixes)
  - [Start](#start)
  - [Finish](#finish)

# Init

To start using GitFlow, you must init it on a folder with an existing repository.

```
git flow init -d
```

> The `-d` argument will accept all the defaults

# Features

Features start from **develop**, and are merged to **develop**.

## Start

```
git flow feature start <feature-name>
```

> If you want to use some special characters like `#`, wrap the feature name with `'`

## Finish

```
git flow feature finish <feature-name>
```

# Releases

Releases start from **develop**, and are merged to **develop** and **master**.

## Start

```
git flow release start <release-tag>
```

> Optionally, can add a commit ID from the **develop**, from where the release will be made

## Finish

```
git flow release finish <release-tag>
```

> After pushing master to the remote, push the tags with `git push origin --tags`

# Hotfixes

Hotfixes start from **master**, and are merged to **develop** and **master**

## Start

```
git flow hotfix start <hotfix-tag>
```

## Finish

```
git flow hotfix finish <hotfix-tag>
```

> A tag will be created in **master** with the hotfix name, don't forget to push tags with `git push origin --tags`

Copyright 2019 jfrmm.
