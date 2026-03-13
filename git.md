# git

## Change default username

`git config --global user.name <name>`

## Change default email

`git config --global user.email <email>`

## Change default git editor

`git config --global core.editor <editor>`

## Change default branch name

`git config --global init.defaultBranch <name>`

## Change default pull behavior to rebase

`git config --global pull.rebase true`

## Configure to push all tags

`git config --global push.followTags true`

---

# Managing Stacked Branches in Git: An Instructional Guide

Working with "stacked branches" (creating a new feature branch off an existing feature branch) can lead to a messy commit history and difficult merge conflicts if the base branch needs to be rebased.

This guide covers two effective methods for managing this workflow cleanly, depending on your Git version and the current state of your branches.

## Method 1: The Modern Approach (`--update-refs`)

*Best for Git version 2.38+ when the base branch has not yet been rebased by someone else.*

Git 2.38 introduced a built-in feature specifically designed to solve the stacked branch problem. When you rebase the bottom branch, Git can automatically update the pointers for any branches stacked on top of it.

### Instructions

1. Ensure you are currently checked out on your first feature branch (e.g., `feature-1`).
2. Run the rebase command with the update flag against your target branch (e.g., `main`):
```bash
git rebase --update-refs main

```



**How it works:** Git rebases `feature-1` onto `main`. As it rewrites the commits for `feature-1`, it identifies any dependent branches (like `feature-2`) and automatically moves their pointers to the newly rewritten commits.

> **Pro-Tip:** Make this the default behavior for all rebases by updating your global Git configuration:
> ```bash
> git config --global rebase.updateRefs true
> 
> ```
> 
> 

---

## Method 2: The Surgical Fix (`--onto`)

*Best for all Git versions when the base branch has already been rebased or squashed.*

If `feature-1` has already been rewritten (e.g., via a squash-and-merge on a remote repository or a rebase by a teammate), `feature-2` will be stuck pointing to the old, orphaned history. You must manually transplant `feature-2` onto the new `feature-1` using `git rebase --onto`.

### Syntax

```bash
git rebase --onto <new-parent> <old-parent> <branch-to-move>

```

### Instructions

Assume `feature-2` was branched off `feature-1`. `feature-1` was just rebased, leaving `feature-2` behind.

1. **Locate the old base commit:** Identify the commit hash where `feature-2` originally connected to `feature-1`. You can find this by inspecting the `git log` on `feature-2` and finding the last commit *before* your `feature-2` specific work began. Let's assume this hash is `abc1234`.
2. **Execute the transplant:** Run the following command to move your work:
```bash
git rebase --onto feature-1 abc1234 feature-2

```



**How it works:** This command instructs Git to take all commits on `feature-2` that come strictly *after* `abc1234`, and replay them on top of the current state of `feature-1`.

---
