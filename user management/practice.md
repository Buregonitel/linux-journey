# User Management - Practice

Hands-on notes and exercises from the User Management section.

---

## 1. Users and Groups

### Goal

Understand how Linux identifies users and groups.

### Practice

Check the current user:

```bash
whoami
```

Check the current user's identity information:

```bash
id
```

Inspect a user through NSS:

```bash
getent passwd username
```

Inspect a group:

```bash
getent group groupname
```

### Result

The system reports the current user's username, UID, GID, and group memberships.

### What I learned

Linux uses numeric IDs to represent users and groups. Usernames and group names are human-readable representations of those identities.

---

## 2. root

### Goal

Understand privileged access and the difference between normal and administrative operations.

### Practice

Check the current identity:

```bash
whoami
```

Check sudo permissions:

```bash
sudo -l
```

Run an administrative command through `sudo`:

```bash
sudo command
```

If permitted in the environment, switch users:

```bash
su username
```

### Result

Privileged operations can be performed through controlled mechanisms such as `sudo`.

### What I learned

`root` has a highly privileged identity, so administrative commands should be executed deliberately and only when necessary.

---

## 3. `/etc/passwd`

### Goal

Understand the structure of local user account records.

### Practice

View the file:

```bash
cat /etc/passwd
```

Inspect a specific account:

```bash
grep '^username:' /etc/passwd
```

Query the account through NSS:

```bash
getent passwd username
```

### Result

A user entry contains several colon-separated fields describing the account.

### What I learned

`/etc/passwd` contains account information such as the username, UID, primary GID, home directory, and login shell.

The `getent` command is useful because it queries the configured name-service sources rather than assuming that all account information comes directly from `/etc/passwd`.

---

## 4. `/etc/shadow`

### Goal

Understand the purpose and sensitive nature of the shadow database.

### Practice

In a controlled environment:

```bash
sudo cat /etc/shadow
```

Inspect only the structure of an entry and do not copy real values into documentation.

### Result

The shadow database contains authentication-related information that is intentionally separated from the general user database.

### What I learned

`/etc/shadow` contains password hashes and password-aging information.

It is sensitive data and should never be published in a GitHub repository.

---

## 5. `/etc/group`

### Goal

Understand how Linux stores local group information.

### Practice

View groups:

```bash
cat /etc/group
```

Search for a specific group:

```bash
grep '^groupname:' /etc/group
```

Query a group through NSS:

```bash
getent group groupname
```

Check a user's memberships:

```bash
id username
```

### Result

Group records contain a group name, GID, and supplementary member list.

### What I learned

Groups provide an additional identity mechanism that can be used for access control.

---

## 6. User Management Tools

### Goal

Practice creating, modifying, verifying, and removing a test account.

### Practice

Create a test user:

```bash
sudo useradd -m labuser
```

Set a password:

```bash
sudo passwd labuser
```

Verify the account:

```bash
id labuser
```

```bash
getent passwd labuser
```

Inspect the home directory:

```bash
ls -la /home/labuser
```

Change the shell:

```bash
sudo usermod -s /bin/bash labuser
```

Verify the change:

```bash
getent passwd labuser
```

Add the user to a supplementary group:

```bash
sudo usermod -aG groupname labuser
```

Verify group membership:

```bash
id labuser
```

Remove the test account when finished:

```bash
sudo userdel -r labuser
```

### Result

A local test account can be created, configured, verified, and removed using standard Linux account-management tools.

### What I learned

User management should follow a verify-after-change workflow.

Instead of assuming that an operation succeeded, I can check the resulting account state with commands such as:

```bash
id labuser
getent passwd labuser
```

---

# Mini Exercise

## Goal

Combine identity inspection and account management into one controlled workflow.

## Practice

First check the current identity:

```bash
whoami
```

Check current credentials:

```bash
id
```

Create a temporary test account:

```bash
sudo useradd -m textuser
```

Set its password:

```bash
sudo passwd textuser
```

Verify the account:

```bash
id textuser
```

```bash
getent passwd textuser
```

Inspect its home directory:

```bash
ls -la /home/textuser
```

Check the relevant group information:

```bash
id textuser
```

When the exercise is complete, remove the test account:

```bash
sudo userdel -r textuser
```

Verify that the account no longer exists:

```bash
getent passwd textuser
```

### Result

The complete workflow covers account creation, password configuration, identity verification, inspection, and cleanup.

### What I learned

The most important part of user management is understanding the relationship between:

```text
User → UID
Group → GID
Process → Credentials
Account files → Identity information
sudo → Controlled privilege escalation
```

Account-management commands should always be followed by verification, especially when working with privileged operations.

