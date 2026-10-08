# tmux Cheat Sheet

All key bindings use the default prefix `Ctrl+b`: press `Ctrl+b`, release it,
then press the key. Commands starting with `:` are typed into the command
prompt, which opens with `Ctrl+b :`.

!!! note "Version"
    tmux 3.8. Everything without a version mark also works in tmux 3.4.
    Check yours with `tmux -V`, distributions often ship older versions.

## Sessions

| Command | Description |
| --- | --- |
| `tmux` | Start a new session |
| `tmux new -s mysession` | Start a new session named `mysession` |
| `tmux new -A -s mysession` | Attach to `mysession`, create it if it does not exist |
| `:new` | Start a new session from inside tmux |
| `tmux ls` | List all sessions |
| `tmux a` | Attach to the last session |
| `tmux a -t mysession` | Attach to `mysession` |
| `:attach -d` | Detach all other clients from the current session |
| `tmux kill-session -t mysession` | Kill `mysession` |
| `tmux kill-session -a` | Kill all sessions except the current one |
| `tmux kill-session -a -t mysession` | Kill all sessions except `mysession` |
| `:kill-session` | Kill the current session |
| `tmux kill-server` | Kill the server and all sessions |
| `Ctrl+b d` | Detach from the session |
| `Ctrl+b $` | Rename the session |
| `Ctrl+b s` | Show all sessions |
| `Ctrl+b w` | Show all sessions and their windows |
| `Ctrl+b (` | Previous session |
| `Ctrl+b )` | Next session |
| `Ctrl+b L` | Last used session |
| `Ctrl+b Shift+Tab` | Quick session switcher (3.8+) |

## Windows

| Command | Description |
| --- | --- |
| `tmux new -s mysession -n mywindow` | Start a session with a named first window |
| `Ctrl+b c` | Create a window |
| `Ctrl+b ,` | Rename the window |
| `Ctrl+b &` | Close the window |
| `Ctrl+b p` | Previous window |
| `Ctrl+b n` | Next window |
| `Ctrl+b l` | Last used window |
| `Ctrl+b 0` ... `9` | Go to window by number |
| `Ctrl+b '` | Go to window by number, also above 9 |
| `Ctrl+b f` | Find a window by name |
| `Ctrl+b Tab` | Quick window switcher (3.8+) |
| `Ctrl+b <` | Open the window menu |
| `:swap-window -s 2 -t 1` | Swap windows 2 and 1 |
| `:swap-window -t -1` | Move the window one position to the left |
| `:move-window -t 5` | Move the window to number 5 |
| `:move-window -r` | Renumber all windows without gaps |

## Panes

| Command | Description |
| --- | --- |
| `Ctrl+b %` | Split side by side (`:split-window -h`) |
| `Ctrl+b "` | Split top and bottom (`:split-window -v`) |
| `Ctrl+b Up/Down/Left/Right` | Move to the pane in that direction |
| `Ctrl+b o` | Next pane |
| `Ctrl+b ;` | Last used pane |
| `Ctrl+b q` | Show pane numbers, press a number to jump to it |
| `Ctrl+b z` | Zoom the pane in and out |
| `Ctrl+b {` | Swap with the previous pane |
| `Ctrl+b }` | Swap with the next pane |
| `Ctrl+b Space` | Cycle through layouts |
| `Ctrl+b Alt+1` ... `7` | Switch to a preset layout (6 and 7 need 3.5+) |
| `Ctrl+b E` | Spread the panes evenly |
| `Ctrl+b Ctrl+Arrow` | Resize the pane by 1 cell |
| `Ctrl+b Alt+Arrow` | Resize the pane by 5 cells |
| `Ctrl+b !` | Move the pane into its own window |
| `:join-pane -s 2 -t 1` | Move the pane of window 2 into window 1 |
| `:setw synchronize-panes` | Toggle typing into all panes at once |
| `Ctrl+b T` | Change the pane title (3.8+) |
| `Ctrl+b *` | Open a floating pane (3.7+) |
| `Ctrl+b g` | Move or resize a floating pane (3.8+) |
| `Ctrl+b x` | Close the pane |
| `Ctrl+b >` | Open the pane menu |

## Copy Mode

The keys below assume vi mode. tmux uses emacs keys by default, enable vi
mode with `:setw -g mode-keys vi` or in the [config file](#config-file).

| Command | Description |
| --- | --- |
| `Ctrl+b [` | Enter copy mode |
| `Ctrl+b PgUp` | Enter copy mode and scroll up one page |
| `q` | Quit copy mode |
| `g` / `G` | Go to the top / bottom |
| `h` `j` `k` `l` | Move the cursor |
| `w` / `b` | Next / previous word |
| `/` / `?` | Search forward / backward |
| `n` / `N` | Next / previous search match |
| `Space` | Start the selection |
| `Esc` | Clear the selection |
| `Enter` | Copy the selection and quit |
| `Ctrl+b ]` | Paste the latest buffer |

## Buffers

| Command | Description |
| --- | --- |
| `:show-buffer` | Show the latest buffer |
| `:capture-pane` | Copy the visible pane content into a buffer |
| `:list-buffers` | List all buffers (`Ctrl+b #`) |
| `:choose-buffer` | Pick a buffer to paste (`Ctrl+b =`) |
| `:save-buffer buf.txt` | Save the latest buffer to a file |
| `:delete-buffer -b 1` | Delete buffer 1 (`Ctrl+b -` deletes the latest) |

## Misc

| Command | Description |
| --- | --- |
| `Ctrl+b :` | Open the command prompt |
| `:set -g OPTION` | Set a session option for all sessions |
| `:setw -g OPTION` | Set a window option for all windows |
| `:set mouse on` | Enable the mouse (default since 3.8) |
| `:source-file ~/.tmux.conf` | Reload the config file |
| `tmux -V` | Show the tmux version |

## Config File

A minimal starting point for `~/.tmux.conf`. Reload it with
`:source-file ~/.tmux.conf` or restart tmux.

```bash
# Mouse support for selecting panes, resizing and scrolling (default since 3.8)
set -g mouse on

# vi keys in copy mode
setw -g mode-keys vi

# Number windows and panes from 1 instead of 0
set -g base-index 1
setw -g pane-base-index 1
set -g renumber-windows on

# Keep more scrollback
set -g history-limit 10000

# Optional: use Ctrl+a as prefix instead of Ctrl+b
# unbind C-b
# set -g prefix C-a
# bind C-a send-prefix
```

## Help

| Command | Description |
| --- | --- |
| `Ctrl+b ?` | List all key bindings |
| `tmux list-keys` | List all key bindings from the shell |
| `tmux info` | Show server and terminal info |
| `man tmux` | Full manual |
