---
layout: post
title: TMUX brief cheatsheet
date: 2024-03-19
category: tmux
---  

```
TMUX has a very simple hierarchy : Session -> Window -> Panel

TMUX
|-> Session 1
    |-> Window 1
        |-> Panel 1
        |-> Panel 2
        |-> ...
    |-> Window 2
        |-> ...
    |-> ...
|-> Session 2
    |-> ...

All commands start with prefix [PX] - default prefix is [ ctrl+B ]

tmux a
tmux att
tmux attach
tmux attach-session           --- attach to latest session
tmux a -t <session name>      --- attach to session by name
tmux new
tmux new-seesion
tmux new -s <session name>    --- create new session
tmux ls                       --- show sessions list

PX + ?               --- show all keybindigs

PX + s               --- show all sessions
PX + w               --- preview all sessions
PX + ( )             --- move between sessions
PX + $               --- rename session
PX + d               --- exit session

PX + c               --- create new window
PX + <window number> --- change window
PX + n / PX + p      --- next / previous window
PX + &               --- kill window

PX + %               --- split panel horizontally
PX + "               --- split panel vertically
PX + ← ↑ → ↓         --- navigate between panels
PX + { }             --- switch panels
PX + q               --- show panel number , and optional go to this panel if hit number
PX + !               --- close all other panels
PX + x               --- close current panel

# how to merge windows into panel:
1. go to source window
2. PX + m
3. go to target window
4. PX + :join-pane
5. PX + space    # --- to switch layout if necessary

PX + esc + 1   --- make all panes even and split vertical
PX + esc + 2   --- make all panes even and split horizontal
```

To make it look and feel slightly better I use the following configuration:
```bash
And first of all you need to install Tmux Plugin Manager TPM - https://github.com/tmux-plugins/tpm
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

In TPM repository are all the necessary instructions to install and remove plugins.
But all you need to know is that plugins are installed by typing PrefiX + I in TMUX.

TMIX configuration is specifed in ~/.config/tmux/tmux.conf :

# Enable 24-bit color mode
set-option -sa terminal-overrides ",xterm*:Tc"

# Start windows and panes at 1, not 0
set -g base-index 1
set -g pane-base-index 1
set-window-option -g pane-base-index 1
set-option -g renumber-windows on

# Split window from current dir
bind '"' split-window -v -c "#{pane_current_path}"
bind % split-window -h -c "#{pane_current_path}"

# Enable mouse support
set -g mouse on

# Install plugins --- can be installed in TMUX with Prefix+I
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'dreamsofcode-io/catppuccin-tmux'

run '~/.tmux/plugins/tpm/tpm'
```

All these settings I have borrowed from one awesome guy on youtube :  
<https://www.youtube.com/watch?v=DzNmUNvnB04/>  
Thanks, friend, for sharing your experience.

A very nice and short cheatsheet from pluralsight - <https://www.pluralsight.com/resources/blog/cloud/tmux-cheat-sheet/>

