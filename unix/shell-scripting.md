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

* `$#` is count of arguments passed
* `$*` and `$@` are the arguments as a single string or separate strings
* `$-` shell options
* `$?` exit status of previous command
* `$$` shell process id
* `$0` name of shell script
* `$!` process id of last background command
* `ENV` pathname to environment variables
* `HOME` path to home directory
* `IFS` internal field separator
* `LINENO` line number that is currently executing
* `PPID` parent process id

It is possible to remove the first argument in the list with the `shift` command making the second argument the first, this makes looping easier.

`$((x += 7))` an arithmetic expression adding 7 to the variable x.

### General

`;` separates two lines or commands.

`&` runs preceding command in the background.

`$(...)` command substitution, runs a command and uses the results of it in place. It is also possible to use backticks \` as delimiters for command substitution but is error prone.

`(...)` creates a subshell and runs commands in a new process, new environment, without changing the parent environment. `{}` creates a code block that runs in the same environment, the closing bracket needs to be on a new line or after a `;`.

#### Some Built-in Commands

`:` does nothing but expansion of arguments.

`.` reads a file and executes its contents.

### Variables

```
var_name=var_value
echo $var_name

var_one="Var One"
var_two="Var Two"
var_three="$var_one, $var_two"

"The expression ${#var_one} returns the length of ${var_one}"
```

#### Substitution Operators
```
${varname:-default} # use varname if exists and not null, otherwise default
${varname:=default] # use varname if exists and not null, otherwise set varname to default and use that
${varname:?message} # use varname if exists and not null, otherwise abort script showing message
${varname:+value} # use value if varname exists and is not null
```

By removing the colon in the above expressions it is possible to test only if `varname` exists using the null value if it exists and is set to null.

#### Pattern matching operators

```
${variable # pattern} # if pattern matches beginning of variable's value, delete shortest possible match and return rest
${variable ## pattern } # same as previous but delete longest possible match
${variable % pattern } # if pattern matches end, delete shortest match and return rest
${variable %% pattern } # same as previous but delete longest possible match
```

### Conditionals

```
if some command or expression
then
    # do something if true
elif some command or expression
then
    # do something if true
else
    # do something if none of the above were true
fi
```

#### Case Statement

```
case $1 in
"woo")
    # do something here
    ;; #stop executing case here
"foo")
    # do something here
    ;;
*) # catch-all
    # do something here
    ;; # optional but good form
esac
```

#### the test Command

```
if test "$val1" = "some string"
then
    # do something if strings equal
fi

if [ "$val1" = "some string" ] # alternative way of writing the above
then ...
```

It's a good practice to quote all arguments to the test function as arguments are required, that way an argument will always be passed albeit null in some circumstances.

### Looping

```
for i in "$@" # can be written 'for i' and '$@' is implied
do
    # do something here
done

while test
do
    # do something here
done
```

There is also an `until` loop which works the same as the `while` loop but executes until the condition returns a success, while a `while` loop executes until condition fails.

### I/O

`command < file.txt` uses contents of file.txt as input for command.

`command > file.txt` puts output of command into file.txt creating the file if it doesn't exist. `set -C` will cause this to fail if file already exists, can be overridden with `>|`.

`command >> file.txt` appends the output to file.txt and creates the file if it doesn't exist.

`command1 | command2` uses output of command1 as input to command2.

`command > /dev/null` discards output. Useful when only the exit code from the command is interesting.

```
cat << EOFILE
Content goes here
It is the input for cat
EOFILE
```

A here document `<< SOME_END_MARKER ...` makes it possible to write an entire document into a script with line breaks and all that would otherwise have special meaning. It is possible to do variable and command substitution within a here document.

Starting a here document with `<<-` (an added minus sign) will ignore tabs at the start of a line.

```
stty -echo
read password < /dev/tty
stty echo
```

`stty` controls terminal settings, `-echo` means do not print what is typed, `echo` resets. `/dev/tty` is the terminal and the above line reads the input from the terminal into the `password` variable.

### Functions

`function_name () { ... }`. Arguments are numbered the same as in parent script $1, $2 ... and the function shares the runstate of the parent program, i.e. variables (and some other things like signals) are global.

### Troubleshooting

#### Tracing

```
set -x # starts tracing

echo "Lines after tracing has started are being traced."

set +x # stops tracing
```

## Resources

* Classic Shell Scripting, Arnold Robbins and Nelson H. F. Beebe, O'Reilly 2005
