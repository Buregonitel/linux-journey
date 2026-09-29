# Text-Fu

## Overview

This section focuses on processing text directly from the Linux command line.

I practiced how Bash handles standard input, standard output, and standard error, how commands can be connected with pipelines, and how common text-processing utilities can be combined to inspect, transform, filter, sort, and count data.

The section contains 16 interactive lessons.

## Lessons Completed

* [x] stdout (Standard Out) Redirect
* [x] stdin (Standard In) Redirect
* [x] stderr (Standard Error) Redirect
* [x] Pipe and `tee`
* [x] Environment
* [x] `cut`
* [x] `paste`
* [x] `head`
* [x] `tail`
* [x] `expand` and `unexpand`
* [x] `join` and `split`
* [x] `sort`
* [x] `tr`
* [x] `uniq`
* [x] `wc` and `nl`
* [x] `grep`

## Skills Practiced

* Redirecting standard output to files
* Reading input from files through standard input
* Separating standard output and standard error
* Combining output streams
* Building command pipelines
* Using `tee` to save and pass data simultaneously
* Working with environment variables
* Selecting characters and fields with `cut`
* Combining files with `paste`
* Inspecting the beginning and end of files
* Following changing files with `tail`
* Converting tabs and spaces
* Joining sorted files by a common field
* Splitting files into smaller parts
* Sorting text by different criteria
* Translating, deleting, and compressing characters
* Removing or counting repeated adjacent lines
* Counting lines, words, bytes, and characters
* Numbering lines
* Searching text with fixed strings and regular expressions

## Key Commands

```bash
>
>>
<
2>
2>&1
|
tee
env
export
cut
paste
head
tail
expand
unexpand
join
split
sort
tr
uniq
wc
nl
grep
```

## Key Concepts

### Redirection

Bash provides different streams for command input and output:

* `stdin` - standard input
* `stdout` - standard output
* `stderr` - standard error

These streams can be redirected to files or connected to other commands.

### Pipelines

The pipe operator `|` sends the output of one command directly to the input of another command.

```bash
command1 | command2
```

This makes it possible to build processing chains from small commands.

### Text Processing

Linux provides many small utilities that perform one focused operation.

For example:

```bash
cat file.txt | grep "error" | sort | uniq
```

Each command processes the result of the previous command.

## What I Learned

The main lesson from this section was that Linux commands can be combined instead of being used independently.

By understanding input/output streams, redirection, and pipelines, I can build command-line workflows that process text efficiently.

I also learned the purpose of several standard text-processing utilities and how they can be combined to inspect, filter, transform, sort, and analyze text.

## Practice

Hands-on exercises for this section are documented in:

* [`commands.md`](./commands.md) — command reference
* [`practice.md`](./practice.md) — practical exercises and notes

