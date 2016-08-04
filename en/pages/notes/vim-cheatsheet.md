# Vim cheatsheet

## Insert mode commands
- delete back to beginning of the prev word (or begin of curr world if cursor is
already inside a world): `CTRL-w`
- indent current line: `CTRL-t` (mnemonic: 'tab')
   - dedent current line: `CTRL-d` (mnemonic: `dedent`)

## Normal mode commands
- `==`: intelligently indent
- `gq{motion}`: reformat (rewrap); e.g. `gqq` reformats current line
- `gU{motion}`: uppercase (mnemonic: go Upper in {motion})
    - `gUU` to capitalize curent line
- `gu{motion}`: lowercase
- `g~{motion}`: toggle case
-  `C-a`: increment the number at the end of the word under the cursor
-  `C-x`: decrement
-  `gv`: reselect last selection

## Window and splits

```
C-hjkl          for navigating splits

C-w r           rotate windows (downwards/rightwards)
C-w {hjkl}      move window to occupy entire edge of viewport

For a horizontally split window
Ctrl-w +        resize height of current split by single row
Ctrl-w -
10 Ctrl-w +

For a vsplit window
Ctrl-w >
Ctrl-w <

Ctrl-w =        resize all windows to equal dimensions based on their splits

Ctrl-w _        increase window to max height
Ctrl-w |        increase window to max width
```

## Movement
- `hjkl`
- `s` to invoke easymotion
- `leader-j` to invoke easymotion line down
- `leader-k` to invoke measymotion line up
- `w`,`e`,`b` and capital equivs ('big' words)
- `ge`, `gE`: back to end of last word
- `[count]f{char}` forward to next occurance of char
- `F{char}` backward to next occur of char
- `t` and `T`: same as `f/F` but **till/until**
- `;`: repeat last `f,F,t,T` operation
- `C-f` forward a full screen (mnemonic: forward)
- `C-b` backward a full screen
- `C-d` down a half screen (mnemonic: down)
- `C-u` up a half screen
- `H` mapped to `0`
- `L` mapped to `$`
- `gg`, `G`
- `200G` jump to line 200
- `:200` same as `200G`
- `{` jump to last blank line (move up by one paragraph)
- `}`
- `%` jump to matching
- `zg` mark a word as good, adding it to the spelling dictionary

## Scrolling
- `zz` centers current cursor line within viewport
- `zt` scroll current cursor line to top of viewport (mnemonic: t for top)
- `zb` scroll current cursor line to bottom of viewport (mnemonic: b for bott)
- `C-y` normall is scroll down a line but remapped to yank to sys clipboard atm
- `C-e` scroll up a line
- `C-d` down a half screen (mnemonic: down)
- `C-u` up a half screen

## Search
- `*` find word under cursor
- `#` find word under cursor reverse direction
- `g*` like `*` but also look for matches which are substrings of other words
- `/{pattern}`
- `?{pattern}`


## Folding
- `zR` unfold everything
- `zM` fold everything
- `za` toggle fold, `tab` maps to this

## Visual mode commands
- `vip/vap` select current para
- `o` jump to opposite end of selection

## Visual block mode commands
- `<C-v> -> select the block -> I{string}<ESC>` will insert `{string}` at the
start of block on every line of block. (see :h v_b_I)
- `<C-v> -> select the block -> A{string}<ESC>` will do the same but append

## Command mode commands
- `:noh[lsearch]` or `leader-Space` (custom map) removes visiable search
highlighting
- `:ene[w]` opens a new scratch buffer
- `:read` read into current buffer; e.g.
    - `:read !{shell command}` inserts output of command into current buffer
- `:{range}y` yank lines in {range}; e.g. `:88,100y`
- `:{range}d`
- `q:` for vim command history
- `q/` for search history


