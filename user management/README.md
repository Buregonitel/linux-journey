# User Management

## Overview

This section focuses on Linux users, groups, authentication-related files, and basic local account management.

I practiced how Linux identifies users and groups, how process credentials affect access decisions, how privileged access works through `root`, `su`, and `sudo`, and how local account information is stored in `/etc/passwd`, `/etc/shadow`, and `/etc/group`.

The section contains 6 interactive lessons.

## Lessons Completed

* [x] Users and Groups
* [x] root
* [x] `/etc/passwd`
* [x] `/etc/shadow`
* [x] `/etc/group`
* [x] User Management Tools

## Skills Practiced

* Understanding Linux user identities
* Understanding group identities
* Working with UID and GID
* Understanding process credentials
* Understanding how access decisions use user and group identity
* Working with the `root` account
* Using `su`
* Using `sudo`
* Understanding the `sudoers` policy
* Reading `/etc/passwd`
* Understanding the structure of `/etc/passwd`
* Reading `/etc/shadow`
* Understanding password hashes and account aging information
* Reading `/etc/group`
* Understanding group membership
* Creating local users
* Modifying user accounts
* Setting and protecting passwords
* Verifying account information
* Removing local users

## Key Files

| File          | Purpose                                              |
| ------------- | ---------------------------------------------------- |
| `/etc/passwd` | Local user account information                       |
| `/etc/shadow` | Local password hashes and password aging information |
| `/etc/group`  | Local group information                              |

## Key Concepts

### Users

Linux identifies users using numeric user IDs called UIDs.

A username is a human-readable representation of a user identity.

### Groups

Groups provide another identity mechanism that can be used when making access decisions.

Each group has a numeric GID.

A user can belong to a primary group and additional supplementary groups.

### Process Credentials

Processes run with user and group credentials.

These credentials are important when Linux determines whether a process can access a resource.

### root

`root` is the privileged administrative identity in Linux.

Commands such as `su` and `sudo` provide controlled ways to perform operations with elevated privileges.

### Account Files

Linux local account information is stored in several files:

```text
/etc/passwd
/etc/shadow
/etc/group
```

These files contain different parts of the account and group information and should not be treated as interchangeable.

## Security Notes

Some commands in this section modify system accounts.

Before creating, modifying, or deleting users, I should verify:

* Which account I am operating on
* Which privileges the command requires
* Which files or resources may be affected
* Whether the change is reversible

I should also avoid exposing password hashes or other sensitive account information in public GitHub repositories.

## What I Learned

The main lesson from this section was that Linux access control starts with identity.

Users and groups are represented by IDs, processes carry credentials, and the kernel uses those identities when making access decisions.

I also learned how local account information is o

