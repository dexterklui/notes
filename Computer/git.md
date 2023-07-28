---
title: Git
date: 2023-07-25 (Tue)
---

# Commit Message

## 📛 Title prefix

One practice from plugin development is to add title prefix to describe the type
of commit:

- `feat(type):` features
- `fix(type):` fixes
- `refactor(type):`
- `chore(type):` minor changes
  - `chore(main): release 1.2.0`
- `docs:`
- `build:`
- `style:`

A `!` before the `:` indicates that it introduced a breaking changes.

# ⤵️ Merge

## Merge strategy

When doing `git merge`, the flag `-s` chooses a merge strategy.

- `ort`: Default for two heads
- `octopus`: Default for more than two heads
- `ours`: Ignoring other branches, i.e. only merge for the git history

# 🍒 Cherry Pick

## Synopsis

To initiate: `git cherry-pick [opts]... <commit>...`

In process: `git cherry-pick (--continue|--skip|--abort|--quit)`

## Options

| Option | Description                         |
| ------ | ----------------------------------- |
| `-e`   | Edit commit message                 |
| `-x`   | Append line recording source commit |
| `-n`   | Don't commit; fill working tree     |

## Sequencer subcommands

| Command      | Description                                   |
| ------------ | --------------------------------------------- |
| `--continue` | Continue after resolving conflicts            |
| `--skip`     | Skip current commit                           |
| `--quit`     | Forget current operation; keep merged commits |
| `--abort`    | Cancel whole operation; undo merged commits   |

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](../index.md)
- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
- [🗃️ Master Index](../../index.md)
