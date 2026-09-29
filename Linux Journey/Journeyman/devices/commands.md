# Devices - Commands Cheat Sheet

A practical reference for inspecting Linux devices, device nodes, hardware topology, and low-level block copying.

---

## 1. `/dev`

List device nodes:

```bash
ls -l /dev
```

Inspect a specific device:

```bash
ls -l /dev/sda
ls -l /dev/null
```

The first character in `ls -l` output helps identify the object type.

Common examples:

```text
c    character device
b    block device
p    named pipe
-    regular file
```

---

## 2. Device Types

### Character device

Character devices provide stream-oriented access.

Example:

```bash
ls -l /dev/tty
```

### Block device

Block devices provide block-oriented access and are commonly used for storage.

Example:

```bash
ls -l /dev/sda
```

### Other filesystem objects

Pipes and sockets are different from device nodes:

```text
p    named pipe
s    socket
```

A device node should not be confused with every special filesystem object.

---

## 3. Device Names

Common Linux storage names:

| Device            | Meaning               |
| ----------------- | --------------------- |
| `/dev/sda`        | disk device           |
| `/dev/sda1`       | partition on the disk |
| `/dev/nvme0n1`    | NVMe device/namespace |
| `/dev/nvme0n1p1`  | NVMe partition        |
| `/dev/mapper/...` | device-mapper device  |

Persistent device references:

```bash
ls -l /dev/disk/
ls -l /dev/disk/by-id/
ls -l /dev/disk/by-uuid/
ls -l /dev/disk/by-path/
```

Persistent paths can help avoid depending on a potentially changing device name such as `/dev/sda`.

---

## 4. sysfs

Inspect the top-level sysfs hierarchy:

```bash
ls /sys
```

Explore device information:

```bash
ls /sys/devices
ls /sys/class
ls /sys/bus
ls /sys/block
ls /sys/module
```

Useful relationships to investigate:

```text
device
  ↓
bus
  ↓
driver
  ↓
class
```

`/sys` exposes the kernel's current device model rather than serving as the normal data-access interface for devices.

---

## 5. udev

Inspect udev-related information:

```bash
udevadm info /dev/sda
```

Query a device using its device path:

```bash
udevadm info --query=all --name=/dev/sda
```

Monitor device events:

```bash
udevadm monitor
```

The exact available options can vary by distribution, so use:

```bash
udevadm --help
```

or:

```bash
man udevadm
```

### What udev does

```text
Kernel event
    ↓
udev
    ↓
udev rules / policy
    ↓
device permissions / naming / links
    ↓
user-space device interface
```

---

## 6. `lsusb`

List USB devices:

```bash
lsusb
```

Show more detailed USB information:

```bash
lsusb -v
```

List USB devices with additional tree information:

```bash
lsusb -t
```

Typical use:

```text
lsusb
  ↓
identify USB hardware
  ↓
investigate a specific device
  ↓
check its driver/interface information
```

---

## 7. `lspci`

List PCI devices:

```bash
lspci
```

Show more detailed information:

```bash
lspci -v
```

Show kernel driver information:

```bash
lspci -k
```

The `-k` output is particularly useful when investigating which kernel driver is associated with a PCI device.

---

## 8. `lsscsi`

List devices associated with the SCSI layer:

```bash
lsscsi
```

This can help connect storage hardware with its SCSI-layer representation.

---

## 9. `dd`

Basic syntax:

```bash
dd if=SOURCE of=DESTINATION
```

Specify a block size:

```bash
dd if=SOURCE of=DESTINATION bs=1M
```

Specify the number of blocks:

```bash
dd if=SOURCE of=DESTINATION bs=1M count=10
```

Skip input blocks:

```bash
dd if=SOURCE of=DESTINATION bs=1M skip=10
```

Skip output blocks:

```bash
dd if=SOURCE of=DESTINATION bs=1M seek=10
```

### Example with a regular file

Create a small test file:

```bash
dd if=/dev/zero of=test.img bs=1M count=10
```

Inspect the result:

```bash
ls -lh test.img
```

### Important safety rule

Always verify:

```text
if=  → source
of=  → destination
```

Before executing `dd`, confirm:

```bash
ls -l SOURCE
ls -l DESTINATION
```

Never experiment with an unknown block device or real disk when a regular test file can be used instead.

---

## 10. Quick Reference

| Command            | Purpose                         |
| ------------------ | ------------------------------- |
| `ls -l /dev`       | inspect device nodes            |
| `ls -l /dev/disk/` | inspect persistent device links |
| `ls /sys`          | inspect sysfs                   |
| `ls /sys/devices`  | inspect kernel device hierarchy |
| `ls /sys/class`    | inspect device classes          |
| `ls /sys/bus`      | inspect buses                   |
| `udevadm info ...` | inspect udev device information |
| `udevadm monitor`  | monitor device events           |
| `lsusb`            | list USB devices                |
| `lspci`            | list PCI devices                |
| `lspci -k`         | show PCI kernel drivers         |
| `lsscsi`           | list SCSI-layer devices         |
| `dd`               | copy raw data streams           |

---

## Safety Checklist for `dd`

Before using `dd`:

1. Identify the input.
2. Identify the output.
3. Verify both paths.
4. Confirm the direction of the copy.
5. Confirm the block size and count.
6. Make sure the destination does not contain important data.
7. Prefer a regular test file for learning.
8. Only use real block devices when the operation is fully understood.

