---
date: 2023-07-25 (Tue)
---

# Bash

## Alias

You can chain alias.

```bash
alias abc = "echo 'xxx'"
alias def = "echo 'kkk'; abc"
def
## kkk
## xxx
```

## Variables

### Using Variables

```bash
name=alice
echo $name # alice
echo ${#name} # 5 - number of chars
echo ${name:1:3} # "lic": substring slice from 0-based index 2 with length 3
```

### Special Variables

- `$#` number of arguments
- `$0` name of the script
- `$n` n-th argument
- `$$` process ID of the shell
- `$?` exit status of the last command
- `$@` all arguments as separate strings
- `$*` all arguments as one string

#### IFS and Difference between `$*` and `$@`

In `$*`, each argument is separated by the first character in `$IFS` (a builtin
variable which stands for "**_internal field separator_**"). By default it is a
space `" "`, then a tab `"\t"` and lastly a newline `"\n"`. So by default each
argument in `$*` is separated by a space.

In a _for-in-loop_, bash will break the expression after `in` at any character
contained in `$IFS`. So if an argument contains a space, bash will break it into
two parts and assign them in two separate iterations:

```bash
# in file dollarStar
for x in $*; do
  echo $x
done
```

Running `dollarStar 1 "2 3" 4` will output:

```text
1
2
3
4
```

NOTE that double-quoting `$*` in the for-in-loop won't do, because all the
arguments stored in `$*` will be taken as one single string, and hence there
will be only one iteration where `$x` is assigned the whole string.

To prevent bash from breaking up arguments containing spaces, you should use
**double-quoted** `$@`, which will be expended into separate arguments (without
the quotes it will be the same as `$*`):

```bash
# in file dollarAt
for x in "$@"; do
  echo $x
done
```

Now, running `dollarAt 1 "2 3" 4` will output:

```text
1
2 3
4
```

### Array

Define an array:

```bash
names[0]="Alice"
names[1]="Bob"

names=("Alice" "Bob")
```

Using an array:

```bash
echo ${names[0]}
echo ${names[1]}
echo ${names[*]}
echo ${#names[*]}
```

## Compound Statements

### Condition

Conditions are used in `if`, `while`,`until`, `for`. Check `man test` to see
some ways to write condition tests.

For `true` and `false`, you can use a variable like `flag1=true` and
`flag2=false`, and use the variable as the condition:

```bash
myflag=false
# make myflag true in some condition then test for it
if $myflag; then
  # do something
fi
```

### if

```bash
if [[ "${my_num}" -gt 10 ]]; then
  # do something
fi
```

### while

```bash
x=0
while [[ "${my_count}" -gt 0 ]]; do
  # do something
  ((x++)) # increment x by 1
done

```

### until

Like while, but run until the condition is true

```bash
x=0
until [ x -ge 5 ]; do
  echo $x
  ((x++))
done
```

### for

```bash
for x in 1 2 3 4 5; do
  echo $x
done

###

for x in {1..5}; do
  echo $x
done

###

for arg in $@; do
  echo "$arg"
done

###

foo=string
for (( i=0; i<${#foo}; i++ )); do
  echo "${foo:$i:1}"
done

###

for file in $(ls); do
  echo "$file"
done
```

- Infinite `for` loop is created if the list is empty
- Note that the variables declared/used in for loop is **global**, e.g. `i`,
  `arg`, `file`, `x` in the above examples.

### case

```bash
case "$1" in
  -e|--extension)
    EXTENSION="$2"
    shift # past argument
    shift # past value
    ;;
  -*|--*)
    echo "Unknown option $1"
    exit 1
    ;;
  *)
    POSITIONAL_ARGS+=("$1") # save positional arg
    shift # past argument
    ;;
esac

# Dunno if same as what listed in `man tr`
case "$1" in
  [[:digit:]])
    # do something
    ;;
  [a-z])
    # do something
    ;;
  [[:alpha:]])
    # do something
    ;;
  *)
    ;;
esac
```

#### Pattern matching in case

Brace expansion doesn't work, but `*,` `?` and `[]` do. If you set
`shopt -s extglob` then you can also use extended pattern matching:

- `?()` - zero or one occurrences of pattern
- `*()` - zero or more occurrences of pattern
- `+()` - one or more occurrences of pattern
- `@()` - one occurrence of pattern
- `!()` - anything except the pattern

Here's an example:

```bash
shopt -s extglob
for arg in apple be cd meet o mississippi
do
    # call functions based on arguments
    case "$arg" in
        a*             ) foo;;    # matches anything starting with "a"
        b?             ) bar;;    # matches any two-character string starting with "b"
        c[de]          ) baz;;    # matches "cd" or "ce"
        me?(e)t        ) qux;;    # matches "met" or "meet"
        @(a|e|i|o|u)   ) fuzz;;   # matches one vowel
        m+(iss)?(ippi) ) fizz;;   # matches "miss" or "mississippi" or others
        *              ) bazinga;; # catchall, matches anything not matched above
    esac
done
```

### break and continue

`break` and `continue` can optional accept a number to specify how many levels
to break or continue.

## Here Document (Multi-line Input)

You often see people use `cat <<EOF` to handle multi-line input. It uses a
feature call **_Here Documents_** which can be found in `man bash`.

To write multi-line to a file, you can also use `cat > file` and then type
`<Ctrl-D>` to terminate typing.

### Syntax

```text
<<[-]word
  here-document
delimiter
```

`<<` instructs the shell to read input from the current source until a line
containing only the given _word_ (conventionally `EOF`, which is also called a
_Here tag_) with no leading nor trailing blanks is seen.

No parameter expansion, command substitution, arithmetic expansion, or pathname
expansion is performed on the given _word_.

If any characters in the given _word_ are **quoted**, the _delimiter_ is the
result of quote removal on the given _word_, and the lines in the
_here-document_ are **not expanded**.

If the given _word_ is **unquoted**, all lines of the _here-document_ are
subjected to parameter expansion, command substitution, and arithmetic
expansion. In this case, the character sequence `\<newline>` is ignored, and `\`
must be used to escape the characters `\`, `$`, and `` ` ``.

If the redirection operator is `<<-`, then all leading **tab** characters are
stripped from input lines and the line containing _delimiter_. This allows
here-documents within shell scripts to be indented in a natural fashion.

### Assign multi-line string to a shell variable

```bash
sql=$(cat <<EOF
SELECT foo, bar FROM db
WHERE foo='baz'
EOF
)
```

### Pass multi-line string to a file

```bash
cat <<EOF > print.sh
#!/bin/bash
echo \$PWD
echo $PWD
EOF
```

Here, the variable `$PWD` will get expanded, and now `print.sh` will contain:

```bash
#!/bin/bash
echo $PWD
echo /home/user
```

If you want to pass multi-line string to a file with **sudo** privilege, you
can't simply add `sudo` before the `cat` because this will elevate privilege
while executing `cat` but not while writing to the file. Instead, you need to
use `tee`.

```bash
# pass multi-line string to a file with sudo
sudo tee /etc/somepath/file > /dev/null <<EOF
#!/bin/bash
echo \$PWD
echo $PWD
EOF
```

### Pass multi-line string without expansion

```bash
cat <<'EOF' > print.sh
#!/bin/bash
echo \$PWD
echo $PWD
EOF
```

Now `print.sh` will contain:

```bash
#!/bin/bash
echo \$PWD
echo $PWD
```

### Pass multi-line string to a pipe

```bash
cat <<EOF | grep 'b' | tee b.txt
foo
bar
baz
EOF
```

## Reading Lines From a File

See
[Looping Through the Content of a File in Bash - StackOverflow](https://stackoverflow.com/questions/1521462/looping-through-the-content-of-a-file-in-bash)

One way to do it is:

```bash
while read p; do
  echo "$p"
done <peptides.txt
```

This has the side effects of trimming leading whitespace, interpreting backslash
sequences, and skipping the last line if it's missing a terminating linefeed. If
these are concerns, you can do:

```bash
while IFS="" read -r p || [ -n "$p" ]
do
  printf '%s\n' "$p"
done < peptides.txt
```

`[ -n "$p" ]` is needed because `read` return non-zero if EOF is detected, so
this will handle the last line.

Setting **empty** `IFS` is needed to preserve leading and trailing spaces in the
`read` line.

## function

### Define a Function

```bash
function myFunc() {
  # ...
}

# or

myFunc() {
  # ...
}
```

### Special variables in Function

A function is like a modular script/program. `$1` is the first argument passed
to the function, `$#` is the number of arguments passed, etc. But **NOTE** that
`$0` is the **name of the script**, not the function.

It has access to all variables in the script, (except the overridden variables
like `$1`)

### Calling a Function

You just type the function name, like calling a program. `myFunc`. You can pass
arguments to a function as usual.

### Return Value

A function is like a program in Bash. It takes in input from input stream (or
arguments) and output to output stream. So in the context of bash scripting, a
return value is just what is being output, and it is done by command like
`echo`.

```bash
function getName() {
  read -p "Enter your name: " name
  echo $name
}

echo "Your name is $(getName)"
```

### Return Code

Like the exit code of a program, you can `return` a code from a function. It can
be accessed by `$?`.

```bash
function isChar() {
  case $1 in
    [a-zA-Z])
      return 0 ;;
    *)
      return 1 ;;
    esac
}

read -p "Enter a character: " char
if isChar $char; then
  echo "It's a single character"
else
  echo "It's not a single character"
fi
```

### Variables Scope

By default variables defined in a function is **global** to the script. If you
want to define a local variable, use `local`:

```bash
function cube() {
  x=$[ $1 * $1 * $1 ]
  local y=123
}
cube 3
echo "The cube of 3 is $x." # The cube of 3 is 27.
echo "The value of y is $y." # The value of y is .
```

## Calculation

- `echo "2 * 3" | bc -s`

- `$(( 2 * 3 ))` arithmetic expansion

  - respect arithmetic operation precedence
  - Note that cannot quote the tokens, so this doesn't work:
    `num=2; echo $(( "$num" * 3 ))`. Instead, you can do
    `num=2; echo $(( $num * 3 ))`
  - In fact anything `(( ))` is evaluated in the manner of arithmetic operation
    - `(( count++ ))` increments count by 1
    - `(( count+= 1 ))` the same
    - `let "count++"` the same
    - `let "count+=1"` the same

- `$[ 2 * 3 ]`. Like `$(( ))` but deprecated and not POSIX compliant

- `expr 2 \* 3`

- `let num="2+3"`

  - respect arithmetic operation precedence
  - stores the result 5 in `num`
  - If it is not a valid arithmetic expression, it will return an error

To store value in other than base ten:

```bash
let binResult=2#1001
let octResult=8#14
let hexResult=16#FF
```

## Scripting

### Error message

One common style is:

`echo $0: error: {what went wrong}`

## Links and Resources

- [Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/index.html)
  - [pdf version](http://www.tldp.org/LDP/abs/abs-guide.pdf)

## 🧭 Navigation

- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
