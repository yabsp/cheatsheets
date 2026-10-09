# Regex Cheat Sheet

Regular expressions as used by GNU `grep` on Linux. The tables use the
extended syntax of `grep -E`. Put patterns in single quotes so the shell does
not change them: `grep -E 'a|b' file`.

!!! note "Version"
    GNU grep 3.11 and 3.12. `sed -E` and `awk` use the same extended syntax.

## Modes

| Command | Description |
| --- | --- |
| `grep 'text'` | Basic: `+ ? | ( ) { }` need a backslash to be special |
| `grep -E 'text'` | Extended: everything below works as written |
| `grep -P 'text'` | Perl compatible: adds `\d`, lazy matching and lookarounds |
| `grep -F 'text'` | Fixed string, no regex at all, e.g. for `a.b` |

| Extended (`grep -E`) | Basic (`grep`) |
| --- | --- |
| `a+` | `a\+` |
| `a?` | `a\?` |
| `a{2,5}` | `a\{2,5\}` |
| `(ab)` | `\(ab\)` |
| `a|b` | `a\|b` |

## Characters

| Pattern | Description |
| --- | --- |
| `.` | Any character |
| `[abc]` | One of `a`, `b` or `c` |
| `[^abc]` | Any character except `a`, `b` or `c` |
| `[a-z0-9]` | One character from the ranges |
| `\w` / `\W` | Word character `[A-Za-z0-9_]` / anything else |
| `\s` / `\S` | Whitespace / anything else |
| `[[:digit:]]` | Digit, same as `[0-9]` |
| `[[:alpha:]]` / `[[:alnum:]]` | Letter / letter or digit |
| `[[:upper:]]` / `[[:lower:]]` | Uppercase / lowercase letter |
| `[[:space:]]` / `[[:punct:]]` | Whitespace / punctuation |
| `\.` | A literal dot, escape `. * [ ] ^ $ \ + ? ( ) { } |` the same way |

`\d` does not work in `grep` or `grep -E`, it matches a plain `d`. Use
`[0-9]` or `grep -P`.

## Anchors

| Pattern | Description |
| --- | --- |
| `^` | Start of the line |
| `$` | End of the line |
| `\<` / `\>` | Start / end of a word |
| `\b` / `\B` | Word boundary / not a word boundary |

## Quantifiers

| Pattern | Description |
| --- | --- |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 |
| `{3}` | Exactly 3 |
| `{2,}` | 2 or more |
| `{2,5}` | 2 to 5 |

Quantifiers are greedy: `<.+>` matches all of `<a><b>`. For the shortest
match use `<.+?>` with `grep -P`.

## Groups

| Pattern | Description |
| --- | --- |
| `(ab)+` | Group, here `ab` repeated |
| `cat|dog` | Either `cat` or `dog` |
| `(cat|dog)s` | Alternatives inside a group |
| `(ab)\1` | `\1` repeats what group 1 matched, here `abab` |

## Perl Only (grep -P)

| Pattern | Description |
| --- | --- |
| `\d` / `\D` | Digit / not a digit |
| `+?` / `*?` | Lazy, match as little as possible |
| `(?i)` | Ignore case for the rest of the pattern |
| `(?:ab)` | Group without capturing |
| `(?=EUR)` / `(?!EUR)` | Followed / not followed by `EUR` |
| `(?<=price: )` / `(?<!-)` | Preceded / not preceded by |
| `\K` | Drop everything matched so far from the output |

## grep Options

| Option | Description |
| --- | --- |
| `-i` | Ignore case |
| `-w` | Match whole words only |
| `-x` | Match whole lines only |
| `-v` | Show lines that do not match |
| `-o` | Show only the matching part |
| `-c` | Count matching lines |
| `-n` | Show line numbers |
| `-r` | Search all files in a folder |
| `-l` | Show only file names |
| `-A 3` / `-B 3` / `-C 3` | Show 3 lines after / before / around each match |

## Examples

| Command | Description |
| --- | --- |
| `grep -Ev '^\s*(#|$)' <file>` | Config without comments and empty lines |
| `grep -Ei 'error|warn' <log>` | Lines with errors or warnings |
| `grep -E '^[0-9]{4}-[0-9]{2}-[0-9]{2}' <file>` | Lines starting with a date |
| `grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' <file>` | Extract IPv4 addresses |
| `grep -oP 'version=\K[0-9.]+' <file>` | Extract the value after `version=` |
| `grep -oP '\d+(?= EUR)' <file>` | Extract numbers followed by `EUR` |
| `grep -rnw 'TODO' <dir>` | Find `TODO` in all files with line numbers |

## Other Tools

| Tool | Notes |
| --- | --- |
| `sed -E 's/(\w+)-(\w+)/\2-\1/'` | Extended syntax, `\1` in the replacement |
| `awk '/^(cat|dog)$/'` | Extended syntax |
| `find . -regextype posix-extended -regex '.*\.(jpg|png)'` | Must match the whole path |
| `rg` | ripgrep: supports `\d` and lazy matching, lookarounds need `rg -P` |
| Vim | Own syntax, `\v` makes it close to extended, see the [Vim cheat sheet](vim.md#search) |
