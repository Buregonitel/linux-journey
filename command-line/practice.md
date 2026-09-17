# Command Line - Practice

Hands-on practice based on the Linux Journey Command Line lessons.

---

## 1. The Shell

### Goal

Understand what the Linux shell is and how commands are executed.

### Practice

```bash
echo "Hello, Linux"
```

### Result

The shell accepted the command and displayed the output in the terminal.

### What I learned

The shell provides an interface for interacting with the operating system by executing commands.

---

## 2. `pwd` - Print Working Directory

### Goal

Identify the current location in the filesystem.

### Practice

```bash
pwd
```

### Result

```text
/home/ilya
```

### What I learned

`pwd` displays the absolute path of the current working directory.

---

## 3. `cd` - Change Directory

### Goal

Navigate through the filesystem.

### Practice

```bash
cd /tmp
pwd

cd ..
pwd

cd ~
pwd
```

### What I learned

`cd` changes the current working directory.

I practiced both absolute paths and relative paths.

---

## 4. `ls` - List Directories

### Goal

Inspect files and directories.

### Practice

```bash
ls
ls -l
ls -a
ls -la
```

### What I learned

Different `ls` options provide different levels of information about directory contents.

---

## 5. `touch`

### Goal

Create files and work with file timestamps.

### Practice

```bash
touch example.txt
ls -l example.txt
```

### What I learned

`touch` can create an empty file and can also update its timestamps.

---

## 6. `file`

### Goal

Identify a file's content type.

### Practice

```bash
file example.txt
```

### Result

```text
example.txt: ASCII text
```

### What I learned

The `file` command determines the likely file type from its contents rather than relying only on the filename extension.

---

## 7. `cat`

### Goal

Display and combine file contents.

### Practice

```bash
echo "Hello Linux" > example.txt
cat example.txt
```

### What I learned

`cat` can display file contents and can be combined with shell redirection.

---

## 8. `less`

### Goal

Read and navigate through longer text files.

### Practice

```bash
less example.txt
```

### What I learned

`less` allows interactive navigation and searching through text without loading the entire file into a simple terminal output.

---

## 9. `history`

### Goal

Inspect previously executed commands.

### Practice

```bash
history
```

### What I learned

Bash keeps a history of previously executed commands, which can be inspected and reused.

---

## 10. `cp` - Copy

### Goal

Copy files and directories.

### Practice

```bash
cp example.txt example-copy.txt
ls -l
```

### What I learned

`cp` creates a copy while leaving the original file unchanged.

---

## 11. `mv` - Move

### Goal

Move and rename files.

### Practice

```bash
mv example-copy.txt renamed.txt
ls -l
```

### What I learned

`mv` can be used both to move a file to another location and to rename it.

---

## 12. `mkdir`

### Goal

Create directories.

### Practice

```bash
mkdir practice
mkdir -p practice/linux/command-line
```

### What I learned

`mkdir -p` allows multiple levels of directories to be created when the parent directories do not already exist.

---

## 13. `rm` - Remove

### Goal

Remove files and directories safely.

### Practice

```bash
touch temporary.txt
rm temporary.txt
```

### What I learned

`rm` permanently removes files, so the target should be checked before executing the command.

---

## 14. `find`

### Goal

Search for files and directories.

### Practice

```bash
find . -name "*.txt"
find . -type f
find . -type d
```

### What I learned

`find` can search directory trees using conditions such as name and file type.

---

## 15. `help`

### Goal

Use Bash's built-in help system.

### Practice

```bash
help cd
```

### What I learned

`help` provides information about Bash built-in commands.

---

## 16. `man`

### Goal

Read installed manual pages.

### Practice

```bash
man ls
```

Useful actions inside `man`:

```text
/keyword    Search
n           Next match
q           Quit
```

### What I learned

Manual pages provide detailed information about commands, options and usage.

---

## 17. `whatis`

### Goal

Get a short description of a command.

### Practice

```bash
whatis ls
whatis find
```

### What I learned

`whatis` provides a concise description based on the installed manual-page database.

---

## 18. `alias`

### Goal

Create and manage Bash command aliases.

### Practice

```bash
alias ll='ls -la'
ll
```

Check aliases:

```bash
alias
```

Remove the alias:

```bash
unalias ll
```

### What I learned

Aliases can provide convenient shortcuts for frequently used commands.

---

## 19. `exit`

### Goal

Exit the current shell.

### Practice

```bash
exit
```

### What I learned

`exit` terminates the current shell and can return an exit status to the calling process.

---

# Final Practice

## Mini Exercise - File Management

I combined several commands from this section:

```bash
mkdir -p ~/linux-practice/documents
cd ~/linux-practice/documents

touch notes.txt
echo "Linux Command Line Practice" > notes.txt

cat notes.txt
file notes.txt

cp notes.txt backup.txt
mv backup.txt backup-notes.txt

find . -type f
ls -la
```

### What I practiced

* Creating directories
* Navigating with `cd`
* Creating files with `touch`
* Writing and reading file contents
* Identifying file types
* Copying files
* Renaming files
* Searching for files
* Listing directory contents

## Summary

After completing this section, I can perform common filesystem and shell operations from the Linux command line and use built-in documentation to learn about commands independently.

