# Filesystem Commands

## Filesystem Hierarchy

### Inspect directories

```bash
ls /
ls -la /
```

Useful locations:

| Path     | Purpose                                  |
| -------- | ---------------------------------------- |
| `/`      | Root of the filesystem hierarchy         |
| `/boot`  | Boot-related files                       |
| `/etc`   | System configuration                     |
| `/home`  | User home directories                    |
| `/usr`   | Most user-space programs and shared data |
| `/var`   | Variable data such as logs and caches    |
| `/tmp`   | Temporary files                          |
| `/dev`   | Device nodes                             |
| `/proc`  | Process and kernel information           |
| `/sys`   | Kernel device and system model           |
| `/run`   | Runtime state                            |
| `/mnt`   | Common temporary mount location          |
| `/media` | Common location for removable media      |

---

# Filesystem Types

## Identify filesystem type

```bash
lsblk -f
```

```bash
blkid
```

```bash
findmnt
```

Display the filesystem type of a specific path:

```bash
findmnt /
```

Common filesystem types:

```text
ext4
xfs
btrfs
tmpfs
```

---

# Disk and Block Devices

## List block devices

```bash
lsblk
```

Detailed filesystem information:

```bash
lsblk -f
```

Show UUID and filesystem information:

```bash
blkid
```

A useful inspection workflow:

```bash
lsblk -f
blkid
findmnt
```

---

# Disk Partitioning

## Inspect a partition table

Using `parted`:

```bash
sudo parted /dev/sdX print
```

Interactive mode:

```bash
sudo parted /dev/sdX
```

Inside `parted`:

```text
print
```

Always replace `/dev/sdX` with the verified target device.

> **Warning:** Partitioning can destroy data. Never run partition-modification commands against an unverified device.

---

# Creating Filesystems

## Create an ext4 filesystem

```bash
sudo mkfs.ext4 /dev/sdX1
```

## Create an XFS filesystem

```bash
sudo mkfs.xfs /dev/sdX1
```

Check the result:

```bash
lsblk -f
```

```bash
blkid /dev/sdX1
```

> `mkfs` normally destroys existing filesystem structures on the target.

---

# Mounting Filesystems

## Create a mount point

```bash
sudo mkdir -p /mnt/data
```

## Mount a filesystem

```bash
sudo mount /dev/sdX1 /mnt/data
```

Verify:

```bash
findmnt /mnt/data
```

```bash
df -h /mnt/data
```

---

# Unmounting Filesystems

```bash
sudo umount /mnt/data
```

Verify:

```bash
findmnt /mnt/data
```

If a filesystem cannot be unmounted, first identify processes using it rather than immediately forcing an operation.

Useful diagnostic commands may include:

```bash
findmnt /mnt/data
```

```bash
lsof +D /mnt/data
```

```bash
fuser -vm /mnt/data
```

---

# `/etc/fstab`

## Inspect the file

```bash
cat /etc/fstab
```

```bash
less /etc/fstab
```

## Identify UUIDs

```bash
blkid
```

A typical entry:

```text
UUID=<uuid>  /mnt/data  ext4  defaults  0  2
```

Using UUIDs helps avoid depending on potentially changing device names.

## Test `fstab`

After making a change, validate the configuration:

```bash
sudo mount -a
```

Then verify:

```bash
findmnt
```

> Always validate `/etc/fstab` before rebooting after changing it.

---

# Swap

## Inspect active swap

```bash
swapon --show
```

```bash
free -h
```

## Create swap metadata

For a prepared swap device:

```bash
sudo mkswap /dev/sdX2
```

## Enable swap

```bash
sudo swapon /dev/sdX2
```

## Disable swap

```bash
sudo swapoff /dev/sdX2
```

Verify:

```bash
swapon --show
```

---

# Disk Usage

## Filesystem usage

```bash
df
```

Human-readable:

```bash
df -h
```

Inode usage:

```bash
df -i
```

`df` answers:

> How much space is available in this filesystem?

---

## Directory usage

```bash
du -sh /path
```

Show usage of immediate children:

```bash
du -h --max-depth=1 /path
```

`du` answers:

> How much space is being consumed by files and directories under this path?

---

# Inodes

## Display inode numbers

```bash
ls -li
```

Example:

```text
123456 -rw-r--r-- 2 user user 1024 file.txt
```

The first number is the inode number.

## Inspect metadata

```bash
stat file.txt
```

`stat` can show:

* inode number
* permissions
* ownership
* file size
* timestamps
* link count

---

# Symbolic Links

## Create a symbolic link

```bash
ln -s target link-name
```

Example:

```bash
ln -s /var/log/app.log app.log
```

## Inspect a symbolic link

```bash
ls -l app.log
```

```bash
readlink app.log
```

```bash
readlink -f app.log
```

---

# Hard Links

## Create a hard link

```bash
ln original.txt copy.txt
```

Compare inode numbers:

```bash
ls -li original.txt copy.txt
```

Both directory entries should reference the same inode.

---

# Symbolic Link vs Hard Link

```text
Symbolic link:

link
 │
 └── path ──► target
```

```text
Hard link:

name A ──┐
         ├── inode ──► file data
name B ──┘
```

Comparison:

| Operation                          | Symbolic Link       | Hard Link            |
| ---------------------------------- | ------------------- | -------------------- |
| Create                             | `ln -s target link` | `ln target link`     |
| References                         | Path                | Inode                |
| Same inode as target               | No                  | Yes                  |
| Cross filesystem                   | Yes                 | No                   |
| Broken when target path disappears | Yes                 | No                   |
| Typical directory use              | Supported           | Normally not allowed |

---

# Filesystem Repair

## Identify the filesystem first

```bash
lsblk -f
```

```bash
blkid
```

Repair utilities depend on the filesystem type.

For example, Linux systems may provide filesystem-specific tools such as:

```bash
fsck.ext4
xfs_repair
```

> Filesystem repair is normally performed offline when required by the filesystem/tool. Do not blindly run repair commands against mounted or actively used filesystems.

A safe conceptual workflow:

```text
Identify filesystem
        ↓
Identify device
        ↓
Check backups
        ↓
Unmount if required
        ↓
Run filesystem-specific diagnostic/repair tool
        ↓
Verify filesystem
        ↓
Mount and verify data
```

---

# Quick Reference

| Task                        | Command          |
| --------------------------- | ---------------- |
| List block devices          | `lsblk`          |
| Show filesystem information | `lsblk -f`       |
| Identify filesystem/UUID    | `blkid`          |
| Show mounted filesystems    | `findmnt`        |
| Mount filesystem            | `mount`          |
| Unmount filesystem          | `umount`         |
| Inspect partition table     | `parted`         |
| Create ext4 filesystem      | `mkfs.ext4`      |
| Create XFS filesystem       | `mkfs.xfs`       |
| Check filesystem space      | `df -h`          |
| Check inode usage           | `df -i`          |
| Check directory usage       | `du -sh`         |
| Inspect file metadata       | `stat`           |
| Show inode numbers          | `ls -li`         |
| Create symbolic link        | `ln -s`          |
| Resolve symbolic link       | `readlink`       |
| Initialize swap             | `mkswap`         |
| Enable swap                 | `swapon`         |
| Disable swap                | `swapoff`        |
| Inspect persistent mounts   | `cat /etc/fstab` |
| Test `fstab` entries        | `mount -a`       |

---

# Safe Inspection Workflow

For an unfamiliar storage configuration:

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

This provides a useful overview of:

* block devices
* partitions
* filesystem types
* UUIDs
* mount points
* available space
* inode usage

Only after identifying the target should a potentially modifying operation be considered.

