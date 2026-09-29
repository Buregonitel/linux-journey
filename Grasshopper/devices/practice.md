# Devices - Practice Journal

Hands-on practice for understanding Linux device nodes, hardware discovery, `sysfs`, `udev`, and safe low-level copying.

---

## 1. `/dev` Directory

### Goal

Explore how Linux exposes devices through the `/dev` filesystem.

### Practice

List the contents of `/dev`:

```bash
ls -l /dev
```

Inspect several entries:

```bash
ls -l /dev/null
ls -l /dev/tty
ls -l /dev/zero
```

Look at the first character in the permissions column.

### Result

Example:

```text
The listing showed special device nodes with different object types.
```

### What I learned

`/dev` provides user-space interfaces to many kernel-managed devices and pseudo-devices.

---

## 2. Device Types

### Goal

Distinguish character devices, block devices, pipes, sockets, and regular files.

### Practice

Inspect several filesystem objects:

```bash
ls -l /dev/null
ls -l /dev/sda
```

Compare their type indicators with a regular file:

```bash
touch regular-file
ls -l regular-file
```

### Result

Example:

```text
Different filesystem object types were represented by different type indicators.
```

### What I learned

Linux uses different object types to represent different access and communication mechanisms.

---

## 3. Device Names

### Goal

Recognize common Linux device naming conventions.

### Practice

Inspect available device links:

```bash
ls -l /dev/disk/
ls -l /dev/disk/by-id/
ls -l /dev/disk/by-uuid/
```

If the system has NVMe storage, inspect:

```bash
ls -l /dev/nvme*
```

### Result

Example:

```text
The system exposed device names for storage devices, partitions, and persistent references.
```

### What I learned

Device names can represent physical devices, partitions, logical devices, or persistent references.

---

## 4. sysfs

### Goal

Explore the kernel's live device model through `/sys`.

### Practice

Start with the main hierarchy:

```bash
ls /sys
```

Inspect several important directories:

```bash
ls /sys/devices
ls /sys/class
ls /sys/bus
ls /sys/block
```

Compare `/dev` and `/sys`:

```text
/dev → device access interfaces
/sys → kernel device model and relationships
```

### Result

Example:

```text
The /sys hierarchy exposed information about devices, buses, classes, and kernel components.
```

### What I learned

`sysfs` provides a structured view of how the kernel represents hardware and device relationships.

---

## 5. udev

### Goal

Understand how device events are handled in user space.

### Practice

Inspect information for a known device:

```bash
udevadm info --query=all --name=/dev/sda
```

Monitor device events:

```bash
udevadm monitor
```

If working in a lab environment where devices can safely be connected or disconnected, observe the generated events.

Stop monitoring with:

```text
Ctrl-C
```

### Result

Example:

```text
udevadm displayed device metadata and the monitor showed kernel/udev device events.
```

### What I learned

`udev` processes device events and applies rules for device management, permissions, and persistent naming.

---

## 6. Hardware Discovery

### Goal

Inspect different parts of the system hardware topology.

### Practice

USB devices:

```bash
lsusb
```

PCI devices:

```bash
lspci
```

PCI devices and associated kernel drivers:

```bash
lspci -k
```

SCSI-layer devices:

```bash
lsscsi
```

### Result

Example:

```text
The commands provided different views of USB, PCI, and SCSI-layer hardware.
```

### What I learned

No single hardware listing command provides the complete picture. Different tools expose different layers of the Linux device model.

---

## 7. `dd`

### Goal

Practice low-level copying without modifying a real disk.

### Practice

Create a small test image:

```bash
dd if=/dev/zero of=test.img bs=1M count=10
```

Check the file:

```bash
ls -lh test.img
```

Copy it:

```bash
dd if=test.img of=test-copy.img bs=1M
```

Compare the files:

```bash
ls -lh test.img test-copy.img
```

Optionally verify that they have the same checksum:

```bash
sha256sum test.img test-copy.img
```

Clean up:

```bash
rm test.img test-copy.img
```

### Result

Example:

```text
A regular file was used as the source and destination, allowing dd to be practiced without modifying a real block device.
```

### What I learned

`dd` works at a low level and can copy raw data. The `if=` and `of=` parameters must always be checked carefully.

---

# Mini Exercise

## Goal

Combine device inspection, hardware discovery, and safe `dd` usage.

### Tasks

### 1. Inspect `/dev`

```bash
ls -l /dev
```

Identify several character and block devices.

### 2. Explore persistent links

```bash
ls -l /dev/disk/by-id/
ls -l /dev/disk/by-uuid/
```

### 3. Inspect sysfs

```bash
ls /sys/devices
ls /sys/class
ls /sys/bus
```

### 4. Discover hardware

Run:

```bash
lsusb
lspci
lspci -k
lsscsi
```

### 5. Inspect udev information

Choose a safe device path and run:

```bash
udevadm info --query=all --name=/dev/DEVICE
```

Replace `DEVICE` with an actual device available in the lab.

### 6. Practice `dd` safely

Create a test image:

```bash
dd if=/dev/zero of=device-test.img bs=1M count=5
```

Create a copy:

```bash
dd if=device-test.img of=device-test-copy.img bs=1M
```

Verify:

```bash
sha256sum device-test.img device-test-copy.img
```

Clean up:

```bash
rm device-test.img device-test-copy.img
```

## Final Reflection

After completing the exercise, document:

* Which device types you identified in `/dev`
* Which persistent device links were available
* What information you found in `/sys`
* Which hardware was visible through `lsusb`, `lspci`, and `lsscsi`
* What `udevadm` showed about a selected device
* Why `dd` requires special care when working with block devices

## Key Takeaway

Linux devices are represented through several connected layers:

```text
Hardware
   ↓
Kernel device model
   ↓
sysfs (/sys)
   ↓
Kernel device events
   ↓
udev
   ↓
Device nodes (/dev)
   ↓
User-space applications
```

Understanding these layers makes it easier to investigate hardware, drivers, device names, and low-level storage operations.

