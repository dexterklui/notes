---
title: Mkdnflow
date: 2023-07-22 (Sat)
---

# ⌨️ Keybind

## Normal mode

### Link navigation

| Key       | Context      | Function                   |
| --------- | ------------ | -------------------------- |
| `<CR>`    | On a link    | Open link                  |
|           | On a heading | Toggle fold                |
|           | On a word    | Create link                |
| `<BS>`    | -            | Back to last-active buffer |
| `<Tab>`   | -            | Cursur to next link        |
| `<S-Tab>` | -            | Cursor to previous link    |
| `]]`      | -            | Cursor to next heading     |
| `[[`      | -            | Cursor to previous heading |

### Link modification

| Key        | Context      | Function                                 |
| ---------- | ------------ | ---------------------------------------- |
| `<Lead>p`  | On a word    | Create link w/ URL from clipboard        |
| `<Lead>fc` | On a link    | Open link; create missing dirs           |
| `<A-CR>`   | On a link    | Destroy link; leave only text in `[ ]`   |
| `<F2>`     | On a link    | Prompt for new path and move link        |
| `yaa`      | On a heading | Yank formatted anchor link               |
| `yfa`      | On a link    | Yank formatted anchor link with filename |

### Document formatting

| Key        | Context        | Function                |
| ---------- | -------------- | ----------------------- |
| `+`        | On a heading   | Move up heading level   |
| `-`        | On a heading   | Move down heading level |
| `<C-Spc>`  | -              | Toggle todo status      |
| `<Lead>nn` | On number list | Update numbering        |
| `<Lead>ir` | On a table     | Add new row below       |
| `<Lead>iR` | On a table     | Add new row above       |
| `<Lead>ic` | On a table     | Add new column right    |
| `<Lead>iC` | On a table     | Add new column left     |

### Document navigation

| Key       | Context      | Function                            |
| --------- | ------------ | ----------------------------------- |
| `]v`      | On a table   | To next cell; also format table     |
| `[v`      | On a table   | To previous cell; also format table |
| `<Lead>f` | On a section | Fold section                        |
| `<Lead>F` | On a section | Unfold section                      |

## Insert mode

| Key       | Context    | Function                             |
| --------- | ---------- | ------------------------------------ |
| `<Tab>`   | In a table | Jump to next cell; also format table |
| `<S-Tab>` | In a table | Jump to prev cell; also format table |

## Visual mode

| Key       | Context      | Function                          |
| --------- | ------------ | --------------------------------- |
| `<CR>`    | On a link    | Open link                         |
|           | On a word    | Create link w/ word               |
|           | On a heading | Toggle fold                       |
| `<Lead>p` | On a word    | Create link w/ word               |
| `<A-CR>`  | -            | Tag selected span of text with ID |

# Command

| Command             | Context | Effect                            |
| ------------------- | ------- | --------------------------------- |
| `MkdnTable x y`     | -       | Create table w/ x cols and y rows |
| `MkdnTable x y noh` | -       | Like above but exclude headers    |

# Link

-   Can append id `path/to/file#id-of-heading` to jump to a particular section

# 🧭 Navigation

-   [🔼 Back to top](#)
-   [◀️ Back](index-nvim.md)
-   [📑 Index](/media/mikeX/Nextcloud/index.md)
