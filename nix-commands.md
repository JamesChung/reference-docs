# Unix/Linux Commands

## `head`

### Get *n* lines from the beginning of a file

```sh
head -n [int]
```

## `tail`

### Get *n* lines from the bottom of a file

```sh
tail -n [int]
```

### Stream contents of a file

> With `--follow` (`-f`), tail defaults to following the file descriptor, which means that even if a tail'ed file is renamed, tail will continue to track its end. This default behavior is not desirable when you really want to track the actual name of the file, not the file descriptor (e.g., log rotation). Use `--follow=name` in that case. That causes tail to track the named file in a way that accommodates renaming, removal and creation.

```sh
tail -f [file]
```

> `-F` same as `--follow=name --retry`. Ideally used when you want to track/retry the actual name of the file, not the file descriptor (e.g., log rotation)

```sh
tail -F [file]
```

## `grep`

### Get line which contains search characters and `n` lines associated with search line

```sh
grep -C [int] [search]
```
