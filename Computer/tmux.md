---
title: tmux
---

# Invoke Tmux in Terminal

`tmux [OPT]... [CMD [CMD-OPT]...]`

## Tmux options

- `-2` forces tmux to assume the terminal supports 256 colours
  - Equivalent to `-T 256`
- `-c {SHELL-CMD}`
- `-f {FILE}` specifies config file

# Tmux keybinds

## Prefix table

Keybind followed by following symbols has meanings:

- "\*": Custom keybinds
- "^": Custom behaviour extending the default behaviour
- "#": Plugin keybinds

### Misc

| Keybind    | Effect                                |
| ---------- | ------------------------------------- |
| `:`        | Command prompt                        |
| `[`        | Enter copy mode                       |
| `]`        | Paste from last tmux clipboard buffer |
| `=`        | Choose a buffer to paste              |
| `#`        | List tmux clipboard                   |
| `-`        | Delete last tmux clipboard buffer     |
| `<C-e>` \* | Popup floating terminal               |
| `?`        | List default keybinds and effect      |
| `z`        | Zoom current pane                     |
| `t`        | Display clock in current pane         |
| `R` \*     | Resource `tmux.conf`                  |
| `$`        | Rename current session                |
| `m`        | Toggle [mark](#mark)                  |
| `M`        | Remove [mark](#mark)                  |

### Layout

| Keybind   | Effect                       |
| --------- | ---------------------------- |
| `%`       | Split pane vertically        |
| `"`       | Split pane horizontally      |
| `}`       | Swap with next pane          |
| `{`       | Swap with prev pane          |
| `<Space>` | Change layout                |
| `<A-1>`   | Equal vertical layout        |
| `<A-2>`   | Equal horizontal layout      |
| `<A-3>`   | Main pane above layout       |
| `<A-4>`   | Main pane on the left layout |
| `<A-5>`   | Main pane below layout       |

### Windows

| Keybind | Effect                                                         |
| ------- | -------------------------------------------------------------- |
| `<` \*  | Swap with prev window (can repeat)                             |
| `>` \*  | Swap with next window (can repeat)                             |
| `.` ^   | Assign new index (swap indices if other window has that index) |
| `,`     | Rename window                                                  |
| `c`     | Create new window                                              |
| `!`     | Break pane to a new window                                     |

### Navigation

| Keybind    | Effect                                          |
| ---------- | ----------------------------------------------- |
| `o`        | To next pane                                    |
| `<A-q>` \* | Switch to pane above and zoom (can wrap-around) |
| `n`        | To next window (can repeat)                     |
| `p`        | To prev window (can repeat)                     |
| `l`        | To last window                                  |
| `[0-9]`    | To window at index                              |
| `L`        | To last session                                 |
| `s`        | Session selection mode                          |
| `w`        | Window selection mode                           |
| `f`        | Find a [node](#node-selection-mode)             |

## Node selection mode

You can go to window/session/pane (**_node_**) selection mode with `<prefix>w`,
`<prefix>s`, or `<prefix>f` followed by a search string.

- `j`, `k` move cursor up/down
- `h`, `l` shrink/expand children list of a node. `-` and `<S-=>` also.
- `g`, `G` move cursor to top/bottom does this
- `<S-o>` changes sort method (index, name, time)
- `r` reverses sort order
- `/` searches for node name and cursor jump to that
- `f` filters window/session\
  takes a format string, and only list nodes where the string evaluates to true
- `t`: I guess it marks nodes as target for `-t` flag
- `m` marks a window or pane? `M` clears mark. (target for `-s` flag)
- `x` kills a node
- `v` toggles preview
- `:` starts inputting tmux commands

## tmux-sessionist

| Keybind | Effect                                 |
| ------- | -------------------------------------- |
| `g`     | Prompt for session name and switch     |
| `C`     | Prompt for session name and create new |
| `X`     | Kill current session without detaching |
| `S`     | Switch to last session (same as `L`)   |
| `@`     | Promote pane into new session          |
| `C-@`   | Promote window into new session        |

- `t<secondary-key>` join currently marked pane (`m`) to current session/window,
  and switch to it. Secondary keys:
  - `h`, `-`, `"`: join horizontally
  - `v`, `|`, `%`: join vertically
  - `f`, `@`: join full screen

# Tmux Commands

## Executing commands

`<prefix>:` enters to command prompt.

## List of commands

- `display-message` (`display`) print message in the status line.
- `command-prompt -p 'prompt message:' 'command template %1'`

## Control statements

- `if-shell` (`if`)

  - `if-shell {shell-command} {true-command} [false-command]`
  - shell-command is expanded with tmux format first

## Windows

- `select-window`
- `swap-window`

# Tmux scripts

## Basic Syntax

Line continuation with `\` at the end of line.

## String

- Things inside `''` is taken literally.
- `""` allows special characters and expansion. E.g. `"${myenv}"`.
- `#{}` in `""` is tmux format string.

# Concepts explanation

## Mark

Marked pane is the default target for `-s` to `join-pane`, `move-pane`,
`swap-pane` and `swap-window`.

Only one pane can be marked at a time. Marking a new pane remove existing mark.

## Nodes

A node is either a session, a window or a pane.

# 🧭 Navigation

- [🔼 Back to top](#)
- [📑 Notes Index](../index.md)
- [🗃️ Index](/media/mikeX/Nextcloud/index.md)
