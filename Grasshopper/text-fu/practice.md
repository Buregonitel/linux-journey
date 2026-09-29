# Text-Fu - Practice

Hands-on notes and exercises from the Linux Journey / LabEx Text-Fu section.

---

## 1. stdout - Standard Output

### Goal

Understand how standard output is displayed in the terminal and how to redirect it to a file.

### Practice

```bash
echo "Hello Linux"
```

Redirect output to a file:

```bash
echo "Hello Linux" > output.txt
```

Append additional content:

```bash
echo "Text-Fu" >> output.txt
```

Check the result:

```bash
cat output.txt
```

### Result

The command output can be redirected from the terminal into a file.

### What I learned

`>` overwrites the destination file, while `>>` appends to it.

---

## 2. stdin - Standard Input

### Goal

Understand how commands receive input from the terminal or from a file.

### Practice

```bash
sort < names.txt
```

### Result

The command receives the contents of `names.txt` through standard input.

### What I learned

The `<` operator connects a file to a command's standard input.

---

## 3. stderr - Standard Error

### Goal

Learn how error messages can be redirected separately from normal output.

### Practice

```bash
command 2> errors.txt
```

Redirect both output streams:

```bash
command > output.txt 2>&1
```

### Result

Standard error can be stored separately from standard output.

### What I learned

`stderr` uses file descriptor `2`, while `stdout` uses file descriptor `1`.

---

## 4. Pipe and `tee`

### Goal

Understand how commands can be connected and how `tee` can save intermediate output.

### Practice

```bash
ls | sort
```

Use several commands in a pipeline:

```bash
cat file.txt | sort | uniq
```

Save output while continuing the pipeline:

```bash
cat file.txt | tee copy.txt | sort
```

### Result

The output of one command becomes the input of the next command.

### What I learned

The pipe operator allows small commands to be combined into larger workflows.

`tee` is useful when I want to save output without stopping it from continuing through the pipeline.

---

## 5. Environment

### Goal

Understand environment variables and how Bash exposes them to processes.

### Practice

Display environment variables:

```bash
env
```

Inspect a variable:

```bash
echo "$HOME"
```

Create and export a variable:

```bash
MY_VAR="Linux"
export MY_VAR
```

Check it:

```bash
echo "$MY_VAR"
```

Temporarily define a variable for a command:

```bash
MY_VAR="temporary" env | grep MY_VAR
```

### Result

Environment variables can be created, exported, inspected, and temporarily overridden.

### What I learned

Exported variables are inherited by processes started from the current shell.

---

## 6. `cut`

### Goal

Extract selected characters or fields from text.

### Practice

Select characters:

```bash
cut -c 1-5 file.txt
```

Select a field using a delimiter:

```bash
cut -d ':' -f 1 file.txt
```

### Result

Only the requested part of each line is displayed.

### What I learned

`cut` is useful for extracting structured information from text.

---

## 7. `paste`

### Goal

Combine corresponding lines from multiple files.

### Practice

```bash
paste file1.txt file2.txt
```

Use a custom delimiter:

```bash
paste -d ',' file1.txt file2.txt
```

### Result

Lines from the input files are combined horizontally.

### What I learned

`paste` is useful when information stored in separate files needs to be combined line by line.

---

## 8. `head`

### Goal

Inspect the beginning of a file without displaying the entire contents.

### Practice

```bash
head file.txt
```

Display a specific number of lines:

```bash
head -n 5 file.txt
```

Display a specific number of bytes:

```bash
head -c 20 file.txt
```

### Result

Only the beginning of the input is displayed.

### What I learned

`head` is useful for quickly inspecting large files.

---

## 9. `tail`

### Goal

Inspect the end of a file and monitor files as new content is added.

### Practice

```bash
tail file.txt
```

Display a specific number of lines:

```bash
tail -n 5 file.txt
```

Follow a file:

```bash
tail -f logfile.txt
```

### Result

The final part of a file can be inspected, and `tail -f` can continuously display new content.

### What I learned

`tail -f` is especially useful for monitoring log files.

---

## 10. `expand` and `unexpand`

### Goal

Understand the conversion between tabs and spaces.

### Practice

Convert tabs to spaces:

```bash
expand file.txt
```

Use a custom tab width:

```bash
expand -t 4 file.txt
```

Convert spaces to tabs:

```bash
unexpand file.txt
```

### Result

Tab characters and spaces can be converted according to tab-stop positions.

### What I learned

Tab formatting depends on tab positions, so converting between tabs and spaces can affect text alignment.

---

## 11. `join` and `split`

### Goal

Learn how to join related data from sorted files and split large files into smaller parts.

### Practice

Join two files:

```bash
join file1.txt file2.txt
```

Split a file by line count:

```bash
split -l 100 file.txt part_
```

### Result

Related records can be combined using `join`, while large files can be divided into smaller files using `split`.

### What I learned

`join` is useful for combining structured data, while `split` is useful for dividing large files into manageable pieces.

---

## 12. `sort`

### Goal

Sort text alphabetically, numerically, or according to a selected field.

### Practice

Alphabetical sorting:

```bash
sort file.txt
```

Reverse sorting:

```bash
sort -r file.txt
```

Numeric sorting:

```bash
sort -n numbers.txt
```

Sort by the second field:

```bash
sort -k2 file.txt
```

### Result

Text can be ordered according to different sorting criteria.

### What I learned

The appropriate `sort` options depend on the structure and type of data being processed.

---

## 13. `tr`

### Goal

Transform, delete, or compress characters in standard input.

### Practice

Convert lowercase characters to uppercase:

```bash
echo "hello" | tr 'a-z' 'A-Z'
```

Delete digits:

```bash
echo "user123" | tr -d '0-9'
```

Compress repeated spaces:

```bash
echo "hello     world" | tr -s ' '
```

### Result

Characters can be translated, removed, or compressed directly in a pipeline.

### What I learned

`tr` is useful for simple character-level transformations.

---

## 14. `uniq`

### Goal

Identify and process adjacent duplicate lines.

### Practice

Remove adjacent duplicates:

```bash
uniq file.txt
```

Count repeated lines:

```bash
uniq -c file.txt
```

Show only duplicated lines:

```bash
uniq -d file.txt
```

For duplicate counting across unsorted input:

```bash
sort file.txt | uniq -c
```

### Result

Repeated adjacent lines can be collapsed, counted, or filtered.

### What I learned

`uniq` operates on adjacent equal lines, so sorting the input first is often necessary when duplicates can occur in different parts of a file.

---

## 15. `wc` and `nl`

### Goal

Count text and number lines.

### Practice

Count lines:

```bash
wc -l file.txt
```

Count words:

```bash
wc -w file.txt
```

Count bytes:

```bash
wc -c file.txt
```

Count characters:

```bash
wc -m file.txt
```

Number lines:

```bash
nl file.txt
```

### Result

The size and structure of text can be measured using different counters.

### What I learned

`wc` provides several useful ways to measure text, while `nl` makes line-based inspection easier.

---

## 16. `grep`

### Goal

Search text for fixed strings or regular-expression patterns.

### Practice

Search for a string:

```bash
grep "error" file.txt
```

Ignore case:

```bash
grep -i "error" file.txt
```

Show line numbers:

```bash
grep -n "error" file.txt
```

Show lines that do not match:

```bash
grep -v "error" file.txt
```

Search recursively:

```bash
grep -r "error" directory/
```

Use an extended regular expression:

```bash
grep -E "error|warning" file.txt
```

### Result

`grep` can select matching lines from files or command output.

### What I learned

`grep` is one of the most useful tools for filtering text and searching through files.

---

# Mini Exercise

## Goal

Combine several Text-Fu commands into one practical pipeline.

## Practice

Create a small text file:

```bash
printf "error\nwarning\nerror\ninfo\nwarning\nerror\n" > messages.txt
```

Search for relevant messages:

```bash
grep -E "error|warning" messages.txt
```

Sort them:

```bash
grep -E "error|warning" messages.txt | sort
```

Count occurrences:

```bash
grep -E "error|warning" messages.txt | sort | uniq -c
```

Save the result while displaying it:

```bash
grep -E "error|warning" messages.txt | sort | uniq -c | tee summary.txt
```

Check the saved result:

```bash
cat summary.txt
```

Count the final lines:

```bash
wc -l summary.txt
```

## Result

The workflow searches the input, filters the required lines, sorts them, counts duplicates, saves the result, and measures the resulting file.

## What I learned

The main skill from Text-Fu is not memorizing individual commands, but combining them into useful pipelines.

A command can perform one small operation, while a pipeline can turn several simple operations into a complete text-processing workflow.

