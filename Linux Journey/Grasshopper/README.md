# Grasshopper

## Overview

**Grasshopper** is a practical Linux learning section focused on building a strong foundation in command-line usage, system administration, process management, permissions, software management, text processing, and low-level Linux concepts.

The section combines everyday Linux commands with the underlying concepts that explain how Linux systems work.

Each topic is documented separately with its own:

* `README.md` - topic overview and key concepts
* `commands.md` - command reference and examples
* `practice.md` - hands-on exercises and practice notes

---

## Topics

### 1. Command Line

Learn the fundamentals of working with the Linux shell and navigating the filesystem.

Topics include:

* Shell basics
* Working with directories and files
* File inspection
* Copying, moving, and removing files
* Searching the filesystem
* Command help and manual pages
* Shell aliases
* Exiting the shell

[Open Command Line →](./command-line/)

---

### 2. Packages

Learn how Linux software is distributed, packaged, installed, updated, and removed.

Topics include:

* Software distribution
* Package repositories
* `tar` and `gzip`
* Package dependencies
* `rpm` and `dpkg`
* `yum` and `apt`
* Compiling software from source

[Open Packages →](./packages/)

---

### 3. Permissions

Learn how Linux controls access to files, directories, and processes.

Topics include:

* File permissions
* `chmod`
* Ownership
* `chown` and `chgrp`
* `umask`
* Setuid
* Setgid
* Process credentials
* Sticky bit

[Open Permissions →](./permissions/)

---

### 4. Processes

Learn how Linux creates, manages, monitors, and terminates processes.

Topics include:

* Process inspection with `ps` and `top`
* Controlling terminals
* Process creation with `fork` and `exec`
* Process termination
* Exit codes and zombies
* Signals
* `kill`
* Niceness
* Process states
* `/proc`
* Job control

[Open Processes →](./processes/)

---

### 5. Text-Fu

Learn how to process, transform, search, and combine text from the command line.

Topics include:

* Standard input, output, and error
* Redirection
* Pipes
* `tee`
* Environment variables
* `cut`
* `paste`
* `head` and `tail`
* `expand` and `unexpand`
* `join` and `split`
* `sort`
* `tr`
* `uniq`
* `wc` and `nl`
* `grep`

[Open Text-Fu →](./text-fu/)

---

### 6. User Management

Learn how Linux identifies users and groups and how local accounts are managed.

Topics include:

* Users and groups
* `root`
* `su` and `sudo`
* `/etc/passwd`
* `/etc/shadow`
* `/etc/group`
* User management tools

[Open User Management →](./user-management/)

---

### 7. Advanced Text-Fu

Learn more advanced Linux text-processing and editing techniques.

Topics include:

* Regular expressions
* Text editors
* Vim
* Vim search and navigation
* Vim text insertion and editing
* Vim file operations
* Emacs
* Emacs file manipulation
* Emacs buffers and navigation
* Emacs editing and help

[Open Advanced Text-Fu →](./advanced-text-fu/)

---

## Skills Developed

After completing this section, the main practical skills include:

* Navigating and managing Linux filesystems
* Using the shell efficiently
* Finding and inspecting files
* Redirecting and processing command output
* Building command pipelines
* Searching and transforming text
* Managing users and groups
* Understanding Linux permissions
* Monitoring and controlling processes
* Managing software packages
* Working with regular expressions
* Using Vim and Emacs
* Understanding how common Linux subsystems interact

---

## Linux Concepts

The topics in Grasshopper are connected rather than isolated.

A simplified relationship looks like this:

```text
                    Linux System
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       Users          Processes       Files
          │              │              │
          └─────── Permissions ─────────┘
                         │
                    Command Line
                         │
              ┌──────────┴──────────┐
              │                     │
          Text-Fu             Packages
              │
       Advanced Text-Fu
       Vim / Emacs / Regex
```

For example:

* The **command line** provides the interface for interacting with the system.
* **Users and groups** determine identity.
* **Permissions** determine what an identity can access.
* **Processes** perform the actual work.
* **Text-Fu** provides tools for processing command output and files.
* **Packages** provide software used by the system and users.
* **Advanced Text-Fu** extends command-line text processing through regex and powerful editors.

---

## Practical Workflow

Many Linux administration tasks combine several topics from this section:

```text
Inspect
   ↓
Identify
   ↓
Modify
   ↓
Verify
   ↓
Troubleshoot
```

For example:

```text
ps / ls / id
      ↓
Understand the current state
      ↓
chmod / usermod / package manager
      ↓
Make a controlled change
      ↓
Verify with commands and system information
```

This workflow is useful because Linux administration is not only about knowing individual commands. It also requires understanding **what the system is doing and how to verify changes safely**.

---

## Portfolio Structure

```text
grasshopper/
├── README.md
├── command-line/
│   ├── README.md
│   ├── commands.md
│   └── practice.md
├── packages/
│   ├── README.md
│   ├── commands.md
│   └── practice.md
├── permissions/
│   ├── README.md
│   ├── commands.md
│   └── practice.md
├── processes/
│   ├── README.md
│   ├── commands.md
│   └── practice.md
├── text-fu/
│   ├── README.md
│   ├── commands.md
│   └── practice.md
├── user-management/
│   ├── README.md
│   ├── commands.md
│   └── practice.md
└── advanced-text-fu/
    ├── README.md
    ├── commands.md
    └── practice.md
```

---

## Key Takeaway

Grasshopper builds a practical foundation for working with Linux from the command line.

The section connects everyday shell usage with deeper system concepts such as:

* users and groups
* permissions
* processes
* software management
* text processing
* regular expressions
* text editors

The goal is not simply to memorize commands, but to understand **how Linux works and how to interact with it safely and systematically**.

