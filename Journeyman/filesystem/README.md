# Filesystem

## Overview

The **Filesystem** section focuses on how Linux organizes, stores, mounts, and manages data.

The lessons move from the structure of the Linux filesystem to the underlying storage layers, filesystem creation and mounting, persistent configuration, disk usage, filesystem repair, inodes, and symbolic links.

This section also introduces the relationship between:

```text
Storage Device
      ↓
Partition Table
      ↓
Partition
      ↓
Filesystem
      ↓
Mount Point
      ↓
Files and Directories
```

Understanding these layers is essential for Linux system administration and storage troubleshooting.

---

## Lessons

### 1. Filesystem Hierarchy

Learn the purpose of the main Linux directories and understand how modern systems may use unified filesystem layouts.

Key areas include:

* `/`
* `/boot`
* `/etc`
* `/home`
* `/usr`
* `/var`
* `/tmp`
* `/dev`
* `/proc`
* `/sys`
* `/run`
* `/mnt`
* `/media`

The filesystem hierarchy provides a predictable organization for operating system files, configuration, user data, runtime information, and temporary data.

---

### 2. Filesystem Types

Learn how the Linux **Virtual File System (VFS)** provides a common interface for different filesystem implementations.

The VFS allows applications and system utilities to interact with different filesystems through a common API.

Examples include:

* `ext4`
* `xfs`
* `btrfs`
* `tmpfs`
* network filesystems
* pseudo-filesystems

The filesystem type determines how data and metadata are managed underneath the common Linux filesystem interface.

---

### 3. Anatomy of a Disk

Learn how storage is divided into several logical layers.

```text
Block Device
     ↓
Partition Table
     ↓
Partition
     ↓
Filesystem
     ↓
Mount Point
```

Important concepts include:

* block devices
* disks
* partition tables
* partitions
* filesystem metadata
* mounted filesystems

Understanding these layers helps distinguish a disk problem from a partition problem or a filesystem problem.

---

### 4. Disk Partitioning

Learn how to inspect and modify disk partition boundaries using `parted`.

Important concepts include:

* partition tables
* partition boundaries
* partition sizes
* free space
* partition inspection
* controlled partition changes

Partitioning is a potentially destructive operation, so identifying the correct target device and verifying the current layout are essential steps.

---

### 5. Creating Filesystems

Learn how to verify a target block device and create a filesystem using tools specific to the filesystem format.

Typical workflow:

```text
Identify device
      ↓
Verify device
      ↓
Choose filesystem type
      ↓
Create filesystem
      ↓
Verify result
```

Common filesystem creation tools include commands such as:

```bash
mkfs.ext4
mkfs.xfs
```

Creating a filesystem normally destroys existing filesystem data on the target device, so the target must always be verified first.

---

### 6. `mount` and `umount`

Learn how to attach filesystems to the Linux directory tree and safely detach them.

Key concepts include:

* mount points
* mounted filesystems
* filesystem types
* mount options
* checking current mounts
* safely unmounting filesystems

Typical workflow:

```text
Identify filesystem
      ↓
Create or select mount point
      ↓
Mount filesystem
      ↓
Verify
      ↓
Unmount when finished
```

---

### 7. `/etc/fstab`

Learn how `/etc/fstab` defines persistent filesystem and swap configuration.

An `fstab` entry describes information such as:

* filesystem source
* mount point
* filesystem type
* mount options
* dump settings
* filesystem check order

A typical entry has the form:

```text
<source> <mount-point> <type> <options> <dump> <pass>
```

Persistent configuration should be validated before rebooting.

---

### 8. Swap

Learn how Linux uses swap space and how swap can be initialized, activated, sized, and safely deactivated.

Important concepts include:

* swap partitions
* swap files
* `mkswap`
* `swapon`
* `swapoff`
* `/etc/fstab`
* monitoring active swap

Swap provides additional virtual memory backing, but it is not simply an extension of physical RAM.

---

### 9. Disk Usage

Learn how `df` and `du` provide different views of filesystem storage consumption.

`df` focuses on filesystem-level space:

```bash
df -h
```

`du` focuses on directory and file usage:

```bash
du -sh /path
```

These tools answer different questions and can therefore show different results.

---

### 10. Filesystem Repair

Learn how to diagnose filesystem corruption and select an appropriate offline repair process for the filesystem type.

Important principles include:

* identify the filesystem type
* identify the affected device
* preserve data and backups
* avoid repairing a mounted filesystem when the tool requires it to be offline
* use the filesystem-specific repair utility
* verify the filesystem after repair

Filesystem repair should be treated as a controlled recovery procedure rather than a routine command.

---

### 11. Inode

Learn how inode numbers connect directory names with filesystem metadata and file data.

An inode stores metadata such as:

* file type
* permissions
* ownership
* timestamps
* file size
* link count
* references to data blocks

A directory maps names to inode numbers:

```text
Directory Entry
      │
      ├── filename
      │
      └── inode number
              ↓
           Inode
              ↓
         File Metadata
              ↓
           File Data
```

This model explains important filesystem behavior such as hard links and why a filename is not the same thing as a file's underlying inode.

---

### 12. Symbolic Links

Learn the difference between symbolic links and hard links.

A symbolic link stores a path to another filesystem object:

```text
symlink
   ↓
target path
   ↓
target
```

A hard link creates another directory entry referring to the same inode:

```text
filename A ──┐
             ├── inode ── file data
filename B ──┘
```

Important differences include:

| Feature                           | Symbolic Link | Hard Link |
| --------------------------------- | ------------- | --------- |
| Refers to                         | Path          | Inode     |
| Can cross filesystems             | Yes           | No        |
| Can normally point to directories | Yes           | No        |
| Depends on target path            | Yes           | No        |
| Shares inode with target          | No            | Yes       |

---

## Skills Developed

After completing this section, the main practical skills include:

* Navigating the Linux filesystem hierarchy
* Identifying filesystem types
* Understanding storage layers
* Inspecting disk and partition layouts
* Understanding filesystem creation
* Mounting and unmounting filesystems
* Reading and validating `/etc/fstab`
* Managing swap
* Measuring filesystem and directory usage
* Understanding filesystem repair workflows
* Working with inode concepts
* Understanding symbolic and hard links

---

## Key Commands

Some of the important commands and tools covered by this section include:

```bash
ls
findmnt
mount
umount
df
du
lsblk
blkid
parted
mkfs
mkfs.ext4
mkfs.xfs
fsck
mkswap
swapon
swapoff
stat
ln
readlink
```

The exact command and options should always match the filesystem, device, and task being performed.

---

## Storage Model

A useful mental model for Linux storage is:

```text
Physical / Virtual Disk
          │
          ▼
   Block Device
          │
          ▼
  Partition Table
          │
          ▼
      Partition
          │
          ▼
     Filesystem
          │
          ▼
     Mount Point
          │
          ▼
 Linux Directory Tree
          │
          ▼
   Files and Directories
```

Not every storage configuration uses every layer. For example, filesystems may also be placed on logical volumes, encrypted devices, RAID devices, or other block-device abstractions.

---

## Safe Storage Workflow

Storage operations should follow a verification-first approach:

```text
Inspect
  ↓
Identify
  ↓
Verify
  ↓
Plan
  ↓
Change
  ↓
Verify Again
```

For potentially destructive operations:

```text
lsblk / blkid
      ↓
Confirm target
      ↓
Confirm backups
      ↓
Perform operation
      ↓
Check result
```

This is especially important for:

* partitioning
* filesystem creation
* filesystem repair
* mounting the wrong device
* changing `/etc/fstab`

---

## Documentation

* [Commands Cheat Sheet](commands.md)
* [Practice Journal](practice.md)


