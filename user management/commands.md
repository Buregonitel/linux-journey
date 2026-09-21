# User Management - Command Cheat Sheet

A practical reference for the commands, files, and concepts covered in the User Management section.

---

# 1. Users and Groups

## Current user

```bash
whoami
```

Displays the username of the current user.

## User and group IDs

```bash
id
```

Displays the current user's UID, primary GID, and supplementary groups.

### Check another user

```bash
id username
```

---

## User information

```bash
getent passwd username
```

Queries the system's configured user database through NSS.

### Group information

```bash
getent group groupname
```

Queries group information through NSS.

---

# 2. root and Privileged Access

## Switch user with `su`

```bash
su username
```

Starts a shell as another user.

### Switch to root

```bash
su -
```

Starts a login shell as `root` when permitted.

---

## Run a command with `sudo`

```bash
sudo command
```

Runs a command with elevated privileges according to the system's `sudoers` policy.

### Example

```bash
sudo command
```

The exact command should be replaced with the administrative operation that is required.

---

## Check sudo access

```bash
sudo -l
```

Shows the commands the current user is allowed to run through `sudo`.

---

## `sudoers`

The main configuration file is:

```text
/etc/sudoers
```

Use the dedicated editor when modifying it:

```bash
sudo visudo
```

`visudo` helps validate the syntax before applying changes.

---

# 3. `/etc/passwd`

Display the file:

```bash
cat /etc/passwd
```

Search for a specific account:

```bash
grep '^username:' /etc/passwd
```

A typical local entry has seven colon-separated fields:

```text
username:x:UID:GID:GECOS:home:shell
```

| Field      | Meaning                            |
| ---------- | ---------------------------------- |
| `username` | Login name                         |
| `x`        | Password data is stored separately |
| `UID`      | User ID                            |
| `GID`      | Primary group ID                   |
| `GECOS`    | User information/comment field     |
| `home`     | Home directory                     |
| `shell`    | Login shell                        |

For example:

```text
alice:x:1001:1001:Alice:/home/alice:/bin/bash
```

> The exact values depend on the system.

---

# 4. `/etc/shadow`

Display requires appropriate privileges:

```bash
sudo cat /etc/shadow
```

A typical entry contains fields separated by colons.

The file can contain information related to:

* Password hashes
* Password aging
* Account expiration
* Password change dates
* Authentication policy

### Important

Do not publish the contents of `/etc/shadow` in a GitHub repository.

Even when documenting this topic, use fictional or sanitized examples.

---

# 5. `/etc/group`

Display local groups:

```bash
cat /etc/group
```

Search for a specific group:

```bash
grep '^groupname:' /etc/group
```

A typical entry has four fields:

```text
groupname:x:GID:members
```

| Field       | Meaning                     |
| ----------- | --------------------------- |
| `groupname` | Group name                  |
| `x`         | Group password placeholder  |
| `GID`       | Group ID                    |
| `members`   | Supplementary group members |

Example:

```text
developers:x:1002:alice,bob
```

---

# 6. User Management Tools

## Create a user

```bash
sudo useradd username
```

### Create a user with a home directory

```bash
sudo useradd -m username
```

### Specify a login shell

```bash
sudo useradd -m -s /bin/bash username
```

---

## Set a password

```bash
sudo passwd username
```

For the current user:

```bash
passwd
```

---

## Modify a user

```bash
sudo usermod [options] username
```

Example — change the login shell:

```bash
sudo usermod -s /bin/bash username
```

Example — change the home directory:

```bash
sudo usermod -d /home/newhome username
```

---

## Add a user to a supplementary group

```bash
sudo usermod -aG groupname username
```

Important:

```text
-aG
```

means to append the group rather than replace the user's existing supplementary group memberships.

---

## Verify the user

```bash
id username
```

```bash
getent passwd username
```

---

## Delete a user

```bash
sudo userdel username
```

### Delete a user and its home directory

```bash
sudo userdel -r username
```

Use this carefully because it removes the user's home directory and its contents.

---

# Quick Reference

| Command                | Purpose                                     |
| ---------------------- | ------------------------------------------- |
| `whoami`               | Display current username                    |
| `id`                   | Display UID, GID, and groups                |
| `id user`              | Display another user's identity information |
| `getent passwd user`   | Query user information                      |
| `getent group group`   | Query group information                     |
| `su user`              | Switch to another user                      |
| `su -`                 | Start a root login shell                    |
| `sudo command`         | Run a command with elevated privileges      |
| `sudo -l`              | Show allowed sudo commands                  |
| `sudo visudo`          | Safely edit sudoers configuration           |
| `cat /etc/passwd`      | View local passwd entries                   |
| `sudo cat /etc/shadow` | View shadow entries with privileges         |
| `cat /etc/group`       | View local group entries                    |
| `useradd`              | Create a user                               |
| `passwd`               | Set or change a password                    |
| `usermod`              | Modify a user                               |
| `userdel`              | Delete a user                               |

---

# Important Files

```text
/etc/passwd
/etc/shadow
/etc/group
/etc/sudoers
```

These files have different purposes and different security considerations.

---

# Safe Verification Workflow

When managing a test account:

```bash
whoami
id
getent passwd username
id username
```

After making a change:

```bash
id username
getent passwd username
```

For group membership:

```bash
id username
```

For sudo permissions:

```bash
sudo -l
```

This makes it easier to verify the actual state instead of assuming that a command succeeded.

