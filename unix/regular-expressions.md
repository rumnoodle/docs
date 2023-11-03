# Regular Expressions

## Metacharacters

Not all of these are available for all commands in the shell

* `\` escape character
* `.` match single character
* `*` match 0 or more characters
* `+` match 1 or more characters
* `^`, `$` match from start of line, end of line
* `[abce-g]` matches one character from set a, b, c, e, f, or g
*  `a{2,4}` matches 2 to four consecutive occurrences of a, brackets may require escaping
* `(cat)\1` parens saves pattern and makes it reusable, matches catcat, parens may require escaping
* `?` matches zero or one of the preceding character
* `a|electric` matches either a or electric

### Extensions

These may not be supported by all programs but are fairly common.

* `\w` matches any word-constituent character, alphanumeric character
* `\W` opposite of `\w`
* `\<`, `\>` matches the beginning and end of a word
* `\b` matches a word boundary, similar to `\<` and `\>` combined. `\y` in awk because `\b` matches backspace
* `\B` matches the space between two words

## Posix bracket expressions

They must appear inside a bracket expression [].

* `[:alnum:]` any alphanumeric character
* `[:alpha:]` any alphabetic character
* `[:blank:]` spaces and tabs
* `[:digit:]` any numeric character
* `[:graph:]` any non-whitespace character
* `[:lower:]` any lowercase character
* `[:print:]` any printable character
* `[:punct:]` punctuation character
* `[:space:]` whitespace character
* `[:upper:]` uppercase character
* `[:xdigit:]` hexadecimal digit

Collating element is something that matches a group of characters as if it was one e.g. `[.kj.]` in which kj is treated as one character.

An equivalence class matches on all variants of a character e.g. `[=e=]` matches all types of e characters.

## Extended Regular Expressions

`\` has special meaning inside brackets and backreferences with parentheses doesn't exist, instead they are used for grouping e.g. `(match){3}` matches exactly 3 consecutive instances of the word match. Brackets {} and parentheses () don't need to be escaped.
