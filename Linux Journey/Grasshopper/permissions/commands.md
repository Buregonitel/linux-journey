# Linux Permissions - Command Cheat Sheet

A practical reference for inspecting and modifying Linux permissions, ownership, and special permission bits.

---

## 1. Inspect File Permissions

### `ls -l`

Display detailed information about files and directories:

```bash
ls -l
```

Example:

```text
-rwxr-xr--  user  developers  script.sh
```

The first field contains the file type and permission bits:

```text
-rwxr-xr--
│├───┬───┤
│    │   └── other
│    └────── group
└─────────── owner
```

### Permission characters

| Character | Meaning                |
| --------- | ---------------------- |
| `r`       | Read                   |
| `w`       | Write                  |
| `x`       | Execute                |
| `-`       | Permission not granted |

Permission classes:

| Class | Symbol |
| ----- | ------ |
| Owner | `u`    |
| Group | `g`    |
| Other | `o`    |
| All   | `a`    |

---

## 2. File Types

The first character of an `ls -l` permission string identifies the file type.

Common types include:

| Symbol | File type     |
| ------ | ------------- |
| `-`    | Regular file  |
| `d`    | Directory     |
| `l`    | Symbolic link |

Example:

```text
-rw-r--r--
```

The object is a regular file.

```text
drwxr-xr-x
```

The object is a directory.

---

## 3. `chmod`

Change file or directory permissions.

### Symbolic mode

Add execute permission for the owner:

```bash
chmod u+x script.sh
```

Remove write permission from the group:

```bash
chmod g-w file.txt
```

Add read permission for others:

```bash
chmod o+r file.txt
```

Set permissions for multiple classes:

```bash
chmod u+x,g+rx script.sh
```

---

## 4. Octal Permissions

Permissions can also be represented using numeric values.

```text
r = 4
w = 2
x = 1
```

The values are added for each permission class.

| Permission | Value |
| ---------- | ----: |
| `---`      |   `0` |
| `--x`      |   `1` |
| `-w-`      |   `2` |
| `-wx`      |   `3` |
| `r--`      |   `4` |
| `r-x`      |   `5` |
| `rw-`      |   `6` |
| `rwx`      |   `7` |

### Examples

```bash
chmod 755 script.sh
```

Equivalent to:

```text
rwxr-xr-x
```

Another common example:

```bash
chmod 644 file.txt
```

Equivalent to:

```text
rw-r--r--
```

---

## 5. Ownership

Linux filesystem objects have:

* a user owner;
* a group owner.

Inspect ownership with:

```bash
ls -l
```

Example:

```text
-rw-r--r-- user developers file.txt
```

Here:

```text
user       → owner
developers → group
```

---

## 6. `chown`

Change the user owner of a file.

```bash
chown user file.txt
```

Change both user and group:

```bash
chown user:developers file.txt
```

Change only the group:

```bash
chown :developers file.txt
```

Changing ownership generally requires appropriate privileges.

---

## 7. `chgrp`

Change the group owner of a filesystem object.

```bash
chgrp developers file.txt
```

Example:

```bash
chgrp project-team shared.txt
```

---

## 8. `umask`

Display the current process file creation mask:

```bash
umask
```

A symbolic representation can also be requested:

```bash
umask -S
```

Example output:

```text
u=rwx,g=rwx,o=rx
```

`umask` does not directly assign permissions. Instead, it restricts permission bits requested when new files and directories are created.

---

## 9. Setuid

Setuid is a special permission bit for executable files.

Symbolic form:

```bash
chmod u+s program
```

Remove it:

```bash
chmod u-s program
```

A setuid executable may run with the effective user ID of the file owner.

This is security-sensitive because a privileged owner can affect the privileges available to the process.

---

## 10. Setgid

Setgid can be applied to executable files:

```bash
chmod g+s program
```

Remove it:

```bash
chmod g-s program
```

Setgid also has an important directory behavior.

When applied to a directory, newly created files and subdirectories can inherit the directory's group.

Example:

```bash
chmod g+s shared/
```

This is useful for shared project directories.

---

## 11. Sticky Bit

The sticky bit is commonly used on writable shared directories.

Set it symbolically:

```bash
chmod +t shared/
```

Remove it:

```bash
chmod -t shared/
```

A common example is:

```text
/tmp
```

The sticky bit helps prevent users from deleting or renaming entries belonging to other users in a shared writable directory.

---

## 12. Special Permission Notation

Special permission bits can appear in the permission string.

### Setuid

```text
-rwsr-xr-x
```

The `s` in the owner execute position indicates setuid.

### Setgid

```text
-rwxr-sr-x
```

The `s` in the group execute position indicates setgid.

### Sticky bit

```text
drwxrwxrwt
```

The `t` in the other execute position indicates the sticky bit.

---

## 13. Process User IDs

Linux processes can maintain several user identity values.

### Real UID

Identifies the user associated with the process invocation.

### Effective UID

Used for many permission checks.

### Saved UID

Allows a process to retain a privileged identity that can potentially be restored when permitted.

These identities are especially important when understanding setuid programs and privilege transitions.

---

## Quick Reference

| Command / concept | Purpose                               |
| ----------------- | ------------------------------------- |
| `ls -l`           | Inspect permissions and ownership     |
| `chmod`           | Change permission bits                |
| `chown`           | Change user/group ownership           |
| `chgrp`           | Change group ownership                |
| `umask`           | Inspect file creation permission mask |
| `chmod u+s`       | Set setuid                            |
| `chmod g+s`       | Set setgid                            |
| `chmod +t`        | Set sticky bit                        |
| `r`               | Read                                  |
| `w`               | Write                                 |
| `x`               | Execute                               |
| `u`               | Owner                                 |
| `g`               | Group                                 |
| `o`               | Other                                 |
| `a`               | All                                   |

---

## Common Permission Examples

### Read/write for owner, read-only for everyone else

```bash
chmod 644 file.txt
```

```text
rw-r--r--
```

### Full owner permissions, read/execute for group and others

```bash
chmod 755 script.sh
```

```text
rwxr-xr-x
```

### Add execute permission to a script

```bash
chmod u+x script.sh
```

### Make a shared directory inherit its group

```bash
chmod g+s shared/
```

### Protect entries in a shared writable directory

```bash
chmod +t shared/
```

---

## Security Checklist

Before changing permissions or special bits, consider:

* Who owns the object?
* Which group owns it?
* Who needs read access?
* Who needs write access?
* Who needs execute access?
* Is a special permission bit necessary?
* Could the change give a process unnecessary privileges?
* Is the directory shared between multiple users?

Permissions should follow the principle of granting only the access that is actually required.

