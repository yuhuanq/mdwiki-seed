# Misc Vim Tips

## Closing Buffers

`:bd` Unload buffer [N] and del from list. if buffer was changed, this fails unless `!` specified.

## Resizing Vim

#### Resizing splits

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

#### Window resizing
`:resize` or `:res` command to change height
`:vertical resize` to change width

```vim
:resize 60  " change the height to 60 rows
:res +5     " change height by increments
:res -5

" :vertical resize to change width of current window
:vertical resize 80
:vertical resize +5
:vertical resize -5
```


