# tmux

## Windows
| command        | action                |
|----------------|-----------------------|
| C-b <command\> | tmux prefex           |
| c              | create new window     |
| ,              | rename window         |
| [p/n]          | cycle through windows |
| w              | list windows          |
| &              | kill window           |


## Panes

### Splitting

| command | action             |
|---------|--------------------|
| %       | split vertically   |
| "       | split horizontally |

### Resizing Panes

```
PREFIX : resize-pane -D (Resizes the current pane down)
PREFIX : resize-pane -U (Resizes the current pane upward)
PREFIX : resize-pane -L (Resizes the current pane left)
PREFIX : resize-pane -R (Resizes the current pane right)
PREFIX : resize-pane -D 20 (Resizes the current pane down by 20 cells)
PREFIX : resize-pane -U 20 (Resizes the current pane upward by 20 cells)
PREFIX : resize-pane -L 20 (Resizes the current pane left by 20 cells)
PREFIX : resize-pane -R 20 (Resizes the current pane right by 20 cells)
PREFIX : resize-pane -t 2 20 (Resizes the pane with the id of 2 down by 20 cells)
PREFIX : resize-pane -t -L 20 (Resizes the pane with the id of 2 left by 20 cells)
```

## Sessions

to create a session `tmux new -s [name]`.

Just like starting tmux but its a named session.

| command | action         | more                                                 |
|---------|----------------|------------------------------------------------------|
| d       | detach session | processes in session will still be running on detach |

Even after leaving ssh session, process will still be running.

`tmux list-sessions` will still show the last session running.
`tmux ls`

Can now reattach to it

`tmux attach -t [name]`

kill session:
`tmux kill-session -t myname`


**More** at https://gist.github.com/MohamedAlaa/2961058

