# Shell Scripting

File should start with `#! /bin/sh -` on the first line, or something equivalent that points out which shell to run the script in. The dash stops some types of spoofing attacks.

## Executing a shell script

`--` (double dash) signals the end of options to the shell and should be treated as arguments to the script.

## Syntax

### Comments

`# Everything after the hash is a comment`

### Arguments

`$1` is the first argument and arguments above 9 need to be enclosed in brackets `${10}`.

$ `command arg_one arg_two`

### General

`;` separates two lines or commands.

`&` runs preceding command in the background.

### Variables

```
var_name=var_value
echo $var_name

var_one="Var One"
var_two="Var Two"
var_three="$var_one, $var_two"
```

### I/O

`command < file.txt` uses contents of file.txt as input for command.

`command > file.txt` puts output of command into file.txt creating the file if it doesn't exist.

`command >> file.txt` appends the output to file.txt and creates the file if it doesn't exist.

`command1 | command2` uses output of command1 as input to command2.

`command > /dev/null` discards output. Useful when only the exit code from the command is interesting.

```
stty -echo
read password < /dev/tty
stty echo
```

`stty` controls terminal settings, `-echo` means do not print what is typed, `echo` resets. `/dev/tty` is the terminal and the above line reads the input from the terminal into the `password` variable.

### Troubleshooting

#### Tracing

```
set -x # starts tracing

echo "Lines after tracing has started are being traced."

set +x # stops tracing
```

## Resources

* Classic Shell Scripting, Arnold Robbins and Nelson H. F. Beebe, O'Reilly 2005
