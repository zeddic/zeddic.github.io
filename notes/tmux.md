---
layout: page
title: tmux Notes
permalink: /notes/tmux
---

## Named sessions

```bash
tmux new -s mysession
```

- `new` is an alias for `new-session`
- `-s` is short for session-name

```bash
tmux a -t mysession
```

- `a` is an alias for `attach` / `attach-session`
- `-t` is short for target-session
- `-d` may be used to detach other clients

## General purpose new/attach default session

```
tmux new -Ad
```

- Creates a new session with a default name
- `-A` attempts to attach to an existing one if it exists
- `d` will disconnect existing clients

## tmux commands

There are different keyboard shortcuts once tmux has started.

Regular commands are accessed via tmux chord (`ctrl+B`) followed by the key-binding.

tmux console commands can be run using the tmux chord followed by `:`. (`ctrl+B:`)

| Command             | Description                        |
| ------------------- | ---------------------------------- |
| %                   | Splits pane horizontally           |
| "                   | Split pane vertically              |
| up/down/left/right  | Moves pane focus in that direction |
| c                   | Creates a new window (tab)         |
| n                   | Go to next tab                     |
| p                   | Go to previous tab                 |
| z                   | Toggles zooming the current tab    |
| `:detach-client -a` | Detaches all other session clients |

### Resize commands

Panes can be resized using the `:resize-pane` command.
It accepts `-U`, `-D`, `-L`, `-R` followed by an account to resize the pane in that direction. For example:

```shell
:resize-pane -L 10
:resize-pane -U 20
```

Sometimes you can also press and hold `ctrl+B+arrow` to resize the pane in that direction.
