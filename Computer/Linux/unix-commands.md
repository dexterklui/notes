---
date: 2024-07-18 (Thu)
---

# Unix Commands

## System

- `uptime`
- `free`
- `top`
- `ps`
  - `-f` full format in current session; `-F` extra full format; `-l` long
    format
  - `-u {username}` processes by effective user (user permission used by
    process)
  - `-U {username}` processes by real user (user who started the process)
  - `-fu` full format in all sessions by same user
  - `-e` all processes
- `pstree`
- `kill`
  - `-l` list signal names
  - `-9` SIGKILL
  - `-15` SIGTERM
  - `-19` SIGSTOP pause a (background) process
  - `-18` SIGCONT resume a paused process
- `pgrep {pattern}` find process IDs by name, `pkill {name}` kill processes by
  name
- `hostname` show or set the system's host name
- `nice {command}` runs command with adjusted niceness
  - `-n {value}` nice value to be added. 0 - 19 for normal user, -20 - 19 for
    system administrator. Without this option, the niceness added is 10.
- `renice {value} {pid}` change the niceness of a running process

### top

| Colummn            | Description                                        |
| ------------------ | -------------------------------------------------- |
| `PID`              | Process ID                                         |
| `USER`             | User running the process                           |
| `PR`               | Priority - how important the task is               |
| `NI`               | Nice value                                         |
| `VIRT`/`RES`/`SHR` | Memory usage information                           |
| `S`                | State of the process                               |
| `%CPU`             | How much of a single CPU core the process is using |
| `MEM`              | Its share of available memory                      |
| `TIME`             | The cumulative CPU time the process has used       |

### Sysstat Utilities

- [`iostat`](https://www.geeksforgeeks.org/iostat-command-in-linux-with-examples/)
  reports CPU and I/O statistics
  - `iostat [opt]... [device]... [interval [count]]`
  - `-x` - extended statistics
  - `-c` - only CPU statistics
  - `-d` - only device (disk) statistics
  - `-k` - in KB/s; `-m` - in MB/s
  - `-p [device,...]` - for specific device(s)
  - `-z` - suppresses zero statistics

### System Logging

See [System Logging](system-logging.md)

## Networking

- `ss` checks socket statistics and open ports
  - `-t` TCP
  - `-u` UDP
  - `-l` listening
  - `-n` numeric (do not try to resolve into hostnames)
  - `-p` shows process name
  - `-i` shows internal TCP information
- `netstat` shows open ports and active connections
  - Without any option, it shows active TCP connections with IP addresses and
    ports, and TCP state
  - `-a` active TCP connections with listening state, and listening UDP
  - `-f` shows fully qualified domain names
  - `-n` numeric (do not try to resolve into hostnames)
  - `-o` shows PID of the process
  - `-p` specifies protocol (one and only one only), e.g. `tcp`, `udp`, `icmp`,
    `ip`
  - `-r` shows routing table
  - Argument is a time interval in seconds to refresh the output
- `telnet` communicate with another host using the TELNET protocol
  - `telnet {host} [port]`

## Shell

- `env` print environment variables, or run a command in a modified environment
  - `env [opt]... [-] [name=value]... [command [arg]...]` run command
  - `-i` run with empty environment
- `declare` list or set variables
  - without options, it lists all variables (including functions)
  - `-a` indexed array
  - `-A` associative array
- `echo`
  - `-n` no newline at end
  - `-e` enable interpretation of backslash escapes
- `tput` set terminal features
  - `tput cols` number of columns
  - `tput lines` number of lines
- `alias` define or display aliases
  - `unalias` remove alias
- `command` run command with specified arguments, no alias lookup
- `nohup {command} > outputFile 2>&1` run command that continue after this
  terminal exits.
  - After terminal exists, the process becomes the child of `init` process
  - If you don't redirect explicitly, the stdin and stdout will be redirected by
    default to a file `nohup.out` in current directory
- `type {command}` display information about command, check if it is an builtin
  command or identify the path of the executable

### Environment Variable

- `export {var}={value}` set environment variable
  - `export {var}` set existing variable
- `unset {var}` unset environment variable

### Jobs

- `jobs` list jobs
- `fg` bring job to foreground
  - `fg %1` bring job 1 to foreground
- `bg %1` run job 1 in background

### History

- `history` show command history
- `!!` run last command
- `!n` run command number `n`
- `!string` run last command starting with `string`
- `!$` last argument of last command

## Scripting

### shift

`shift` shift positional parameters (arguments)

- Can accept a number to shift multiple times (number cannot be greater than
  `$#`)
- It shifts the argument list of **current scope** (of function or of script)
- After doing `while getopts` you can do `shift $((OPTIND - 1))` to discard
  options in the argument list

### getopts

`getopts` parse short-form options in positional parameters

#### Short-form options

A short-form option is a single letter following a dash, e.g.:

- `-a`
- `-abc` has three options `a`, `b`, and `c`
- `-ab -c` also has three options `a`, `b`, and `c`

#### Simple example of getopts

Use getopts in a while loop to parse options:

```bash
while getopts abc opt; do
  case "$opt" in
    a) echo "Option a";;
    b) echo "Option b";;
    c) echo "Option c";;
    \?) echo "Invalid option";;
  esac
done

shift $((OPTIND - 1))
```

What `getopts abc opt` does is that each time, getopts will check the next given
short-form option in the argument list `$*` or `$@`, and check if it matches any
character in the option string `abc`, if it does, it will assign the matched
option to the variable `opt`, if it does not match, it will assign `?` to `opt`.

When `getopts` encounters the end of options in the argument list, it returns
false.

`getopts` also will update the built-in variable `OPTIND` to the index of the
next argument to be processed. So, after the `while` loop, you can do
`shift $((OPTIND - 1))` to discard the options in the argument list. If you want
to use `getopts` again, you need to explicitly `unset $OPTIND`.

#### OPTARG and Silent Reporting Mode

By default, in case of an illegal option (i.e. when an option does not match any
given option in the option string), `getopts` will output an error message in
the format of `"myscript: illegal option -- x"`.

But you can activate _silent reporting mode_ by adding a colon `:` to the
beginning of the option string, like `:abc`. In silent mode, `getopts` will not
output any error message, and will assign the `?` to `$opt` and the unmatched
option to `$OPTARG`.

#### OPTARG and Argument Options

If a character in the option string is followed by a colon, `:`, it means that
the option requires an argument (whatever is the next positional parameter). In
this case, `getopts` will assign the provided argument of the option to
`$OPTARG`.

If an option that requires an argument is not followed by an argument, `getopts`
will assign `?` to `$opt` and unset `$OPTARG`. Except in silent mode, `getopts`
will assign `:` to `$opt` and the option to `$OPTARG`.

NOTE that in the following first four cases, `getopts` will interpret the option
argument as present:

```bash
# in myscript
while getopts 'a:bc' opt; do
    case "$opt" in
        a)
            echo "Option a with argument $OPTARG" ;;
        b)
            echo "Option b" ;;
        c)
            echo "Option c" ;;
        \?)
            exit 1 ;;
    esac
done

# in terminal
myscript -a -b # output: Option a with argument -b
myscript -a -b -c # output: Option a with argument -b \n Option c
myscript -ab # output: Option a with argument b
myscript -abc # output: Option a with argument bc
myscript -ba # output: Option b \n myscript: option requires an argument -- a
```

### Misc

- `read` read a line from standard input
  - e.g. `read -p "Enter your name: " name` store input in variable `name`
  - `-p {prompt}` prompt
  - `-r` raw input, no backslash escapes
- `xargs` build and execute command lines from standard input
- `mapfile` read lines from standard input into an array. See
  [more](../bash.md#mapfile)

### Running Scripts

- `source` or `.`: beware of `exit` in the script, it exits the current shell
- `bash`
  - `-x` print commands and their arguments as they are executed. For debugging
- Use shebang and make the script executable

## Users

- `sudo` run command as superuser

  - `-i` run shell as superuser
  - `sudo su --login` (`-l`) run a login shell as superuser using the root's
    environment variables
  - `-e` edit files as superuser

- See [User Administration](user-administration.md).

### List Groups

- `cat /etc/group` - file containing group information
- `getent group [groupname]` - list all groups, or list info of a specific group

### Permissions

- `chmod`
  - For octal mode, there are four octal digits:
    - 1st digit: (4) set user ID, (2) set group ID, (1) sticky bit
    - 2nd digit: owner's permission - (4) read, (2) write, (1) execute
    - 3rd digit: group's permission
    - 4th digit: other's permission
  - `chmod ug+x` add execute permission for user and group
  - `chmod +t` set sticky bit
  - `chmod u+s` set user ID
  - `chmod g+s` set group ID

## File System

- `ls`
  - `-r` reverse
  - `-t` sort by modification time
  - `-h` human-readable size
  - `-S` sort by size
  - `-i` print inode number
  - `-A` list all except `.` and `..`
  - `-R` recursive
- `tree` show directory tree
- `du` - estimate file space usage (not the actual size, but disk space reserved
  for the file, which depend on the block size of the file system)
  - `-h` human-readable size
  - `-s` summarize
  - `-d 1` depth
- `df` - report file system disk space usage
  - `-h` human-readable size
  - `-T` show file system type
  - `-i` show inode information
- `stat` display file or filesystem status
  - `-c {format}` specify output format
    - `%i` inode number
- `quota` display disk usage and limits for a user (`-v`)

### find program

`find {where} {what_to_find} {action}`

#### GATCHA with find

##### Symbolic links

If you provide a symbolic link in `{where}`, `find` will not follow it by
default. To follow symbolic links, use `-L` option, but that will also make
`find` to follow symbolic links in the path of the search.

##### Inconsistent output

Note the output of running `find ...` and the output stored in a variable from
`myvar="$(find ...)"` can be different if the output contains weird file names.

The safest way to loop through the output of `find` is:

```bash
while IFS= read -r -d $'\0' file; do
  # do something with $file
done < <(find ... -print0)
```

- `IFS=` prevents leading/trailing whitespace from being trimmed
  - No need to restore `IFS` later as it is only changed for the `read` command
- `-r` prevents backslash escapes from being interpreted
- `-d $'\0'` sets the delimiter to null byte
- `<` input redirection
- `<( ... )` process substitution, takes the output of a command as virtual file
- `-print0` print path with null byte as delimiter

#### actions

- `-print` print path (default action)
- `-print0` print path with null byte, instead of newline
- `-exec command {} ;` execute command for each found files
  - `{}` is a placeholder for the found file (you may need to escape `{}`)
  - `;` is the terminator (you may need to escape `;`)
  - command is executed at the start directory of find command
- `-exec command {} +` like `-exec` but all found files are passed to command at
  once, instead of executing command for each found file
- `-ok command {} ;` like `-exec` but prompt before executing command
  - There is **no** `-ok command {} +` variant
- `-execdir command {} ;` like `-exec` but execute command in the directory of
  the found file
- `-okdir command {} +` to `-execdir` is like `-ok` to `-exec`
- `-delete` delete found files, implies `-depth`

#### filters

- `-name` file name, can use globbing
- `-iname` case insensitive file name, can use globbing
- `-regex` regex pattern for matching the **whole** relative path
- `-type`: `-f` for regular file; `-d` for directory; `-l` for symbolic link
- `-size +100M` larger than 100MB (MB is different from MiB)
- `-atime +7` accessed more than 7 \* 24 hours (7 days) ago
  - `-atime -1` accessed within the last 24 hours
- `-mtime +7` modified more than 7 days ago
- `-empty` empty files
- `-mindepth 2` at least 2 levels deep
- `-maxdepth 2` at most 2 levels deep (1 level is current directory)
- `-depth` process directory's contents before the directory itself
- `-prune` do not descend into directory, does not make sense to use with
  `-depth`

You can logically combine filters with `( expr )`, `! expr`, `-a`, `-o`, etc

#### examples of find command

- `find path/to/dir -size -100c -exec wc {} \;` find files smaller than 100
  bytes
- `find ~ -empty -ok rm {} \;` prompt before removing empty files
- `find . ! -name '*.html'`

## File Name

- `basename`
- `dirname`
- `realpath`
- `readlink -f` resolve symbolic link to actual absolute path

## File Comparison

- `cmp` compare byte by byte and complains (non-zero exit) if two files differ
- `diff` show difference
  - `-y` side-by-side comparison
- `comm` compare sorted files line by line. 1st column: unique to file 1, 2nd
  column: unique to file 2, 3rd column: common to both files

## Data Wrangling

- `tee`
- `sort` - default is `-d` dictionary order, which depends on current Locale
  - `-d` dictionary order. use `env LC_ALL=C sort` for traditional sort order
  - `-n` sort numerically
  - `-f` case insensitive (fold lower case to upper case)
  - `-b` ignore leading blanks
  - `-t ":" -k 2` sort by 2nd field using `:` as delimiter
  - `-u` sort and remove duplicates (unique)
  - `-h` by human-readable size
  - `-r` reverse
  - `-R` randomize
- `uniq` - report or omit repeated lines
  - `-c` count
  - `-i` case insensitive
  - `-d` only repeated lines
  - `-u` only unique lines
- `head`
  - `-n` number of lines (`-n2` is same as `-2`. `-n-2`: up to 2nd last line)
  - `-c` number of bytes
- `tail`
  - `-n` number of lines (`-n2` is the same as `-2`. `-n+2`: from 2nd line)
  - `-f` follow, meaning output appended data as file grows
  - `-c` number of bytes
- `wc`
  - `-l` number of lines
  - `-c` number of bytes
  - `-m` number of characters
  - `-w` number of words
- `cut` - print selected parts of lines (e.g. columns)
  - `-d` delimiter, default is tab
  - `-c` character; `-c 1-3` 1st to 3rd char; `-c 2-` from 2nd char onwards;
    `-c 2,7` 2nd and 7th char
  - `-f` field; `-f 1-3` 1st to 3rd field; `-f 2-` from 2nd field onwards
- `tr set1 [set2]` - translate or delete characters
  - `tr 'a-z' 'A-Z'` convert lowercase to uppercase
  - `tr -d '0-9'` delete digits
- `sed` - stream editor for filtering and transforming text
- `awk` - pattern scanning and processing language
- `grep`
  - `-r` recursive into subdirectories
  - `-n` show line numbers
  - `-c` count
  - `-l` only list file names
  - `-w` match whole word
  - `-i` case insensitive
  - `-I` ignore binary files
  - `-v` invert match
  - `-q` quite mode (exit 0 if at least one match)
  - `egrep` equals `grep -E` extended regex (`|`, `()`, `?`, `+`, `{}`)
  - `fgrep` equals `grep -F` fixed string (no regex)
  - GETCHA: be ware of passing a regex starting with `-`, it would be considered
    an option. To prevent that use `-e {pattern}`
- `paste` - join lines consisting of the sequentially corresponding lines from
  each file. I.e. joining line n from file 1 with line n from file 2.
  - `paste [opt]... [file]...`
    - If no file, or `-` is the file, it is read from stdin. Multiple `-` means
      reading from multiple line from stdin at a time and join them into single
      line. So `paste - - -` will join every 3 lines from stdin into a single
      line
  - `-d <delimiter>` delimiter (default is tab)
  - `-s` paste one file at a time instead of in parallel.
    - Use this to join lines from one file, or from stdin, into a single line

### sed

[Sed - An Introduction and Tutorial](https://www.grymoire.com/Unix/Sed.html)

#### Hold space and pattern space

Other than the `s` command for substitution, you can use

- `h` / `H` for copying / appending pattern space to hold space
- `g` / `G` for copying / appending hold space to pattern space
- `x` for exchanging hold space and pattern space

#### Options

- `-r` (GNU) or `-E` (POXIS) extended regex
- `-e {script}` add the script to the commands to be executed
  - Multiple `-e` can be used for running a series of scripts

#### Substitution flags

- `I` case insensitive
- `g` global, replace all occurrences on a line
- `2` replace the second occurrence
- `p` print the line (after substitution if there is one)
- `w {file}` write the line to file, e.g. `sed -n '/^2024-.*/&/w year2024'`
  - Has to be the **last** flag

### Data Formatting

- `expand` - convert tabs to spaces
- `tr` - translate or delete characters
  - `tr '[a-z] [A-Z] < file.txt'` convert lowercase to uppercase
  - `tr -s ' '` squeeze multiple consecutive spaces into one

## Command Augmentation

- `sleep`
- `timeout`
- `xargs` - build and execute command lines from standard input
  - `-I %` defines `%` as a placeholder for the input (e.g. `xargs -I % echo %`)

## Calculations

- `bc`
  - `echo "scale=2; 5/3" | bc` scale defines number of decimal point (it is
    **truncate** not **rounding**)
    - Default scale is 0.
  - `-s` standard
  - If you want to round to 4 d.p., for example, do `scale=5`, and pipe to
    `printf "%.4f\n"`. Scale 5 because the 5th digit is for rounding to 4th d.p.
- `expr`
- `$[expression]` or `$((expression))`

See also [bash - ways to do calculations](../bash.md#ways-to-do-calculations).

## 🧭 Navigation

- [🔼 Back to top](#unix-commands)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)

```

```
