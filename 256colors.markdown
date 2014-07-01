# How to get 256 colors in Linux

* Configure Konsole to set TERM to `xterm-256color`.
* Add `set -g default-terminal "screen-256color"` in `~/.tmux.conf`.

Vim and other programs capable of 256 (e.g. zsh) should start using 256 colors
automatically now.
