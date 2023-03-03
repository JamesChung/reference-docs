# Git Worktree Reference

## `worktree add`

> How do you use Git worktree add for an existing branch using the same name as the working directory with the command line?

```sh
git worktree add path/to/folder/<existing-branch-name>
```
> How do you use Git worktree add for an existing branch using a different name than the working directory with the command line?

```sh
git worktree add path/to/folder/ <existing-branch-name>
```

> How do you use Git worktree add to create a new branch using the same name as the working directory with the command line?

```sh
git worktree add -b <new-branch-name> path/to/folder/<new-branch-name>
```

> You can also choose to base a new branch from another branch, tag or commit-ish, by specifying your choice at the end of the command.

```sh
git worktree add -b <new-branch-name> path/to/folder/<new-branch-name> <existing-branch-to-use-as-base>
```

> How to use Git worktree add to create a new branch using a different name than the working directory with the command line?

```sh
git worktree add -b <new-branch-name> path/to/folder/
```

## `worktree remove`

> To remove a Git worktree entry with the CLI, you will need to specify the folder you want to remove. If the branch name and the folder name are different, you don’t need to name the branch.

> If there are uncommitted changes in that working tree’s folder, the remove command will fail. To overcome this, you can use the force option: -f.  As with any use of --force, be careful and make sure you know what you’re doing before overriding Git’s safety precautions.

```sh
git worktree remove path/to/folder/
```
