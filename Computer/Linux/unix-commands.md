---
date: 2024-07-18 (Thu)
---

# Unix Commands

## System

- `uptime`
- `free`
- `top`
- `ps`
  - `-f` full format in current session
  - `-fu` full format in all sessions by same user
  - `-e` all processes
- `pstree`
- `kill`
  - `-l` list signal names
  - `-9` SIGKILL
  - `-15` SIGTERM
-

## Users

- `who`
- `whoami`
- `finger`
- `id`
- `groups`

## File Name

- `basename`
- `dirname`
- `realpath`

## File Comparison

- `cmp` compare byte by byte and complains (non-zero exit) if two files differ
- `diff` show difference
  - `-y` side-by-side comparison

## Data Wrangling

- `tee`
- `sort` - by default, 3 < b < B < z < Z < !
  - `sort -t ":" -k 2` sort by 2nd field using `:` as delimiter
  - `sort -n` sort numerically
  - `sort -u` sort and remove duplicates
- `uniq`
- `head`
- `tail`
- `wc`
- `cut` - print selected parts of lines (e.g. columns)
- `sed` - stream editor for filtering and transforming text
- `awk` - pattern scanning and processing language

### Data Formatting

- `expand` - convert tabs to spaces
- `tr` - translate or delete characters
  - `tr '[a-z] [A-Z] < file.txt'` convert lowercase to uppercase

## Command Augmentation

- `sleep`
- `timeout`
- `xargs` - build and execute command lines from standard input
  - `-I %` defines `%` as a placeholder for the input (e.g. `xargs -I % echo %`)

## Calculations

- `bc`

## 🧭 Navigation

- [🔼 Back to top](#unix-commands)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
