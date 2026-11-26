---
title: "Git and GitHub: A Practical Introduction"
date: 2026-11-26
draft: false
description: "Learn repositories, commits, branches, remotes, and the everyday Git workflow."
categories: ["Developer Tools"]
tags: ["Developer Tools"]
readingTime: true
---

## Version control

Version control records how a set of files changes over time. It allows developers to inspect previous work, compare revisions, create parallel lines of development, and collaborate without manually maintaining folders such as `project-final`, `project-final-2`, and `project-final-fixed`.

## Git and GitHub are different

**Git** is a distributed version control system that runs on your computer. **GitHub** is an online platform that hosts Git repositories and adds collaboration features such as pull requests, issue tracking, and code review. Git can be used without GitHub, and GitHub repositories use Git to track history.

Git was created by Linus Torvalds in 2005 for development of the Linux kernel. Its distributed design gives each clone a complete local repository history.

## Configure Git

After installing Git, set the identity that should appear in your commits:

```bash
git config --global user.name "Muhammad Umair"
git config --global user.email "your-email@example.com"
```

Use an email address you intend to associate with your commits. You can inspect the configuration with `git config --list`.

## The everyday workflow

A repository is a project directory tracked by Git. Create one with `git init`, or copy an existing remote repository with `git clone`.

The basic cycle is:

```bash
git status
git add .
git commit -m "Describe the change"
```

`git status` shows changed and staged files. `git add` selects changes for the next snapshot. `git commit` records that snapshot in local history. Review what will be committed before staging everything, especially when a project contains credentials, generated files, or private data.

Useful inspection commands include `git log --oneline`, `git diff`, and `git diff --staged`.

## Remotes and GitHub

A remote is a named reference to another repository. After creating an empty repository on GitHub, connect it and push a branch:

```bash
git remote add origin https://github.com/USERNAME/REPOSITORY.git
git branch -M main
git push -u origin main
```

For later updates, commit locally and run `git push`. To bring remote changes into the current branch, use `git pull` and resolve any merge conflicts carefully.

## Branches and collaboration

A branch is a movable pointer to a line of development. Create a feature branch with `git switch -c feature-name`. Make focused commits, push the branch, and open a pull request so changes can be reviewed before merging into the main branch.

When two branches change overlapping lines, Git may report a merge conflict. Open the affected files, decide which content should remain, remove conflict markers, test the result, and complete the merge.

## Habits that prevent problems

- Commit small, meaningful changes.
- Write messages that explain the purpose of a change.
- Never commit passwords, API keys, or private configuration.
- Use a `.gitignore` file for generated and local-only files.
- Pull or fetch regularly when collaborating.
- Review diffs before committing and before opening a pull request.

Git gives a project a dependable history; GitHub makes that history easier to share and review.
