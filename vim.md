# Vim Cheetsheet

## Global

`:h[elp] keyword` - open help for keyword

`:sav[eas] file` - save file as

`:clo[se]` or `ctrl + wq` - close current pane

`ter[minal]` - open a terminal window

`K` - open man page for word under the cursor


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

