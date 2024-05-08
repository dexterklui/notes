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

## Compound Statements

```bash
if [[ "${my_num}" -gt 10 ]]; then
  # do something
fi

while [[ "${my_count}" -gt 0 ]]; do
  # do something
done
```

## Here Document (Multi-line Input)

You often see people use `cat <<EOF` to handle multi-line input. It uses a
feature call **_Here Documents_** which can be found in `man bash`.

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

## 🧭 Navigation

- [🔖 Parent index](../index.md)
- [📑 Notes Index](../index.md)
