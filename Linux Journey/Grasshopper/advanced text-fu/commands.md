# Advanced Text-Fu - Command Cheat Sheet

A practical reference for the commands, patterns, and editor shortcuts covered in the Advanced Text-Fu section.

---

# 1. Regular Expressions

Regular expressions describe patterns that can be searched for in text.

## Anchors

### Start of line

```text
^
```

Example:

```bash
grep '^error' file.txt
```

Matches lines beginning with `error`.

### End of line

```text
$
```

Example:

```bash
grep 'error$' file.txt
```

Matches lines ending with `error`.

---

## Character Sets

### Match one character from a set

```text
[abc]
```

Matches `a`, `b`, or `c`.

### Character range

```text
[a-z]
```

Matches lowercase letters.

### Negated character set

```text
[^0-9]
```

Matches a character that is not a digit.

---

## Repetition

### Zero or more

```text
*
```

### One or more

```text
+
```

### Zero or one

```text
?
```

### Exact number

```text
{3}
```

### Range

```text
{2,5}
```

These forms are commonly used with extended regular expressions.

Example:

```bash
grep -E 'ab+c' file.txt
```

---

## Useful Regex Examples

### Lines beginning with a word

```bash
grep -E '^user' file.txt
```

### Lines ending with a number

```bash
grep -E '[0-9]$' file.txt
```

### Match either of two words

```bash
grep -E 'error|warning' file.txt
```

### Match one or more digits

```bash
grep -E '[0-9]+' file.txt
```

---

# 2. Text Editors

Linux provides several terminal-based text editors.

The lessons in this section focus on:

* Vim
* Emacs

The important skill is understanding how to work with files directly from the terminal.

---

# 3. Vim

## Start Vim

```bash
vim file.txt
```

or:

```bash
vi file.txt
```

## Open Vim help

From inside Vim:

```vim
:help
```

## Vim Modes

### Normal mode

Used for navigation and commands.

Press:

```text
Esc
```

to return to Normal mode.

### Insert mode

Used for entering text.

Common commands:

```text
i
a
o
O
```

### Command-line mode

Press:

```text
:
```

to enter commands such as:

```vim
:w
:q
```

---

# 4. Vim Search

## Search forward

```text
/pattern
```

Press `Enter` to search.

## Search backward

```text
?pattern
```

## Next match

```text
n
```

## Previous match

```text
N
```

## Remove search highlighting

```vim
:nohlsearch
```

---

# 5. Vim Navigation

## Character movement

```text
h    left
j    down
k    up
l    right
```

## Word movement

```text
w    next word
b    previous word
e    end of word
```

## Beginning and end of line

```text
0    beginning of line
^    first non-blank character
$    end of line
```

## File navigation

```text
gg       beginning of file
G        end of file
Ctrl-d   half-page down
Ctrl-u   half-page up
```

---

# 6. Vim Inserting and Appending

## Insert before cursor

```text
i
```

## Insert after cursor

```text
a
```

## Insert at beginning of line

```text
I
```

## Append at end of line

```text
A
```

## Insert new line below

```text
o
```

## Insert new line above

```text
O
```

---

# 7. Vim Editing

Vim combines operators with motions.

## Delete character

```text
x
```

## Delete line

```text
dd
```

## Copy line

```text
yy
```

## Paste

```text
p
```

## Undo

```text
u
```

## Redo

```text
Ctrl-r
```

## Repeat an operation

```text
.
```

---

# 8. Vim Operators and Motions

Vim commands can often be combined.

For example:

```text
dw
```

Delete from the cursor to the beginning of the next word.

```text
d$
```

Delete from the cursor to the end of the line.

```text
dG
```

Delete from the current position to the end of the file.

This operator + motion model is one of Vim's core editing concepts.

---

# 9. Vim Registers

Registers store copied or deleted text.

Basic operations:

```text
yy
```

Copy the current line.

```text
p
```

Paste from the default register.

```text
"ayy
```

Copy the current line into register `a`.

```text
"ap
```

Paste from register `a`.

---

# 10. Vim Saving and Exiting

## Save

```vim
:w
```

## Quit

```vim
:q
```

## Save and quit

```vim
:wq
```

or:

```text
ZZ
```

## Quit without saving

```vim
:q!
```

## Save under another name

```vim
:w newfile.txt
```

---

# 11. Emacs

## Start Emacs

```bash
emacs file.txt
```

## Emacs Help

```text
Ctrl-h
```

The notation:

```text
C-x
```

means:

```text
Ctrl-x
```

The notation:

```text
M-x
```

usually means pressing `Alt-x` or using the Meta key.

---

# 12. Emacs Files

## Open a file

```text
C-x C-f
```

## Save a file

```text
C-x C-s
```

## Save as

```text
C-x C-w
```

## Revert a file buffer

```text
M-x revert-buffer
```

## Find a file

```text
C-x C-f
```

---

# 13. Emacs Buffers

A buffer contains text being edited or displayed.

## Switch buffers

```text
C-x b
```

## List buffers

```text
C-x C-b
```

## Kill a buffer

```text
C-x k
```

---

# 14. Emacs Windows

An Emacs window is an area displaying a buffer.

## Split window horizontally

```text
C-x 2
```

## Split window vertically

```text
C-x 3
```

## Move between windows

```text
C-x o
```

## Close the current window

```text
C-x 0
```

## Keep only the current window

```text
C-x 1
```

---

# 15. Emacs Editing

## Move point

```text
C-f    forward
C-b    backward
C-n    next line
C-p    previous line
```

## Beginning and end

```text
C-a    beginning of line
C-e    end of line
```

## Kill text

```text
C-k
```

Kills from point to the end of the line.

## Yank text

```text
C-y
```

Yanks previously killed text back into the buffer.

---

# 16. Emacs Point, Region, and Kill Ring

### Point

The current editing position.

### Region

The selected area between point and the mark.

### Kill ring

A collection of text removed using Emacs kill commands.

Common operations:

```text
C-space    set mark
C-w        kill region
C-y        yank
```

---

# 17. Exiting Emacs

## Exit Emacs

```text
C-x C-c
```

If there are unsaved changes, Emacs will ask whether they should be saved.

## Cancel current command

```text
C-g
```

This is useful when a command is active or when I want to cancel the current operation.

---

# Quick Reference

| Tool / Command | Purpose                  |
| -------------- | ------------------------ |
| `^`            | Start of line            |
| `$`            | End of line              |
| `[...]`        | Character set            |
| `*`            | Zero or more repetitions |
| `+`            | One or more repetitions  |
| `?`            | Zero or one              |
| `{n}`          | Exact repetition         |
| `grep -E`      | Extended regex search    |
| `vim file`     | Open file in Vim         |
| `i`            | Insert before cursor     |
| `a`            | Append after cursor      |
| `o`            | New line below           |
| `O`            | New line above           |
| `h/j/k/l`      | Vim navigation           |
| `w/b/e`        | Word navigation          |
| `gg`           | Start of file            |
| `G`            | End of file              |
| `x`            | Delete character         |
| `dd`           | Delete line              |
| `yy`           | Copy line                |
| `p`            | Paste                    |
| `u`            | Undo                     |
| `Ctrl-r`       | Redo                     |
| `/pattern`     | Search forward           |
| `?pattern`     | Search backward          |
| `n`            | Next match               |
| `N`            | Previous match           |
| `:w`           | Save                     |
| `:q`           | Quit                     |
| `:wq`          | Save and quit            |
| `:q!`          | Quit without saving      |
| `emacs file`   | Open file in Emacs       |
| `C-x C-f`      | Open file                |
| `C-x C-s`      | Save file                |
| `C-x C-w`      | Save as                  |
| `C-x b`        | Switch buffer            |
| `C-x k`        | Kill buffer              |
| `C-x 2`        | Split window             |
| `C-x 3`        | Split window vertically  |
| `C-x o`        | Switch window            |
| `C-x 0`        | Close window             |
| `C-x 1`        | Keep current window      |
| `C-a`          | Beginning of line        |
| `C-e`          | End of line              |
| `C-k`          | Kill to end of line      |
| `C-y`          | Yank                     |
| `C-g`          | Cancel command           |
| `C-x C-c`      | Exit Emacs               |

