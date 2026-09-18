# Text-Fu - Command Cheat Sheet

A practical reference for the commands and Bash features covered in the Text-Fu section.

---

## 1. Standard Output - `stdout`

### Redirect output to a file

```bash
command > file.txt
```

Creates the file if it does not exist and replaces its contents if it does.

### Append output to a file

```bash
command >> file.txt
```

Adds output to the end of the file.

### Example

```bash
echo "Hello" > output.txt
echo "World" >> output.txt
```

---

## 2. Standard Input - `stdin`

### Read input from a file

```bash
command < input.txt
```

The contents of the file become the command's standard input.

### Example

```bash
sort < names.txt
```

---

## 3. Standard Error - `stderr`

Standard error uses file descriptor `2`.

### Redirect errors to a file

```bash
command 2> errors.txt
```

### Append errors to a file

```bash
command 2>> errors.txt
```

### Redirect stdout and stderr together

```bash
command > output.txt 2>&1
```

A common Bash shorthand is:

```bash
command &> output.txt
```

---

## 4. Pipe - `|`

Sends the standard output of one command to the standard input of another.

```bash
command1 | command2
```

### Example

```bash
ls | sort
```

Multiple commands can be connected:

```bash
cat file.txt | sort | uniq
```

---

## 5. `tee`

Displays input and writes it to a file at the same time.

```bash
command | tee output.txt
```

### Append instead of overwrite

```bash
command | tee -a output.txt
```

### Example

```bash
echo "Hello" | tee output.txt
```

---

## 6. Environment Variables

### Display environment variables

```bash
env
```

### Display a specific variable

```bash
echo "$HOME"
```

### Set a shell variable

```bash
NAME="Linux"
```

### Export a variable

```bash
export NAME="Linux"
```

An exported variable is available to processes started from the current shell.

### Temporary environment variable

```bash
NAME="Linux" command
```

The variable is set for that command without permanently changing the current shell environment.

---

## 7. `cut`

Extracts characters or fields from lines.

### Select characters

```bash
cut -c 1-5 file.txt
```

### Select a field

```bash
cut -d ':' -f 1 file.txt
```

* `-c` — characters
* `-d` — delimiter
* `-f` — fields

### Example

```bash
echo "user:1000:home" | cut -d ':' -f 1
```

---

## 8. `paste`

Combines corresponding lines from files.

### Combine files

```bash
paste file1.txt file2.txt
```

### Use a custom delimiter

```bash
paste -d ',' file1.txt file2.txt
```

### Example

```bash
paste -d ':' names.txt ids.txt
```

---

## 9. `head`

Displays the beginning of a file or input.

### First 10 lines

```bash
head file.txt
```

### Specific number of lines

```bash
head -n 5 file.txt
```

### Specific number of bytes

```bash
head -c 20 file.txt
```

---

## 10. `tail`

Displays the end of a file or input.

### Last 10 lines

```bash
tail file.txt
```

### Specific number of lines

```bash
tail -n 5 file.txt
```

### Follow a changing file

```bash
tail -f logfile.txt
```

This is useful for monitoring log files as new content is added.

---

## 11. `expand` and `unexpand`

### Convert tabs to spaces

```bash
expand file.txt
```

### Convert tabs using a specific tab width

```bash
expand -t 4 file.txt
```

### Convert spaces to tabs

```bash
unexpand file.txt
```

### Example

```bash
expand input.txt > expanded.txt
```

---

## 12. `join` and `split`

### `join`

Combines lines from two files based on a common field.

The input files should normally be sorted by the join field.

```bash
join file1.txt file2.txt
```

### Example

```bash
join users.txt departments.txt
```

### `split`

Splits a file into smaller files.

```bash
split file.txt
```

### Split by number of lines

```bash
split -l 100 file.txt part_
```

This creates files with approximately 100 lines each.

---

## 13. `sort`

Sorts lines of text.

### Alphabetical order

```bash
sort file.txt
```

### Reverse order

```bash
sort -r file.txt
```

### Numeric sorting

```bash
sort -n numbers.txt
```

### Sort by a field

```bash
sort -k2 file.txt
```

### Numeric sorting by a field

```bash
sort -k2n file.txt
```

---

## 14. `tr`

Translates or modifies characters from standard input.

### Replace characters

```bash
echo "hello" | tr 'a-z' 'A-Z'
```

### Delete characters

```bash
echo "hello123" | tr -d '0-9'
```

### Compress repeated characters

```bash
echo "hello     world" | tr -s ' '
```

Common options:

* `-d` — delete characters
* `-s` — squeeze repeated characters

---

## 15. `uniq`

Processes adjacent duplicate lines.

### Remove adjacent duplicates

```bash
uniq file.txt
```

### Count occurrences

```bash
uniq -c file.txt
```

### Show only repeated lines

```bash
uniq -d file.txt
```

### Show only unique lines

```bash
uniq -u file.txt
```

For reliable duplicate counting across an entire file, sort it first:

```bash
sort file.txt | uniq -c
```

---

## 16. `wc` and `nl`

### Count lines

```bash
wc -l file.txt
```

### Count words

```bash
wc -w file.txt
```

### Count bytes

```bash
wc -c file.txt
```

### Count characters

```bash
wc -m file.txt
```

### Display multiple counts

```bash
wc file.txt
```

### Number lines

```bash
nl file.txt
```

---

## 17. `grep`

Searches input for matching lines.

### Search for a string

```bash
grep "error" file.txt
```

### Case-insensitive search

```bash
grep -i "error" file.txt
```

### Show line numbers

```bash
grep -n "error" file.txt
```

### Invert the match

```bash
grep -v "error" file.txt
```

### Search recursively

```bash
grep -r "error" directory/
```

### Use a regular expression

```bash
grep -E "error|warning" file.txt
```

### Example

```bash
grep -n "failed" logfile.txt
```

---

## Quick Reference

| Command    | Purpose                             |                   |
| ---------- | ----------------------------------- | ----------------- |
| `>`        | Redirect stdout and overwrite       |                   |
| `>>`       | Redirect stdout and append          |                   |
| `<`        | Redirect stdin                      |                   |
| `2>`       | Redirect stderr                     |                   |
| `2>&1`     | Redirect stderr to stdout           |                   |
| `          | `                                   | Create a pipeline |
| `tee`      | Save and pass input                 |                   |
| `env`      | Display environment variables       |                   |
| `export`   | Export a variable                   |                   |
| `cut`      | Extract characters or fields        |                   |
| `paste`    | Combine corresponding lines         |                   |
| `head`     | Show beginning of input             |                   |
| `tail`     | Show end of input                   |                   |
| `expand`   | Convert tabs to spaces              |                   |
| `unexpand` | Convert spaces to tabs              |                   |
| `join`     | Join files by a common field        |                   |
| `split`    | Split a file into parts             |                   |
| `sort`     | Sort lines                          |                   |
| `tr`       | Translate/delete/squeeze characters |                   |
| `uniq`     | Process adjacent duplicate lines    |                   |
| `wc`       | Count lines/words/bytes/characters  |                   |
| `nl`       | Number lines                        |                   |
| `grep`     | Search for matching lines           |                   |

---

## Useful Pipelines

### Search and sort

```bash
grep "error" logfile.txt | sort
```

### Count matching lines

```bash
grep "error" logfile.txt | wc -l
```

### Count unique values

```bash
sort file.txt | uniq -c
```

### Extract, sort, and remove duplicates

```bash
cut -d ':' -f 1 file.txt | sort | uniq
```

### Save pipeline output

```bash
cat file.txt | grep "error" | tee errors.txt
```

