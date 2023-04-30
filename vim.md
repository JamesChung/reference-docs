# Vim Cheetsheet

## Global

`:h[elp] keyword` - open help for keyword

`:sav[eas] file` - save file as

`:clo[se]` - close current pane

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
