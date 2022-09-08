# Linux Reference

## Wildcards

|Wildcard|Meaning|
|-|-|
|*|Matches any characters|
|?|Matches any single character|
|[_characters_]|Matches any character that is a member of the set _characters_|
|[!_characters_]|Matches any character that is not a member of the set _characters_|
|[:_class_:]|Matches any character that is a member of the specified _class_|

### Commonly used Character Classes
|Character Class|Meaning|
|-|-|
|[:alnum:|Matches any alphanumeric character|
|[:alpha:]|Matches any alphabetic character|
|[:digit:]|Matches any numeral|
|[:lower:]|Matches any lowercase letter|
|[:upper:]|Matches any uppercase letter|

## THE CONSOLE BEHIND THE CURTAIN

Even if we have no terminal emulator running, several terminal sessions continue to run behind the graphical desktop. We can access these sessions, called virtual consoles, by pressing CTRL-ALT-F1 through CTRL-ALT-F6 on most Linux distributions. When a session is accessed, it presents a login prompt into which we can enter our username and password. To switch from one virtual console to another, press ALT-F1 through ALT-F6. On most systems, we can return to the graphical desktop by pressing ALT-F7.

## `man`

### Man Page Organization

|Section|Contents|
|-|-|
|1|user commands|
|2|Programming interfaces for kernel system calls|
|3|Programming interfaces to the C library|
|4|Special files such as device nodes and drivers|
|5|File formats|
|6|Games and amusements such as screen savers|
|7|Miscellaneous|
|8|System administration commands|

> **man** _section_ _search\_term_

```sh-session
[me@linuxbox ~]$ man 5 passwd
```

## `printenv`

Prints all or part of an environment

```sh-session
[me@linuxbox ~]$ printenv
```

## `date`

Displays the current time and date.

```sh-session
[me@linuxbox ~]$ date
Wed Aug 31 04:50:31 UTC 2022
```

## `cal`

By default, displays a calendar of the current month

```sh-session
[me@linuxbox ~]$ cal
    August 2022
Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31
```

## `df`

Display the current amount of free space on our disk drives

```sh-session
[me@linuxbox ~]$ df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/sda2             15115452   5012392   9949716  34% /
/dev/sda5             59631908  26545424  30008432  47% /home
/dev/sda1               147764     17370    122765  13% /boot
tmpfs                   256856         0    256856   0% /dev/shm
```

## `free`

Display the amount of free memory

```sh-session
[me@linuxbox ~]$ free
         total       used       free     shared    buffers     cached
Mem:    513712     503976       9736          0       5312     122916
-/+ buffers/cache: 375748     137964
Swap:  1052248     104712     947536
```

> Note that unlike Windows, which has a separate file system tree for each storage device, Unix-like systems such as Linux always have a single file system tree, regardless of how many drives or storage devices are attached to the computer. Storage devices are attached (or more correctly, mounted) at various points on the tree according to the whims of the system administrator, the person (or people) responsible for the maintenance of the system.

## `file`

Display the file's type

```sh-session
[me@linuxbox ~]$ file picture.jpg
picture.jpg: JPEG image data, JFIF standard 1.01
```

## `type`

Returns which type of command is being interpreted by the shell ie. a binary or a shell builtin

```sh-session
[me@linuxbox ~]$ type echo
echo is a shell builtin
```

## `less`

View text files

```sh-session
[me@linuxbox ~]$ less filename
```

## `which`

The which utility takes a list of command names and searches the path for each executable file that would be run had these commands actually been invoked.

```sh-session
[me@linuxbox ~]$ which echo
echo: shell built-in command
```

## `where`

```sh-session
[me@linuxbox ~]$ where echo
echo: shell built-in command
/bin/echo
```

## `whereis`

The whereis utility checks the standard binary, and manual page directories for the specified programs, printing out the paths of any it finds.

```sh-session
[me@linuxbox ~]$ whereis echo
echo: /bin/echo /usr/share/man/man1/echo.1
```

## `reboot`

Command initiates a reboot

```sh-session
[me@linuxbox ~]$ reboot
```

## `reset`

Command is useful after a program dies leaving a terminal in an abnormal state

```sh-session
[me@linuxbox ~]$ reset
```

## `sort`

sort or merge records (lines) of text and binary files

```sh-session
[me@linuxbox ~]$ ls | sort
Applications/
Desktop/
Documents/
Downloads/
Library/
Movies/
Music/
Pictures/
Projects/
Public/
go/
```

## `uniq`

Report or filter out repeated lines in a file

```sh-session
[me@linuxbox ~]$ echo "first\nsecond\nsecond\nthird" | sort -r | uniq
third
second
first
```

## `wc`

- `-c`: The number of bytes in each input file is written to the standard output. This will cancel out any prior usage of the `-m` option.
- `-l`: The number of lines in each input file is written to the standard output.
- `-m`: The number of characters in each input file is written to the standard output. If the current locale does not support multibyte characters, this is equivalent to the -c option.  This will cancel out any prior usage of the `-c` option.
- `-w`: The number of words in each input file is written to the standard output.

```sh-session
[me@linuxbox ~]$
```

## `grep`

- `-i`, causes `grep` to ignore cate when performing the search
- `-v`, tells `grep` to print only those lines that do not match the pattern
- `-n`, tells `grep` to print the line numbers where the match occurs

```sh-session
[me@linuxbox ~]$ cat linux.md | grep -n grep
175:## `grep`
177:- `-i`, causes `grep` to ignore cate when performing the search
178:- `-v`, tells `grep` to print only those lines that do not match the pattern
179:- `-n`, tells `grep` to print the line numbers where the match occurs
182:[me@linuxbox ~]$ grep
```

## `tee`

The `tee` program reads standard input and copies it to both standard output (allowing the data to continue down the pipeline) and to one or more files.

```sh-session
[me@linuxbox ~]$ ls | tee ls-output
```

## `id`

Prints real and effective user and group IDs

```sh-session
[me@linuxbox ~]$ id
uid=500(me) gid=500(me) groups=500(me)
```
