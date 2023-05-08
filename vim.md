# Vim Cheetsheet

## General

|General|Working with Files|
|:-|:-|
|`:h[elp] keyword` - open help for keyword|`e[dit] file` - edit a file in a new buffer|
|`:sav[eas] file` - save file as|`bn[ext]` - go to the next buffer|
|`:clo[se]` or `ctrl + wq` - close current pane|`bp[revious]` - go the previous buffer|
|`ter[minal]` - open a terminal window|`bd[elete]` - delete a buffer (close a file)|
|`K` - open man page for word under the cursor|`:ls` or `:buffers` - list all open buffers|
||`:sp[lit] file` - open a file in a new buffer and split window|
||`:vs[plit] file` - open a file in a new bufer and vertically split window|
||`:vert[ical] ba[ll]` - edit all buffers as vertical windows|

## Cursor Movement

|Core Movements|Word Movements|Line Movements|
|:-|:-|:-|
|`h` - move cursor left|`w` - jump forwards to the start of a word|`0` - jump to the start of the line|
|`j` - move cursor down|`W` - jump forwards to the start of a word (words can contain punctuation)|`^` - jump to the first non-blank character of the line|
|`l` - move cursor right|`e` - jump forwards to the end of a word|`$` - jump to the end of the line|
|`gj` - move cursor down (multi-line text)|`E` - jump forwards to the end of a word (words can contain punctuation)|`g_` - jump to the last non-blank character of the line|
|`gk` - move cursor up (multi-line text)|`b` - jump backwards to the start of a word|`gg` - jump to the first line of the document|
||`B` - jump backwards to the start of a word (words can contain punctuation)|`G` - jump to the last line of the document|
||`ge` - jump backwards to the end of a word|`[number]gg` or `[number]G` - go to line [number]|
||`gE` - jump backwards to the end of a word (words can contain punctuation)||

|Character Movements|Macro Movements|Jump Movements|
|:-|:-|:-|
|`f[character]` - jump to next occurrence of [character]|`H` - move to top of screen [High]|`:ju[mps]` - list of jumps|
|`F[character]` - jump to the previous occurrence of [character]|`M` - move to middle of screen [Mid]|`ctrl + i` - go to newer position in jump list|
|`t[character]` - jump to before next occurrence of [character]|`L` - move to bottom of screen [Low]|`ctrl + o` - go to older position in jump list|
|`T[character]` - jump to after previous occurrence of [character]|`%` - move to matching `(), {}, []` pair|
|`;` - repeat previous `f`, `t`, `F`, `T` movements forwards|`}` - jump to next (paragraph, function, block)|
|`,` - repeat previous `f`, `t`, `F`, `T` movements backwards|`{` - jump to previous (paragraph, function, block)|

## Editing

Exit Insert Mode:

> There are multiple ways to escape insert mode

- `Esc`
- `ctrl + c`
- `ctrl + [`

|General Editing|Insert Commands|Visual Mode|
|:-|:-|:-|
|`u` - undo|`i` - insert before the cursor|`v` - start visual mode, mark lines|
|`U` - restore (undo) last changed line|`I` - insert at the beginning of the line|`V` - start visual line mode, mark lines|
|`ctrl + r` - redo|`a` - insert (append) after the cursor|`ctrl + v` - start visual block mode, mark blocks|
|`r` - replace a single character|`A` - insert (append) at the end of the line|`o` - move to other end of marked area|
|`R` - replace more than one character, until `ESC` is pressed|`o` - append (open) a new line below the cursor|`O` - move to other corner of block|
|`s` - delete character and enters insert mode|`O` - append (open) a new line above the cursor|`aw` - mark a word|
|`S` or `cc` - delete line and enter insert mode|`ctrl + h` - delete the character before the cursor during insert mode|`ab` - a block with ()|
|`C` or `c$` - change (replace) from cursor to the end of the line|`ctrl + w` - delete word before the cursor during insert mode|`aB` - a block with {}|
|`cw` or `ce` - change (replace) to the end of the word|`ctrl + t` - indent (move right) line one shiftwidth during insert mode|`at` - a block with <> tags|
|`J` - join line below to the current one with one space in-between|`ctrl + d` - indent (move left) line one shiftwidth during insert mode|`ib` - inner block with ()|
|`gJ` - join line below to the current one without a space in-between||`iB` - inner block with {}|
|`gwip` - reflow paragraph||`it` - inner block with <> tags|
|`g~` - switch case up to motion||`>` - shift text right|
|`gu` - change to lowercase up to motion||`<` - shift text left|
|`gU` - change to uppercase up to motion||`u` - change marked text to lowercase|
|`.` - repeat last command||`U` - change marked text to uppercase|
|||`~` - switch case|
|||`vip` - select inner paragraph only|
|||`vap` - select entire paragraph including whitespace|

## Search & Replace

> Pair Characters can be: "", '', ``, <>, (), {}, [] and more
`vi` + [pair character] - select everything within the next enclosing pair characters

`va` + [pair character]- select everything from pair character to character

`yi` + [pair character]- yank everything within the next enclosing pair characters

`ya` + [pair character]- yank everything from pair character to character

`di` + [pair character] - delete everything within the next enclosing pair characters

`da` + [pair character] - delete everything from pair character to character

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

