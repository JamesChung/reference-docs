# Vim Cheetsheet

## Global

`:h[elp] keyword` - open help for keyword

`:sav[eas] file` - save file as

`:clo[se]` or `ctrl + wq` - close current pane

`ter[minal]` - open a terminal window

`K` - open man page for word under the cursor

## Cursor Movement

### Core Movements

`h` - move cursor left

`j` - move cursor down

`k` - move cursor up

`l` - move cursor right

`gj` - move cursor down (multi-line text)

`gk` - move cursor up (multi-line text)

### Jump Movements

`:ju[mps]` - list of jumps

`ctrl + i` - go to newer position in jump list

`ctrl + o` - go to older position in jump list

#### Lines

`0` - jump to the start of the line

`^` - jump to the first non-blank character of the line

`$` - jump to the end of the line

`g_` - jump to the last non-blank character of the line

`gg` - jump to the first line of the document

`G` - jump to the last line of the document

`[number]gg` or `[number]G` - go to line [number]

#### Words

`w` - jump forwards to the start of a word

`W` - jump forwards to the start of a word (words can contain punctuation)

`e` - jump forwards to the end of a word

`E` - jump forwards to the end of a word (words can contain punctuation)

`b` - jump backwards to the start of a word

`B` - jump backwards to the start of a word (words can contain punctuation)

`ge` - jump backwards to the end of a word

`gE` - jump backwards to the end of a word (words can contain punctuation)

#### Characters

`f[character]` - jump to next occurrence of [character]

`F[character]` - jump to the previous occurrence of [character]

`t[character]` - jump to before next occurrence of [character]

`T[character]` - jump to after previous occurrence of [character]

`;` - repeat previous `f`, `t`, `F`, `T` movements forwards

`,` - repeat previous `f`, `t`, `F`, `T` movements backwards

### Macro Movements

`H` - move to top of screen [High]

`M` - move to middle of screen [Mid]

`L` - move to bottom of screen [Low]

`%` - move to matching `(), {}, []` pair

`}` - jump to next (paragraph, function, block)

`{` - jump to previous (paragraph, function, block)

## Screen Movement

`ctrl + e` - move screen down one line (without moving cursor)

`ctrl + y` - move screen down one line (without moving cursor)

`ctrl + f` - move forward one full screen

`ctrl + b` - move back on full screen

`ctrl + d` - move forward half a screen

`ctrl + u` - move backwards half a screen

## Editing

`.` - repeat last command

`u` - undo

`U` - restore (undo) last changed line

`ctrl + r` - redo

`r` - replace a single character

`R` - replace more than one character, until `ESC` is pressed

`s` - delete character and enters insert mode

`S` or `cc` - delete line and enter insert mode

`C` or `c$` - change (replace) from cursor to the end of the line

`cw` or `ce` - change (replace) to the end of the word

`J` - join line below to the current one with one space in-between

`gJ` - join line below to the current one without a space in-between

`gwip` - reflow paragraph

`g~` - switch case up to motion

`gu` - change to lowercase up to motion

`gU` - change to uppercase up to motion

### Insert Mode

Exit Insert Mode:

> There are multiple ways to escape insert mode

- `Esc`
- `ctrl + c`
- `ctrl + [`

#### Core Commands

`i` - insert before the cursor

`I` - insert at the beginning of the line

`a` - insert (append) after the cursor

`A` - insert (append) at the end of the line

`o` - append (open) a new line below the cursor

`O` - append (open) a new line above the cursor

#### Additional Editing

`ctrl + h` - delete the character before the cursor during insert mode

`ctrl + w` - delete word before the cursor during insert mode

`ctrl + t` - indent (move right) line one shiftwidth during insert mode

`ctrl + d` - indent (move left) line one shiftwidth during insert mode

## Working with Files

`e[dit] file` - edit a file in a new buffer

`bn[ext]` - go to the next buffer

`bp[revious]` - go the previous buffer

`bd[elete]` - delete a buffer (close a file)

`:ls` or `:buffers` - list all open buffers

`:sp[lit] file` - open a file in a new buffer and split window

`:vs[plit] file` - open a file ina new bufer and vertically split window

`:vert[ical] ba[ll]` - edit all buffers as vertical windows

## Window Management

`ctrl + ws` - split window

`ctrl + wv` - split window vertically

`ctrl + ww` - switch windows

`ctrl + wq` - quit a window

`ctrl + wx` - exchange current window with next one

`ctrl + w=` - make all windows equal height & width

`ctrl + wh` - move cursor to the left window (vertical split)

`ctrl + wl` - move cursor to the right window (vertical split)

`ctrl + wj` - move cursor to the window below (horizontal split)

`ctrl + wk` - move cursor to the window above (horizontal split)

`ctrl + wH` - make current window full height at far left (leftmost vertical window)

`ctrl + wL` - make current window full height at far right (rightmost vertical window)

`ctrl + wJ` - make current window full width at the very bottom (bottommost horizontal window)

`ctrl + wK` - make current window full width at the very top (topmost horizontal window)

