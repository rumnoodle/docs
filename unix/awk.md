# awk

`awk` is a text processing tool. It views an input stream as a collection of records where a line is a record that has one or more fields.

An awk command looks roughly like this `awk '/pattern/ { action }' input.sources` where pattern or action are optional and sources are input sources for the command, stdin if omitted. Supplying only pattern will print lines matching it and supplying only an action will perform action on all lines. Input sources can be any number of files or stdin.

## Comments

`# start with hash and continue to the end of the line`

## Command Line Arguments

`ARGC` is the number of command line arguments provided and `ARGV` is a vector of the arguments starting from `ARGV[0]` which is the name of the program (awk.)

## I/O

`/dev/stdin`, `/dev/stdout`, `/dev/stderr` are all available in an awk program.

It's possible to get input from files and terminals with `getline` command. Output can be done with `print` and `printf`. Remember to close all files with `close`.

## BEGIN and END

It is possible to supply BEGIN and END patterns that are executed once at the beginning and at the end respectively.

## Regular Expressions

There are two ways to match regular expressions.

`"string" ~ "regex"` returns true if the regular expression in regex matches string and `"string" !~ "regex"` does the opposite, returns true if it doesn't match. It's possible to use slashes to delimit a regex `/regex/`.

## Conversions

`n = 0 + "123"` turns the string 123 into a number and `s "" + 123` turns the number 123 into a string. When converting to number it converts as much as possible from the string to a number but it has to start with a digit for the number to be anything other than 0.

## Variables

### Fields

Fields are named `S1`, `$2`, `$3` and so on up until the number of fields. `$0` is the entire record. Negative numbers cause a fatal error and numbers beyond the fields in the record returns an empty string.

Fields can be reassigned e.g. `$2 = "new value"`.

### Built-in Variables

* `FILENAME` is the name of the current input file.
* `FNR` is the record number in the current input file.
* `FS` file separator (as a regular expression, defaults to one or more whitespace characters " ".) If it is longer than one character it is treated as a regular expression. The empty string "" makes every character a field or possibly the whole record one field.
* `NF` number of fields in the current record.
* `NR` record number in job
* `OFS` output field separator (defaults to space " ")
* `ORS` output record separator (defaults to newline "\n")
* `RS` input record separator (defaults to newline "\n".) An empty string separates records into paragraphs where paragraphs are separated by at least one blank line and `FS` is newline unless otherwise specified. If it is longer than one character it is treated as a regular expression in some flavours of awk.

### Environment Variables

Environment variables can be accessed through array `ENVIRON`, e.g. `ENVIRON["PATH"]`.

## Patterns

If pattern is a regular expression then match the record to that and if it matches then execute actions on the record.

It is possible to use built-in variables in a pattern expression e.g. `FNR < 10` executes an action on the first 10 lines in every input file.

It is also possible to use field variables e.g. `$1 ~ /Woowoo/ && $2 == "happy"`.

It is possible to define ranges with `start of range, end of range` where start and end of range can be any expression that is pattern matched.

## Actions

### Print

`print $1, $4, $5 # prints multiple fields separated by OFS`. `OFS = "\n" ; $1 = $1 ; print $0`, the reassignment is needed to print all fields on a new line because we need to assign at least one of the fields for the new OFS to take effect.

### Conditionals

```
if (expression)
    # do something
else if (expression)
    # do something
else
    # do something
```

Testing for array membership should be done like this `if ("something" in array)` rather than `array["something"] ...` as the latter creates the array item if it doesn't exist.

### Looping

```
for (k = 1; k <= 10; k++) { ... }

for (i in items)
    # do something with items[i]
```

## Functions

```
function name(arg1, arg2, ...) {
    ...
}
```

Functions can have a return expression.

Variables used within a function are global. Arguments passed are local to the function and it is possible to pass fewer arguments than declared so a way to declare a local variable is to add it to the arguments list.

## Running External Programs

External programs can be run with the `system` command/function.

## Examples

Word count: `awk '{ Characters += length($0) + 1 ; Words += NF } END { print NR, Words, Characters }' filename.file`.

Convert windows newlines to unix newlines: `awk 'BEGIN {RS = "\r\n" } { print }' file.name`.

## Resources

Classic Shell Scripting, Arnold Robbins & Nelson H. F. Beebe, O'Reilly 2005

