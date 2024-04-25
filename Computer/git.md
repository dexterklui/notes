---
date: 2023-07-25 (Tue)
---

# Git

## 📛 aliases

Some of them aren't set by me and I don't know where do they come from, but they
are handy:

### Git log aliases

| Command | Description                                                  |
| ------- | ------------------------------------------------------------ |
| `glo`   | `git log --oneline --decorate`                               |
| `glog`  | `git log --oneline --decorate --graph`                       |
| `gloga` | `git log --oneline --decorate --graph --all`                 |
| `glol`  | `git log --graph --pretty=` oneline w/ time elapsed & author |
| `glola` | Like `glol` but w/ `--all`                                   |
| `glols` | Like `glol` but w/ `--stat`                                  |
| `glod`  | `git log --graph --pretty=` oneline w/ timestamp & author    |
| `glods` | Like `glol` but w/ `--date=short`                            |
| `glg`   | `git log --stat`                                             |
| `glgg`  | `git log --graph`                                            |
| `glgga` | `git log --graph --decorate --all`                           |
| `glgm`  | `git log --graph --max-count=10`                             |
| `glgp`  | `git log --stat --patch`                                     |

| Command  | Description                         |
| -------- | ----------------------------------- |
| `gcount` | `git shortlog --summary --numbered` |

### Git commit aliases

| Command | Description                  |
| ------- | ---------------------------- |
| `gc`    | `git commit --verbose`       |
| `gcs`   | `git commit --gpg-sign`      |
| `gcas`  | `git commit --all --signoff` |

### Git checkout aliases

| Command | Description                          |
| ------- | ------------------------------------ |
| `gcm`   | `git checkout $(git_main_branch)`    |
| `gcd`   | `git checkout $(git_develop_branch)` |
| `gco`   | `git checkout`                       |
| `gcb`   | `git checkout -b`                    |

### Git stash aliases

| Command  | Description                          |
| -------- | ------------------------------------ |
| `gsta`   | `git stash push`                     |
| `gstu`   | `git stash push --include-untracked` |
| `gstall` | `git stash --all`                    |
| `gstp`   | `git stash pop`                      |
| `gstaa`  | `git stash apply`                    |
| `gstd`   | `git stash drop`                     |
| `gstc`   | `git stash clear`                    |

| Command | Description             |
| ------- | ----------------------- |
| `gstl`  | `git stash list`        |
| `gsts`  | `git stash show --text` |

### Git branch aliases

| Command | Description                   |
| ------- | ----------------------------- |
| `gb`    | `git branch`                  |
| `gba`   | `git branch --all`            |
| `gbr`   | `git branch --remote`         |
| `gbd`   | `git branch --delete`         |
| `gbD`   | `git branch --delete --force` |
| `gbnm`  | `git branch --no-merged`      |

### Git bisect aliases

| Command | Description        |
| ------- | ------------------ |
| `gbs`   | `git bisect`       |
| `gbss`  | `git bisect start` |
| `gbsg`  | `git bisect good`  |
| `gbsb`  | `git bisect bad`   |
| `gbsr`  | `git bisect reset` |

### Git other aliases

| Command | Description         |
| ------- | ------------------- |
| `gst`   | `git status`        |
| `gp`    | `git push`          |
| `gl`    | `git pull`          |
| `gpr`   | `git pull --rebase` |
| `gbl`   | `git blame -b -w`   |

## 📝 Commit Message

### Why use conventional commit messages

- Automatically generating CHANGELOGs
- Automatically determining a semantic version bump
- Communicating the nature of changes
- Triggering build and publish processes
- Makes the life of contributors easier when reading commit history

### 📛 Subject prefix

One practice from plugin development is to add subject prefix to describe the
type of commit, `<type>(<scope>)[!]:`.

- `feat(type):` new feature
- `fix(type):` bug fix
- `refactor(type):` code change that neither fixes a bug nor adds a feature
- `perf:` code change that improves performance
- `style:` changes that do not affect the meaning of the code
- `chore(type):` minor changes
  - `chore(main): release 1.2.0`
- `docs:` documentation only changes
- `build:` Changes that affect the build system or external dependencies
  (example scopes: gulp, broccoli, npm)
- `ci:` changes CI configuration files and scripts
- `test:` adds missing tests or correcting existing tests
- `revert: <header of reverted commit>`
  - In the body: `This reverts commit <hash>`.

A `!` before the `:` indicates that it introduced a breaking changes.

### Subject

- Try no more than **50** chars
- Use imperative; No tenses
- Don't capitalize first letter
- No dot `.` at the end
- Good if include the places of change (more efficient that checking what files
  are changed manually)

### Body

- Try no more than **78** chars (some newer standard is 100)
- Use imperative; No tenses
- Include motivation for the change and contrast with previous behaviour

### Footer

- Reference GitHub issues that this commit **closes**
  - `Close #234, #245` / `Fix #501` / `Resolve octo-org/octo-repo#100`
  - Note that the pull request must be on the **default** branch for GitHub to
    recognise
- Info for
  [breaking changes](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#)
  - `BREAKING CHANGE: <summary>` followed by an optional body describing
    details; or
  - `BREAK MyClass.method1, which was removed (use mdthod2 instead)`
- Other [references](https://git-scm.com/docs/git-interpret-trailers):
  - `Fix #42`
  - `Refs: #123` (or for reverted commits `Refs: 676104e, a215868`)
  - `Reference-to: 8bc9a0c769 (Add copyright notices., 2005-04-07)`
  - `See-also: fe3187489d69c4 <subject of related commit>`
  - `Signed-off-by: Bob <bob@example.com>`
  - `Acked-by: Alice <alice@example.com>`
  - `Helped-by: Junio C Hamano <junioh@example.com>`

### Git bisect

With consistent format in commit messages, you can find relevant commits with
`git bisect`.

## Select Ranges of Commits

- `<commit>^` selects the parent of a commit (refs and HEAD also works)
- `<commit>^n:` selects an n-th **alternative** parent of a merge commit. The
  parent commit that is in the branch where the merge was done is the first
  parent, others are the 2nd, 3rd ... etc.
- `<commit>~` selects the parent of a commit.
- `<commit>~~` selects the grand-parent of a commit.
- `<commit>~3` selects the 3rd parents of a commit. Only select the 1st
  alternative parent at each ancestral level.
- `HEAD~3^2` selects the 2nd alternative parent of the great-grand parent of
  HEAD.
- `<commit-1>..<commit-2>` selects the commit that are reachable by commit_2 but
  not by commit_1.
  - I tend to think it as the **missing link** from commit_1 to commit_2. But
    note that it doesn't mean commit_1's tree will become commit_2's if it
    incorporate the missing link, for there may be a missing link in reverse
    order as well. This happens when two commits diverge.
  - If either one side is omitted, Git assumes it to be HEAD. E.g.
    `origin/master..` selects all commits reachable by the current branch but
    not by origin/master.
- `^<commit> <commit-i>...` selects all commits reachable by any of the
  commit_i... but not by commit (that one with a `^`).
  - This is equivalent to `--not :commit commit_i...:`
- `<commit-1>...<commit-2>`, where the `...` is typed literally, selects all
  commits that are reachable by **either** of the two commits but **not both**
  of them.
  - Usually with option `--left-right` to show a `<`/`>` to show which side of
    the commit history each commit is in.

## ⤵️ Merge

### Merge strategy

When doing `git merge`, the flag `-s` chooses a merge strategy.

- `ort`: Default for two heads
- `octopus`: Default for more than two heads
- `ours`: Ignoring other branches, i.e. only merge for the git history

## 🍒 Cherry Pick

### Synopsis

To initiate: `git cherry-pick [opts]... <commit>...`

In process: `git cherry-pick (--continue|--skip|--abort|--quit)`

### Options

| Option | Description                         |
| ------ | ----------------------------------- |
| `-e`   | Edit commit message                 |
| `-x`   | Append line recording source commit |
| `-n`   | Don't commit; fill working tree     |

### Sequencer subcommands

| Command      | Description                                   |
| ------------ | --------------------------------------------- |
| `--continue` | Continue after resolving conflicts            |
| `--skip`     | Skip current commit                           |
| `--quit`     | Forget current operation; keep merged commits |
| `--abort`    | Cancel whole operation; undo merged commits   |

## 🔀 Good Workflow

- [Working on a branch with a dependence on another branch that is being reviewed](https://softwareengineering.stackexchange.com/questions/351727/working-on-a-branch-with-a-dependence-on-another-branch-that-is-being-reviewed)

## 🗂️ Rewrite Git History

### Examples

| Command              | Description                    |
| -------------------- | ------------------------------ |
| `commit --amend`     | Amend last commit              |
| `rebase -i <commit>` | Rebase commits onto `<commit>` |

## Creating git repo on other filesystem

On other filesystem, make sure you are the owner of the directory. Usually this
is set by the `uid` mount option, best set in `fstab` file.

On NTFS, if other methods fail, you can try the hack mentioned in the following
blog post:
[Creating git repo on NTFS in Linux](https://subzerodays.wordpress.com/2018/11/21/creating-git-repo-on-ntfs-in-linux/).

## 🧭 Navigation

- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
