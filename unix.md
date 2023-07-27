# Unix

## Command-Line Keystrokes

| Keystroke | Action                                            |
| --------- | ------------------------------------------------- |
| CTRL-B    | Move the cursor left                              |
| CTRL-F    | Move the cursor right                             |
| CTRL-P    | View the previous command (or move the cursor up) |
| CTRL-N    | View the next command (or move the cursor down)   |
| CTRL-A    | Move the cursor to the beginning of the line      |
| CTRL-E    | Move the cursor to the end of the line            |
| CTRL-W    | Erase the preceding word                          |
| CTRL-U    | Erase from cursor to beginning of line            |
| CTRL-K    | Erase from cursor to end of line                  |
| CTRL-Y    | Paste erased text (for example, from CTRL-U)      |

## Standard Error

You can redirect stderr

```sh
$ ls /fffff > file 2> error_file
```

The number 2 specifies the _stream ID_ that the shell modifies. Stream ID 1 is
standard output (the default), and 2 is standard error.

> Redirect to the same file

```sh
$ ls /fffff > file 2>&1
```

> Newer shortcut to 2>&1 in bash

``` sh
$ ls -l /bin/usr &> ls-output.txt
$ ls -l /bin/usr &>> ls-output.txt
```

## Standard Input Redirection

To channel a file to a program's standard input, use the `<` operator:

```sh
$ head < /proc/cpuinfo
```

## `head`

### Get _n_ lines from the beginning of a file

```sh
head -n [int]
```

## `tail`

### Get _n_ lines from the bottom of a file

```sh
tail -n [int]
```

### Stream contents of a file

> With `--follow` (`-f`), tail defaults to following the file descriptor, which
> means that even if a tail'ed file is renamed, tail will continue to track its
> end. This default behavior is not desirable when you really want to track the
> actual name of the file, not the file descriptor (e.g., log rotation). Use
> `--follow=name` in that case. That causes tail to track the named file in a
> way that accommodates renaming, removal and creation.

```sh
tail -f [file]
```

> `-F` same as `--follow=name --retry`. Ideally used when you want to
> track/retry the actual name of the file, not the file descriptor (e.g., log
> rotation)

```sh
tail -F [file]
```

## `grep`

### Get line which contains search characters and `n` lines associated with search line

```sh
grep -C [int] [search]
```

## `cat`

### Get number of lines of text in all files in a given directory

```sh
cat $(find . -type f -print) | wc -l
```
