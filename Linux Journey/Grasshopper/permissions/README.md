# Linux Permissions

This section covers how Linux controls access to files, directories, processes, and privileged operations through permission bits, ownership, `umask`, and special permission bits.

## Lessons Completed

1. **File Permissions**

   * Read Linux file types and permission bits.
   * Understand permissions for the owner, group, and other users.

2. **Modifying Permissions**

   * Change permissions with `chmod`.
   * Use symbolic and octal permission modes.

3. **Ownership Permissions**

   * Check and change the user owner and group owner of filesystem objects.

4. **Umask**

   * Understand how a process `umask` limits permissions requested for newly created files and directories.

5. **Setuid**

   * Understand how the set-user-ID bit affects executable programs.
   * Recognize why setuid programs require careful security review.

6. **Setgid**

   * Understand how set-group-ID affects executable credentials.
   * Understand group inheritance in shared directories.

7. **Process Permissions**

   * Understand real, effective, and saved user IDs.
   * Learn how processes track their caller and manage privileges.

8. **Sticky Bit**

   * Understand how the sticky bit protects entries in writable shared directories such as `/tmp`.

---

## Skills Practiced

* Reading Linux file types and permission bits
* Understanding owner, group, and other permissions
* Using symbolic and octal modes with `chmod`
* Checking and changing file ownership
* Understanding `umask`
* Understanding setuid and setgid
* Understanding real, effective, and saved user IDs
* Understanding the sticky bit
* Recognizing the security implications of special permission bits

---

## Key Concepts

### Permission Classes

Linux permissions are commonly represented using three classes:

```text
owner
group
other
```

For example:

```text
-rwxr-xr--
```

The permission groups are:

```text
rwx | r-x | r--
 ↓     ↓     ↓
owner group other
```

Where:

* `r` — read
* `w` — write
* `x` — execute

---

## Permission Modes

Permissions can be modified using symbolic or octal notation.

### Symbolic mode

```bash
chmod u+x script.sh
chmod g-w file.txt
chmod o+r file.txt
```

### Octal mode

```bash
chmod 755 script.sh
chmod 644 file.txt
```

The common permission values are:

```text
r = 4
w = 2
x = 1
```

---

## Ownership

Every filesystem object has an owner and a group.

Ownership can be inspected and modified using tools such as:

```bash
ls -l
chown
chgrp
```

Example:

```bash
chown user file.txt
chgrp developers file.txt
```

---

## `umask`

`umask` controls which permission bits are restricted when new files and directories are created.

Check the current mask:

```bash
umask
```

A process applies its `umask` when creating new filesystem objects.

---

## Special Permission Bits

Linux provides additional permission mechanisms beyond the standard `rwx` bits.

### Setuid

The set-user-ID bit can cause an executable program to run with the effective user ID of the file owner.

### Setgid

The set-group-ID bit can affect:

* the group identity used by executable programs;
* group inheritance in directories.

### Sticky Bit

The sticky bit is commonly used on shared writable directories.

For example:

```text
/tmp
```

It helps restrict who can remove or rename entries inside the directory.

---

## Process Permissions

Linux processes can have multiple user identity values, including:

* real user ID;
* effective user ID;
* saved user ID.

These identities allow a process to track its caller and temporarily manage privileges.

---

## Security Notes

Permission management is an important part of Linux security.

Special permission bits should be used and reviewed carefully because they can affect process privileges and access control.

In particular:

* avoid granting unnecessary write or execute permissions;
* review ownership carefully;
* understand the effect of `umask`;
* treat setuid and setgid executables as security-sensitive;
* be careful when changing permissions on shared directories.

---

## What I Learned

After completing this section, I can:

* interpret Linux permission strings;
* distinguish owner, group, and other permissions;
* modify permissions with symbolic and octal `chmod` modes;
* understand filesystem ownership;
* check and change user and group ownership;
* explain the role of `umask`;
* explain the purpose and security implications of setuid and setgid;
* distinguish real, effective, and saved user IDs;
* explain how the sticky bit protects shared writable directories.

---

## Documentation

* [Commands Cheat Sheet](commands.md)
* [Practice Journal](practice.md)

