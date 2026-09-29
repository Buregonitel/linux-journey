# Command Line Cheat Sheet

## Shell

| Command / Concept | Purpose                                             |
| ----------------- | --------------------------------------------------- |
| Shell             | Interface for interacting with the operating system |
| Bash              | Common Linux shell used to execute commands         |

---

## Navigation

| Command  | Purpose                                          |
| -------- | ------------------------------------------------ |
| `pwd`    | Show the current working directory               |
| `cd`     | Change the current directory                     |
| `cd ..`  | Move to the parent directory                     |
| `cd ~`   | Move to the home directory                       |
| `cd -`   | Return to the previous directory                 |
| `ls`     | List files and directories                       |
| `ls -l`  | Show detailed file information                   |
| `ls -a`  | Show hidden files                                |
| `ls -la` | Show detailed information including hidden files |

---

## Files

| Command | Purpose                                      |
| ------- | -------------------------------------------- |
| `touch` | Create an empty file or update timestamps    |
| `file`  | Identify the type of a file                  |
| `cat`   | Display file contents                        |
| `less`  | Read and navigate through text interactively |
| `cp`    | Copy files or directories                    |
| `mv`    | Move or rename files or directories          |
| `rm`    | Remove files or directories                  |

---

## Directories

| Command    | Purpose                   |
| ---------- | ------------------------- |
| `mkdir`    | Create a directory        |
| `mkdir -p` | Create nested directories |

---

## Searching

| Command        | Purpose                          |
| -------------- | -------------------------------- |
| `find`         | Search for files and directories |
| `find -name`   | Search by name                   |
| `find -type f` | Search for regular files         |
| `find -type d` | Search for directories           |

---

## Command History

| Command            | Purpose                                      |
| ------------------ | -------------------------------------------- |
| `history`          | Display command history                      |
| `history <number>` | Display a specific number of recent commands |

---

## Getting Help

| Command  | Purpose                               |
| -------- | ------------------------------------- |
| `help`   | Show help for Bash built-in commands  |
| `man`    | Open a command's manual page          |
| `whatis` | Show a short description of a command |

### Examples

```bash
help cd
man ls
whatis find
```

---

## Bash Aliases

| Command                | Purpose                           |
| ---------------------- | --------------------------------- |
| `alias`                | Display or create command aliases |
| `alias name='command'` | Create an alias                   |
| `unalias`              | Remove an alias                   |

### Example

```bash
alias ll='ls -la'
```

---

## Shell Exit

| Command  | Purpose                     |
| -------- | --------------------------- |
| `exit`   | Exit the current shell      |
| `exit 0` | Exit successfully           |
| `exit 1` | Exit with a non-zero status |

---

## Useful Command Examples

### Navigation

```bash
pwd
ls -la
cd /tmp
cd ..
cd ~
```

### File management

```bash
touch file.txt
mkdir project
cp file.txt project/
mv file.txt renamed.txt
rm renamed.txt
```

### File inspection

```bash
file document.txt
cat document.txt
less document.txt
```

### Searching

```bash
find . -name "*.txt"
find . -type f
find . -type d
```

### Getting help

```bash
help cd
man ls
whatis find
```

### Bash aliases

```bash
alias ll='ls -la'
alias
```

### History

```bash
history
```

### Exit

```bash
exit
```

