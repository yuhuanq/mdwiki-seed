# tmux

### Commands

#### Windows 
| command             | action                |
|---------------------|-----------------------|
| C-b <command\>                 | tmux prefex           |
| c               | create new window     |
| ,               | rename window         |
| [p/n]           | cycle through windows |
| w | list windows          |


#### Panes

splitting

| command | action             |
|---------|--------------------|
| %       | split vertically   |
| "       | split horizontally |

#### Sessions

to create a session ```tmux new -s [name]```. 

Just like starting tmux but its a named session.

| command | action         | more                                                 |
|---------|----------------|------------------------------------------------------|
| d       | detach session | processes in session will still be running on detach |

Even after leaving ssh session, process will still be running.

```tmux list-sessions``` will still show the last session running.

Can now reattach to it 

```tmux attach -t [name] ```


