# Quick notes on vimperrator shortcuts

**Navigation**
```
C-n   next tab
C-p   prev tab
H     go back
L     go forw
C-i   go forw
C-o   go back
d , :bd   close active tab
t maps to :tabopen
o maps to :open
O maps to :open with current url as base

w :winopen :wopen    - just like :tabopen but opens in a new window
W                    -
```

**Links, Surfing**
```
f    quick-hint mode to highlight all links
F    quick-hint mode open in new tab
```

**Exiting**
```
:xall - quit and save current browsing session for next time
:qall - quit without saving the session
ZZ    - nnoremap :xall<CR>
ZQ    - nnoremap :qall<CR>
```

**Bookmarks, history**
```
:bmarks  - list all bookmarks
:bmark   - add a new bookmark
:history
```

**Temporarily disable vimperator**
```
S-esc
<Insert>
```

```
:ver    show version info
:sbar   open sidebar
:addons open all the add ons and themes in the current tab
u or :undo[all]       undo closed tab
:st[op] or C-c        stop current loading
```

More at `:helpall`
