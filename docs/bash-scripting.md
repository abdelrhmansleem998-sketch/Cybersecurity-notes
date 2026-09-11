# Bash scripting

Quick syntax reference for scripting during engagements.

## Basics

- `chmod +x file.sh` makes a script executable.
- `#!/bin/bash` is the shebang — it tells the shell which interpreter to use.
- `echo` prints to the screen.
- `variable_name=value` defines a variable — no spaces around the `=`.
- `unset var_name` deletes a variable.
- `var_name=$(date)` or `` var_name=`date` `` captures a command's output into a variable.
- `$0` is the script name, `$1`/`$2` are positional arguments, `$#` is the argument count, `$@` is the full argument list.

## Comparisons

| Operator | Meaning |
|---|---|
| `-eq` / `==` | equal |
| `-ne` / `!=` | not equal |
| `-gt` | greater than |
| `-ge` | greater than or equal |
| `-lt` | less than |
| `-le` | less than or equal |
| `&&` / `-a` | AND |
| `\|\|` / `-o` | OR |

## Control structures

```bash
if [ <test> ]; then
    <commands>
elif [ <test> ]; then
    <commands>
else
    <commands>
fi
```

```bash
for i in 1 2 3; do echo "$i"; done
for i in $(seq 1 10); do echo "$i"; done
for i in {1..10}; do echo "$i"; done
```

```bash
n=1
while [ $n -lt 10 ]; do
    echo "Hello $n"
    ((n++))
done
```

## Functions, arrays, and redirection

```bash
function_name() { <commands>; }
function_name  # call it
```

Arrays:

```bash
arr=("a" "b" "c")
echo ${arr[0]}    # first element
echo ${#arr[@]}   # element count
```

`echo $?` returns the exit code of the last command — 0 means success.

Redirection:

```bash
echo "Hello" > file.txt    # overwrite
echo "Hello" >> file.txt   # append
cat < file.txt              # read as input
```
