# tmux
Tmux is the best tools for Linuxer

# NOW, let's deploy and configure your tmux

## 1. Install zsh & tmux

```shell
yum install libevent-devel ncurses-devel zsh tmux -y
```

## 2. set your account shell style from bash to zsh

```shell
# vim /etc/passwd

# before
test:x:1001:1002::/home/test:/bin/bash

# after
test:x:1001:1002::/home/test:/bin/zsh
```

## 3. create team group and set your account to team group

```shell
groupadd team

# vim /etc/group

# before
team:x:1001:

# after
team:x:1001:root,test
```

## 4. copy tm.conf to /etc/ path

```shell
# /etc/tm.conf
unbind-key C-b
set-option -g prefix C-s
bind-key s send-prefix
bind-key C-s last-window

set-option -g default-shell /bin/zsh

set-option -s escape-time 10

#  set-option -g status-bg blue
set-option -g history-limit 100000

#  set-option -g pane-base-index 1
#  set-option -g base-index 1

bind -n M-j previous-window
bind -n M-k next-window

# select an open window direct with ALT+ 0-9
unbind -n M-0
unbind -n M-1
unbind -n M-2
unbind -n M-3
unbind -n M-4
unbind -n M-5
unbind -n M-6
unbind -n M-7
unbind -n M-8
unbind -n M-9
bind -n M-0 select-window -t 0
bind -n M-1 select-window -t 1
bind -n M-2 select-window -t 2
bind -n M-3 select-window -t 3
bind -n M-4 select-window -t 4
bind -n M-5 select-window -t 5
bind -n M-6 select-window -t 6
bind -n M-7 select-window -t 7
bind -n M-8 select-window -t 8
bind -n M-9 select-window -t 9


# use "v" and "s" to do vertical/horizontal splits, like vim
bind s split-window -v
bind v split-window -h

# split panes using | and -
bind | split-window -h
#bind \ split-window -h
bind - split-window -v

# use the vim motion keys to move between panes
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# switch panes using Alt-arrow without prefix
#  bind -n M-Left	select-pane -L
#  bind -n M-Right select-pane -R
#  bind -n M-Up	select-pane -U
#  bind -n M-Down	select-pane -D

# use the vim resize keys.
# the number at the end is how much the pane will be resized,
# and 1 is fairly small -- you might want to tweak this.
#  bind - resize-pane -D 1
#  bind + resize-pane -U 1
#  bind < resize-pane -L 1
#  bind > resize-pane -R 1

# resize panes with vim movement keys
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

######### copy/paste ###########
bind C-u copy-mode -u
bind C-d copy-mode

# these won't work...
#  bind C-; copy-mode
#  bind-key "\033<C-;>" copy-mode
bind-key -n KP5 copy-mode

# use vim motion keys while in copy mode
setw -g mode-keys vi

# Urxvt middle click
# https://wiki.archlinux.org/index.php/tmux
set-option -ga terminal-override ',rxvt-unicode*:colors=256:Tc:XT:Ms=\E]52;%p1%s;%p2%s\007,xterm*:Tc'

#  set -g default-terminal "tmux"

# E558: Terminal entry not found in terminfo
# 'tmux-256color' not known. ...
# solution: install the uptodate ncurses-term package
# well this breaks mouse in vim.. worked around by setting vim's ttymouse option
#set -g default-terminal "tmux-256color"
set -g default-terminal "screen-256color"

#  This breaks Italics
#  https://github.com/tmux/tmux/issues/175
#  https://github.com/tmux/tmux/commit/7382ba82c5b366be84ca55c7842426bcf3d1f521
#  set -g default-terminal "screen-256color"

# Enable mouse mode (tmux 2.1 and above)
set -g mouse on

set-option -g set-clipboard on

#  bind-key -t emacs-copy	MouseDragEnd1Pane copy-pipe "xclip -in -selection primary"
#  bind-key -Tcopy-mode-vi	MouseDragEnd1Pane copy-pipe "xclip -in -selection primary"

#  unbind -n MouseDrag1Pane
#  unbind -temacs-copy MouseDrag1Pane
#  unbind -tvi-copy MouseDrag1Pane

#bind-key -Tcopy-mode-vi 'v' send -X begin-selection
#bind-key -Tcopy-mode-vi 'x' send -X copy-selection
# bind-key 'p'			run-shell "tmux set-buffer \"$(xclip -out -selection clipboard)\"; tmux paste-buffer"
# bind-key 'y' send-keys x\;	run-shell "tmux show-buffer | xclip -in -selection clipboard"\; display-message "copied"

# http://invisible-island.net/xterm/ctlseqs/ctlseqs.html
# send DATA to PRIMARY
# echo -e "\033]52;;$(echo DATA|base64)\a"
# send DATA to CLIPBOARD
# echo -e "\033]52;c;$(echo DATA|base64)\a"

#  bind-key -Tcopy-mode-vi 'y' copy-pipe "xsel -i"
#bind-key -Tcopy-mode-vi 'y' send -X copy-pipe "xsel -i"
#bind-key -Tcopy-mode-vi '>' send -X copy-pipe 'cat > /tmp/screen-exchange'
#  bind-key p run "xsel -o | tmux load-buffer - ; tmux paste-buffer"

# # Double LMB Select & Copy (Word)
# bind-key -T copy-mode-vi DoubleClick1Pane \
#     select-pane \; \
#     send-keys -X select-word-no-clear \; \
#     send-keys -X copy-pipe-no-clear "xclip -in -sel primary"
# bind-key -n DoubleClick1Pane \
#     select-pane \; \
#     copy-mode -M \; \
#     send-keys -X select-word \; \
#     run-shell "sleep .5s" \; \
#     send-keys -X copy-pipe-no-clear "xclip -in -sel primary"
# 
# # Triple LMB Select & Copy (Line)
# bind-key -T copy-mode-vi TripleClick1Pane \
#     select-pane \; \
#     send-keys -X select-line \; \
#     send-keys -X copy-pipe-no-clear "xclip -in -sel primary"
# bind-key -n TripleClick1Pane \
#     select-pane \; \
#     copy-mode -M \; \
#     send-keys -X select-line \; \
#     run-shell "sleep .5s" \; \
#     send-keys -X copy-pipe-no-clear "xclip -in -sel primary"

# Double LMB Select & Copy (Word)
# bind-key -n DoubleClick1Pane \
#     select-pane \; \
#     copy-mode -M \; \
#     send-keys -X select-word \; \
#     run-shell "sleep .5s" \; \
#     send-keys -X copy-pipe-and-cancel "xclip -in -sel primary"

bind-key -n M-o capture-pane -J -S -3000 \; new-window ' \
  tmux save-buffer - \; delete-buffer | head -n-1 2>/dev/null| {    \
    vim --noplugin -c "set nofen is hls ic ls=0" + -;              \
    test $? -eq 127 && less;                              \
  };                                                      \
' \; send-keys C-L ? # go to bottom and search upward
#  ' \; send-keys y , , p # go to bottom and search upward with vim-easyoperator-phrase

# bind-key -n M-o capture-pane -J -e -S -3000 \; new-window ' \
#   tmux save-buffer - \; delete-buffer | head -n-1 | {    \
#     vim + -c "set nowrap nofen is hls ic ls=0" +'AnsiEsc' -;              \
#     test $? -eq 127 && less;                              \
#   };                                                      \
# ' \; send-keys C-L ? # go to bottom and search upward
#  ' \; send-keys C-L y , , p # go to bottom and search upward with vim-easyoperator-phrase

bind      m new-window -n mail "mutt"
bind -n M-m new-window -n mail "mutt"
bind -n M-w new-window

bind r source-file ~/.tmux.conf

#  set-option -g allow-rename on
#setw -g allow-rename of
#set -g set-titles on
#set -g set-titles-string "#T"

## status bar
# all
set -g status-bg white
#set -g status-attr dim

# left
set -g status-left-length 30
if-shell 'test -z "$SSH_CLIENT"' 'set -g status-left "#[fg=blue]"' 'set -g status-bg colour218; set -g status-left "#[fg=red,bright,bg=yellow]#{host_short}#[fg=blue]"'
if-shell 'test #{host} = inn' 'set -g status-bg colour224'

# right
set -g status-right ""
set -g status-interval 0
#  set -g status-right "#[fg=blue]%H:%M:%S#[default]"
#  set -g status-interval 1

## window options
setw -g window-status-format '#[fg=blue] #I#{?window_flags,#{window_flags}, }#W'
setw -g window-status-current-format '#[fg=red,bold] #I#F#W'
#setw -g window-status-current-attr "underscore"

# per-host setup
# http://unix.stackexchange.com/questions/50001/tmux-conf-embedding-a-shell-script
```

## 5. copy tm to /usr/local/bin/ path, then change tm permission

+ /usr/local/bin/tm

```shell
#!/bin/bash

session=$1
[[ $session ]] || session=pair

[[ -d /tmp/tmux ]] || {
        mkdir /tmp/tmux
	chown .team /tmp/tmux
	chmod 777 /tmp/tmux
} 
sock=/tmp/tmux/$session.sock

if tmux -S /tmp/tmux/$session.sock ls 2>/dev/null; then
	tmux -f /etc/tm.conf -S $sock attach -t $session
#	tmux -S $sock attach -t $session
else
	tmux -f /etc/tm.conf -S $sock new-session -s $session "chgrp team $sock; zsh"
#	tmux -f /etc/tm.conf -S $sock new-session -s $session
#	tmux -S $sock new-session -s $session
fi
```

+ change tm permission

```shell
chmod 777 /usr/loacl/bin/tm
```

## 6. copy .zshrc to your account $HOME/.zshrc, then source

+ $HOME/.zshrc

```shell
# $HOME/.zshrc
#
# refer: [zsh command PROMPT](https://www.cnblogs.com/b-ruce/p/5851624.html)
#
# %n account name
# %M all hostname
# %. relative directory
# %% one '%'
# zsh style

PROMPT='%n@%M %. %% '
```

+ source

```shell
source $HOME/.zshrc
```

+ show the effect

```shell
test@openEuler ~ % 
```

## 7. Let's start using tmux for team work

+ eg.

account1(root)

```shell
tm abc
```

account2(test)

```shell
tm abc
```

> when account1(root) use tmux(tm abc), account2(test) also use tmux(tm abc),
> when account1 is working by operating on him terminal,
> account2 will look at account1's operating on her terminal.
> As you can see, Tmux is the best tools.

+ common shortcut keys

```
general operation

ctrl - s c  create new window
ctrl - s d  quit window (save window status)
ctrl - s x  close window (no save window status)
alt - j/k   left/right window change
ctrl - s p  previous(last) window
ctrl - s n  next window

split screen

ctrl - s %  left/right split windows
ctrl - s "  up/down split window
ctrl - s {  could lookup, by press shortcut key 'v' and K(up)J(down)H(left)L(right) to choose your need, by press shortcut key 'yy' to copy them
ctrl - s }  by press shortcut key 'p' paste
ctrl - s z  full screen this split window
```

