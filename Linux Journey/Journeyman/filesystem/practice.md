# Filesystem Practice

---

## 1. Filesystem Hierarchy

### Goal

Understand the purpose of the main directories in the Linux filesystem hierarchy.

### Practice

Inspect the root directory:

```bash
ls -la /
```

Inspect several important locations:

```bash
ls -ld /etc /home /tmp /usr /var /dev /proc /sys /run
```

Compare their purposes and identify which directories contain configuration, user data, runtime information, devices, and system information.

### Result

Example observation:

```text
The root directory contains separate locations for configuration,
user data, runtime information, devices, and system-managed data.
```

### What I learned

The Linux filesystem hierarchy provides a predictable organization for different categories of system data.

---

## 2. Filesystem Types

### Goal

Identify the filesystem types currently used by the system.

### Practice

Run:

```bash
lsblk -f
```

Then:

```bash
findmnt
```

And:

```bash
blkid
```

Compare the information returned by the commands.

### Result

Example:

```text
The commands show block devices, filesystem types, UUIDs,
and the locations where filesystems are mounted.
```

### What I learned

Linux can provide a common interface to many different filesystem implementations through the VFS.

---

## 3. Anatomy of a Disk

### Goal

Understand the relationship between a block device, partition, filesystem, and mount point.

### Practice

Inspect block devices:

```bash
lsblk -f
```

Inspect mounted filesystems:

```bash
findmnt
```

Build a simple map:

```text
Device
  ↓
Partition
  ↓
Filesystem
  ↓
Mount Point
```

### Result

Example:

```text
The block-device view and mounted-filesystem view describe
different layers of the storage system.
```

### What I learned

A disk, partition, filesystem, and mounted directory are different concepts and should not be treated as interchangeable.

---

## 4. Disk Partitioning

### Goal

Practice inspecting a partition table without modifying it.

### Practice

First identify available devices:

```bash
lsblk
```

Then inspect a verified lab device:

```bash
sudo parted /dev/sdX print
```

Do not perform partition modifications unless the device is a disposable lab device and its identity has been confirmed.

### Result

Example:

```text
The partition table can be inspected without changing partition boundaries.
```

### What I learned

Partitioning should begin with device identification and verification. A wrong target can result in data loss.

---

## 5. Creating Filesystems

### Goal

Understand the workflow used to create a filesystem on a block device.

### Practice

For a disposable lab device only:

```bash
lsblk -f
```

Verify the target before doing anything destructive.

A conceptual workflow is:

```text
Identify
   ↓
Verify
   ↓
Choose filesystem
   ↓
Create
   ↓
Verify
```

Example command for a disposable lab partition:

```bash
sudo mkfs.ext4 /dev/sdX1
```

Then inspect:

```bash
lsblk -f
```

### Result

Example:

```text
The target device reports the newly created filesystem type
after filesystem creation.
```

### What I learned

Creating a filesystem is a destructive operation when the target already contains data. Device verification is mandatory.

---

## 6. Mount and Unmount

### Goal

Practice attaching and safely detaching a filesystem.

### Practice

Create a mount point:

```bash
sudo mkdir -p /mnt/lab
```

Mount a verified lab filesystem:

```bash
sudo mount /dev/sdX1 /mnt/lab
```

Verify:

```bash
findmnt /mnt/lab
```

```bash
df -h /mnt/lab
```

Unmount:

```bash
sudo umount /mnt/lab
```

Verify again:

```bash
findmnt /mnt/lab
```

### Result

Example:

```text
The filesystem appears in the mount table while mounted
and disappears from the mounted-filesystem view after unmounting.
```

### What I learned

Mounting makes a filesystem accessible through a directory in the Linux filesystem tree.

---

## 7. `/etc/fstab`

### Goal

Understand how Linux defines persistent filesystem mounts.

### Practice

Inspect the current configuration:

```bash
cat /etc/fstab
```

Inspect filesystem UUIDs:

```bash
blkid
```

Understand the structure:

```text
source
mount point
filesystem type
options
dump
pass
```

For a disposable lab filesystem, an entry might conceptually look like:

```text
UUID=<uuid>  /mnt/lab  ext4  defaults  0  2
```

Validate the configuration:

```bash
sudo mount -a
```

Then verify:

```bash
findmnt /mnt/lab
```

### Result

Example:

```text
The fstab entry can be tested without rebooting by using mount -a.
```

### What I learned

`/etc/fstab` provides persistent filesystem configuration, and validation should happen before relying on the configuration during boot.

---

## 8. Swap

### Goal

Understand how swap is initialized and activated.

### Practice

Inspect currently active swap:

```bash
swapon --show
```

```bash
free -h
```

For a disposable lab device or prepared swap area:

```bash
sudo mkswap /dev/sdX2
```

Activate it:

```bash
sudo swapon /dev/sdX2
```

Verify:

```bash
swapon --show
```

Deactivate when finished:

```bash
sudo swapoff /dev/sdX2
```

### Result

Example:

```text
The swap area appears in the active swap list while enabled
and disappears after swapoff.
```

### What I learned

Swap must be initialized before activation and should be managed deliberately.

---

## 9. Disk Usage

### Goal

Compare filesystem-level usage with directory-level usage.

### Practice

Check filesystem usage:

```bash
df -h
```

Check inode usage:

```bash
df -i
```

Check a directory:

```bash
du -sh /var
```

Compare the results.

### Result

Example:

```text
df reports filesystem capacity and available space,
while du reports space consumed by files under a path.
```

### What I learned

`df` and `du` measure different aspects of storage usage and are useful for different troubleshooting questions.

---

## 10. Filesystem Repair

### Goal

Understand a safe filesystem-repair workflow without experimenting on an important system filesystem.

### Practice

First identify the filesystem:

```bash
lsblk -f
```

Then determine which repair utility matches the filesystem type.

Examples:

```bash
fsck.ext4
```

```bash
xfs_repair
```

Before any repair operation:

```text
Identify filesystem
       ↓
Identify device
       ↓
Check backups
       ↓
Unmount if required
       ↓
Use filesystem-specific tool
       ↓
Verify result
```

### Result

Example:

```text
The repair procedure depends on the filesystem type and whether
the filesystem is safely available for offline maintenance.
```

### What I learned

Filesystem repair is filesystem-specific and should be performed only after identifying the correct device and preserving recovery options.

---

## 11. Inode

### Goal

Understand how filenames relate to inode numbers and filesystem metadata.

### Practice

Create a test file:

```bash
touch inode-test.txt
```

Display its inode:

```bash
ls -li inode-test.txt
```

Inspect metadata:

```bash
stat inode-test.txt
```

Create another hard link:

```bash
ln inode-test.txt inode-link.txt
```

Compare:

```bash
ls -li inode-test.txt inode-link.txt
```

### Result

Example:

```text
The original filename and hard link reference the same inode.
```

### What I learned

A filename is a directory entry pointing to an inode. The inode contains filesystem metadata and references the file's data.

---

## 12. Symbolic Links

### Goal

Understand how symbolic links differ from hard links.

### Practice

Create a test file:

```bash
echo "filesystem practice" > target.txt
```

Create a symbolic link:

```bash
ln -s target.txt symbolic-link.txt
```

Inspect both:

```bash
ls -li target.txt symbolic-link.txt
```

Inspect the symbolic link:

```bash
readlink symbolic-link.txt
```

Resolve the path:

```bash
readlink -f symbolic-link.txt
```

Create a hard link:

```bash
ln target.txt hard-link.txt
```

Compare all three:

```bash
ls -li target.txt symbolic-link.txt hard-link.txt
```

### Result

Example:

```text
The symbolic link has its own inode and stores a path to the target.
The hard link shares the target's inode.
```

### What I learned

Symbolic links and hard links provide different mechanisms for referring to filesystem objects.

---

# Mini Exercise — Storage Investigation

## Goal

Perform a read-only investigation of the filesystem and explain how the storage layers fit together.

### Practice

Run:

```bash
lsblk -f
```

```bash
blkid
```

```bash
findmnt
```

```bash
df -h
```

```bash
df -i
```

Inspect inode information for a test file:

```bash
touch filesystem-test.txt
ls -li filesystem-test.txt
stat filesystem-test.txt
```

Create a symbolic link:

```bash
ln -s filesystem-test.txt filesystem-link.txt
```

Inspect:

```bash
ls -li filesystem-test.txt filesystem-link.txt
```

Finally, remove only the test files:

```bash
rm filesystem-test.txt filesystem-link.txt
```

### Result

Record your actual observations in the portfolio.

Suggested format:

```text
Block device:
Filesystem:
Mount point:
Filesystem size:
Available space:
Inode usage:
Test file inode:
Symbolic link inode:
```

### What I learned

The investigation connects the main concepts from the section:

```text
Block Device
     ↓
Filesystem
     ↓
Mount Point
     ↓
Directory
     ↓
File
     ↓
Inode
     ↓
Data
```

It also demonstrates why Linux storage administration requires both a filesystem view and a block-device view.

---

# Safety Checklist

Before performing any storage operation, verify:

* [ ] The target device is correct.
* [ ] The filesystem type is known.
* [ ] The mount state is known.
* [ ] Important data is backed up.
* [ ] The operation is appropriate for the filesystem.
* [ ] The command is being run against the intended device.
* [ ] The result will be verified afterward.

For destructive operations such as partitioning, filesystem creation, and repair, use a disposable lab environment whenever possible.
