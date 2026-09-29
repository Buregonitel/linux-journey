# Advanced Text-Fu - Practice

Hands-on notes and exercises from the Advanced Text-Fu section.

> Replace example commands and results with the actual commands and outputs from your own practice environment where appropriate.

---

## 1. Regular Expressions

### Goal

Understand how regular expressions describe patterns in text.

### Practice

Search for lines beginning with `error`:

```bash
grep -E '^error' file.txt
```

Search for lines ending with a number:

```bash
grep -E '[0-9]$' file.txt
```

Search for one or more digits:

```bash
grep -E '[0-9]+' file.txt
```

Search for multiple alternatives:

```bash
grep -E 'error|warning' file.txt
```

### Result

Different regex components can be combined to describe specific text patterns.

### What I learned

Regular expressions provide a flexible way to search for patterns rather than only exact strings.

---

## 2. Text Editors

### Goal

Understand the role of terminal text editors in Linux.

### Practice

Check which editors are available:

```bash
vim --version
```

```bash
emacs --version
```

Open a test file:

```bash
vim test.txt
```

### Result

Terminal editors allow files to be created and modified without leaving the command line.

### What I learned

A terminal text editor is an important tool for Linux administration and development.

---

# Vim

## 3. Vim

### Goal

Learn how Vim works and how to open files and access its help system.

### Practice

Open a file:

```bash
vim test.txt
```

Inside Vim, open help:

```vim
:help
```

Return to normal mode:

```text
Esc
```

### Result

Vim opens files directly in the terminal and provides built-in documentation.

### What I learned

Vim is a modal editor, so the meaning of keyboard input depends on the current mode.

---

## 4. Vim Search Patterns

### Goal

Search for text inside a Vim buffer.

### Practice

Search forward:

```text
/error
```

Search backward:

```text
?error
```

Move to the next match:

```text
n
```

Move to the previous match:

```text
N
```

Remove search highlighting:

```vim
:nohlsearch
```

### Result

Text can be searched in both directions and repeated searches can be navigated quickly.

### What I learned

Vim's search commands make it possible to locate text without manually moving through the file.

---

## 5. Vim Navigation

### Goal

Practice moving through a file using Vim's normal mode.

### Practice

Move by character:

```text
h j k l
```

Move by words:

```text
w
b
e
```

Move within a line:

```text
0
^
$
```

Move through the file:

```text
gg
G
```

### Result

The cursor can be moved efficiently without using arrow keys.

### What I learned

Vim navigation is designed around keyboard commands that can also be combined with editing operators.

---

## 6. Vim Inserting and Appending Text

### Goal

Practice entering text at different positions.

### Practice

Insert before the cursor:

```text
i
```

Append after the cursor:

```text
a
```

Insert at the beginning of a line:

```text
I
```

Append at the end:

```text
A
```

Create a line below:

```text
o
```

Create a line above:

```text
O
```

Return to normal mode:

```text
Esc
```

### Result

Text can be inserted at different positions without manually moving the cursor first.

### What I learned

Vim provides several specialized commands for entering text efficiently.

---

## 7. Vim Editing

### Goal

Practice deleting, copying, pasting, and undoing changes.

### Practice

Delete a character:

```text
x
```

Delete a line:

```text
dd
```

Copy a line:

```text
yy
```

Paste:

```text
p
```

Undo:

```text
u
```

Redo:

```text
Ctrl-r
```

Repeat an operation:

```text
.
```

### Result

Common editing operations can be performed without entering insert mode.

### What I learned

Vim separates navigation and editing from text insertion, making normal-mode commands central to the workflow.

---

## 8. Vim Saving and Exiting

### Goal

Learn how to save changes, save under another name, and exit safely.

### Practice

Save:

```vim
:w
```

Save and exit:

```vim
:wq
```

Save under another name:

```vim
:w copy.txt
```

Exit without saving:

```vim
:q!
```

### Result

Changes can be saved or discarded intentionally.

### What I learned

Understanding Vim's save and exit commands is essential for avoiding accidental loss of changes.

---

# Emacs

## 9. Emacs

### Goal

Learn the basic Emacs interface and its terminology.

### Practice

Open a file:

```bash
emacs test.txt
```

Use the keyboard notation:

```text
C-x
```

for `Ctrl-x`.

Use:

```text
M-x
```

for Meta commands.

Open help:

```text
C-h
```

### Result

Emacs provides a keyboard-driven editing environment with commands accessible through key combinations.

### What I learned

Emacs uses its own command notation and concepts such as buffers, windows, and frames.

---

## 10. Emacs File Manipulation

### Goal

Practice opening, saving, and managing file buffers.

### Practice

Open a file:

```text
C-x C-f
```

Save:

```text
C-x C-s
```

Save under another name:

```text
C-x C-w
```

Revert a buffer:

```text
M-x revert-buffer
```

### Result

Files can be opened and managed directly from Emacs.

### What I learned

Emacs treats files through buffers, which provide the editing environment for file contents.

---

## 11. Emacs Buffer Navigation

### Goal

Practice switching and managing buffers and windows.

### Practice

Switch buffers:

```text
C-x b
```

List buffers:

```text
C-x C-b
```

Kill a buffer:

```text
C-x k
```

Split the current window:

```text
C-x 2
```

Split vertically:

```text
C-x 3
```

Move between windows:

```text
C-x o
```

Close the current window:

```text
C-x 0
```

### Result

Multiple buffers and windows can be managed within the same Emacs session.

### What I learned

Buffers contain the content, while windows control how buffers are displayed.

---

## 12. Emacs Editing

### Goal

Practice navigation and editing using point, region, and the kill ring.

### Practice

Move forward:

```text
C-f
```

Move backward:

```text
C-b
```

Move to the next line:

```text
C-n
```

Move to the previous line:

```text
C-p
```

Go to the beginning of a line:

```text
C-a
```

Go to the end:

```text
C-e
```

Set the mark:

```text
C-space
```

Kill selected text:

```text
C-w
```

Yank killed text:

```text
C-y
```

### Result

Text can be navigated, selected, removed, and restored using Emacs commands.

### What I learned

Emacs uses point and region to define editing positions and the kill ring to manage removed text.

---

## 13. Exiting Emacs and Help

### Goal

Learn how to safely exit Emacs and cancel commands.

### Practice

Cancel an active command:

```text
C-g
```

Exit Emacs:

```text
C-x C-c
```

Open help:

```text
C-h
```

### Result

Emacs can be exited safely and active operations can be cancelled when necessary.

### What I learned

Knowing how to cancel commands and access help makes working in a terminal editor much safer.

---

# Mini Exercise

## Goal

Combine regular expressions, Vim navigation, editing, and file management into one workflow.

## Practice

Create a test file:

```bash
printf "error: disk full\ninfo: backup started\nwarning: low memory\nerror: service stopped\n" > messages.txt
```

Search for errors with a regular expression:

```bash
grep -E '^error:' messages.txt
```

Open the file in Vim:

```bash
vim messages.txt
```

Inside Vim:

1. Search for `error`:

```text
/error
```

2. Move between matches:

```text
n
```

3. Navigate using:

```text
h j k l
```

4. Copy a line:

```text
yy
```

5. Paste it:

```text
p
```

6. Undo the change:

```text
u
```

7. Save and exit:

```vim
:wq
```

Verify the file:

```bash
cat messages.txt
```

### Result

The workflow combines shell commands, regex searching, Vim navigation, editing, undo, and file verification.

### What I learned

Advanced text work in Linux becomes much more powerful when shell tools, regular expressions, and terminal editors are used together.

The goal is not to memorize every command, but to understand how to combine the tools to solve practical text-editing and administration tasks.

