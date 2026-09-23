# Linux Permissions - Practice Journal

Hands-on practice for understanding Linux permission bits, ownership, `umask`, special permission bits, and process identities.

---

## 1. File Permissions

### Goal

Learn how Linux represents file types and permission bits for the owner, group, and other users.

### Practice

Create a test file:

```bash
touch permissions.txt
```

Inspect it:

```bash
ls -l permissions.txt
```

Examine the permission string.

For example:

```text
-rw-r--r--
```

Break it into:

```text
- | rw- | r-- | r--
    owner group other
```

### Result

Record:

* file type;
* owner;
* group;
* owner permissions;
* group permissions;
* other permissions.

### What I learned

Linux permissions are divided into three main classes: owner, group, and other. The first character also identifies the file type.

---

## 2. Modifying Permissions

### Goal

Practice changing permissions with symbolic and octal `chmod` modes.

### Practice

Create a test script:

```bash
touch script.sh
```

Add execute permission for the owner:

```bash
chmod u+x script.sh
```

Check the result:

```bash
ls -l script.sh
```

Set a complete permission mode:

```bash
chmod 755 script.sh
```

Verify it again:

```bash
ls -l script.sh
```

### Result

Record the permission string before and after each `chmod` operation.

### What I learned

`chmod` can modify permissions symbolically or assign an exact permission mode using octal notation.

---

## 3. Ownership Permissions

### Goal

Understand how Linux associates filesystem objects with a user owner and group owner.

### Practice

Inspect a test file:

```bash
ls -l permissions.txt
```

Identify:

```text
owner
group
```

If the lab environment allows ownership changes, practice:

```bash
chown user permissions.txt
```

Change the group:

```bash
chgrp developers permissions.txt
```

Verify:

```bash
ls -l permissions.txt
```

### Result

Record the owner and group before and after the changes.

### What I learned

Ownership is separate from the permission bits. Linux uses both ownership information and permission bits when determining access.

---

## 4. `umask`

### Goal

Understand how `umask` restricts permissions for newly created files and directories.

### Practice

Check the current mask:

```bash
umask
```

Also inspect the symbolic form:

```bash
umask -S
```

Create a test file:

```bash
touch umask-test.txt
```

Inspect its permissions:

```bash
ls -l umask-test.txt
```

Compare the observed permissions with the current `umask`.

### Result

Record:

```text
umask:
new file permissions:
```

### What I learned

`umask` acts as a permission restriction applied when new filesystem objects are created. It does not simply assign a fixed permission value to every new file.

---

## 5. Setuid

### Goal

Understand the set-user-ID permission bit and why it is security-sensitive.

### Practice

Inspect permissions of a suitable executable in the lab:

```bash
ls -l /path/to/program
```

For a controlled test file, set the setuid bit if the lab permits it:

```bash
chmod u+s program
```

Inspect the permission string:

```bash
ls -l program
```

Look for:

```text
s
```

in the owner execute position.

Remove the bit after testing:

```bash
chmod u-s program
```

### Result

Record how the permission string changed when setuid was enabled and disabled.

### What I learned

Setuid can affect the effective user identity of an executable process. Because it can introduce privilege changes, setuid programs require careful security review.

---

## 6. Setgid

### Goal

Understand setgid on executable files and directories.

### Practice

Create a shared directory in the lab:

```bash
mkdir shared
```

Set the setgid bit:

```bash
chmod g+s shared
```

Verify:

```bash
ls -ld shared
```

Look for:

```text
s
```

in the group execute position.

If the lab provides multiple users/groups, create a file inside the directory and inspect its group ownership.

### Result

Record:

* directory permissions;
* directory group;
* group of newly created files.

### What I learned

Setgid has two important contexts: it can affect the group identity of an executable process, and on directories it can support group inheritance for shared workspaces.

---

## 7. Process Permissions

### Goal

Understand real, effective, and saved user IDs.

### Practice

Inspect process information using the tools available in the lab.

For example:

```bash
id
```

For a running process, inspect its status information if available:

```bash
cat /proc/<PID>/status
```

Look for UID-related information.

Compare the identities with the concept of:

```text
Real UID
Effective UID
Saved UID
```

### Result

Record which identity values are visible and what each one represents.

### What I learned

Linux processes can maintain multiple user identity values. The effective identity is particularly important for permission checks and privilege management.

---

## 8. Sticky Bit

### Goal

Understand how the sticky bit protects entries in shared writable directories.

### Practice

Inspect a directory such as `/tmp`:

```bash
ls -ld /tmp
```

Look at the final permission position.

A sticky directory commonly appears with:

```text
drwxrwxrwt
```

For a controlled lab directory, create a shared writable directory and set the sticky bit:

```bash
mkdir shared-tmp
chmod 777 shared-tmp
chmod +t shared-tmp
```

Verify:

```bash
ls -ld shared-tmp
```

Remove the test directory after the exercise if appropriate.

### Result

Record the permission string before and after enabling the sticky bit.

### What I learned

The sticky bit provides additional protection in writable shared directories by restricting deletion and renaming of entries.

---

# Final Mini Exercise

## Goal

Combine standard permissions, ownership, `umask`, setgid, and the sticky bit in one controlled exercise.

### Step 1 - Create a workspace

```bash
mkdir permissions-lab
cd permissions-lab
```

### Step 2 - Create test files

```bash
touch file.txt
touch script.sh
```

### Step 3 - Configure standard permissions

```bash
chmod 644 file.txt
chmod 755 script.sh
```

Verify:

```bash
ls -l
```

### Step 4 - Practice ownership inspection

```bash
ls -l file.txt
```

Identify:

```text
owner
group
```

### Step 5 - Check `umask`

```bash
umask
```

Create another file:

```bash
touch umask-test.txt
```

Compare its permissions with the existing files.

### Step 6 - Practice setgid

Create a shared directory:

```bash
mkdir shared
chmod 2775 shared
```

Verify:

```bash
ls -ld shared
```

The leading `2` represents the setgid special bit in octal notation.

### Step 7 - Practice sticky bit

Create another directory:

```bash
mkdir shared-tmp
chmod 1777 shared-tmp
```

Verify:

```bash
ls -ld shared-tmp
```

The leading `1` represents the sticky bit.

### Step 8 - Document the results

Record:

```text
file.txt:
script.sh:
umask-test.txt:
shared/:
shared-tmp/:
```

For each object, document:

* file type;
* owner;
* group;
* permissions;
* special permission bits.

---

# Reflection

After completing the exercises, answer:

1. How are owner, group, and other permissions represented?
2. What is the difference between symbolic and octal `chmod`?
3. How does ownership affect access decisions?
4. What does `umask` control?
5. Why are setuid programs security-sensitive?
6. How does setgid help shared directories?
7. What is the difference between real and effective user IDs?
8. Why is the sticky bit useful in `/tmp`-style directories?

---

## Key Takeaway

Linux access control is built from several interacting mechanisms:

```text
File type
    +
Owner / Group
    +
rwx permissions
    +
umask for new objects
    +
special permission bits
    +
process identity
```

Understanding how these mechanisms interact is essential for managing Linux systems safely and predictably.

