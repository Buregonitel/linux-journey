# Booting - Practice Journal

Hands-on practice for understanding the Linux boot sequence from firmware to PID 1.

---

## 1. Boot Process Overview

### Goal

Understand the major stages of a Linux boot.

### Practice

Write the boot sequence in your own words:

```text
Firmware
   ↓
Bootloader
   ↓
Kernel + initramfs
   ↓
Real root filesystem
   ↓
PID 1
   ↓
Services and user space
```

Inspect the running kernel:

```bash
uname -r
```

Inspect PID 1:

```bash
ps -p 1 -f
```

### Result

Example:

```text
The running system showed a Linux kernel and a dedicated PID 1 process responsible for system initialization.
```

### What I learned

Booting is a sequence of stages in which each component prepares the environment for the next one.

---

## 2. BIOS and UEFI

### Goal

Understand the role of firmware before the Linux kernel starts.

### Practice

Inspect the firmware interface available on the system:

```bash
ls /sys/firmware
```

On UEFI systems, this commonly includes:

```text
efi/
```

You can also inspect:

```bash
ls /sys/firmware/efi
```

if the directory exists.

### Result

Example:

```text
The system exposed firmware information through /sys/firmware.
```

### What I learned

Firmware performs early platform initialization and provides the mechanism through which the next boot stage is located.

---

## 3. Bootloader

### Goal

Understand what happens between firmware and the Linux kernel.

### Practice

Inspect the boot directory:

```bash
ls -lh /boot
```

Look for:

* kernel images
* initramfs/initrd images
* bootloader-related files

Inspect possible GRUB configuration sources:

```bash
ls -l /etc/default/grub
ls -l /etc/grub.d/
```

If available:

```bash
cat /etc/default/grub
```

### Result

Example:

```text
The /boot directory contained files used during the transition from the bootloader to the Linux kernel.
```

### What I learned

The bootloader selects boot artifacts and passes control to the kernel together with the required kernel parameters.

---

## 4. Linux Kernel

### Goal

Investigate information about the running kernel and its boot parameters.

### Practice

Show the kernel version:

```bash
uname -r
```

Show the complete kernel/system information:

```bash
uname -a
```

Inspect the kernel command line:

```bash
cat /proc/cmdline
```

Inspect recent kernel messages:

```bash
dmesg | tail -n 30
```

### Result

Example:

```text
The kernel command line showed parameters passed to the kernel during boot, while dmesg exposed kernel initialization messages.
```

### What I learned

The kernel receives boot parameters, initializes hardware and kernel subsystems, and prepares the system for user-space initialization.

---

## 5. `initramfs`

### Goal

Understand the role of the temporary early user-space environment.

### Practice

Inspect boot files:

```bash
ls -lh /boot
```

Look for files containing names such as:

```text
initramfs
initrd
```

Inspect the kernel command line for root-related information:

```bash
cat /proc/cmdline
```

### Result

Example:

```text
The boot directory contained an initramfs/initrd image associated with the installed kernel.
```

### What I learned

`initramfs` provides an early user-space environment that can prepare storage and other dependencies before the real root filesystem becomes available.

---

## 6. PID 1 and Init

### Goal

Identify the first user-space process and investigate its role.

### Practice

Inspect PID 1:

```bash
ps -p 1 -f
```

Identify its executable:

```bash
readlink /proc/1/exe
```

Inspect its status:

```bash
cat /proc/1/status
```

If the system uses `systemd`:

```bash
systemctl status
```

### Result

Example:

```text
PID 1 was identified as the system's init process. The executable revealed which init implementation was running.
```

### What I learned

PID 1 has a special role in user space. It initializes the system, manages services, and participates in process lifecycle management.

---

# Mini Exercise

## Goal

Trace the boot process using information available from the running system.

### Step 1 - Kernel

```bash
uname -r
```

Record:

```text
Kernel version: <your result>
```

### Step 2 - Kernel Command Line

```bash
cat /proc/cmdline
```

Identify several parameters and describe what they appear to control.

### Step 3 - Boot Files

```bash
ls -lh /boot
```

Identify:

* kernel image(s)
* initramfs/initrd image(s)
* other boot-related files

### Step 4 - PID 1

```bash
ps -p 1 -f
readlink /proc/1/exe
```

Record:

```text
PID 1: <your result>
Init implementation: <your result>
```

### Step 5 - Process Hierarchy

```bash
ps -ef --forest
```

Find PID 1 and inspect some of its child processes.

### Step 6 - Boot Logs

If available:

```bash
journalctl -b -k
```

Look for messages related to:

* hardware initialization
* storage
* filesystem mounting
* drivers
* kernel startup

### Step 7 - Build the Sequence

Complete the following diagram using your observations:

```text
Firmware
   ↓
<bootloader>
   ↓
<kernel>
   ↓
<initramfs / early user space>
   ↓
<real root filesystem>
   ↓
<PID 1>
   ↓
<services>
   ↓
<user space>
```

## Final Reflection

Document:

* Which firmware interface the system appears to use
* Which kernel is currently running
* What parameters were passed through `/proc/cmdline`
* Which boot artifacts were present in `/boot`
* Which process is PID 1
* Which init system is running
* What boot information was visible in the kernel/system logs

## Key Takeaway

The Linux boot process can be understood as a chain of responsibilities:

```text
Firmware
   ↓
Find and start the next boot stage

Bootloader
   ↓
Select kernel + initramfs + kernel parameters

Kernel
   ↓
Initialize hardware and early user space

initramfs
   ↓
Prepare access to the real root filesystem

PID 1
   ↓
Initialize and manage the running user space

Services
   ↓
Provide the system's normal functionality
```

This model provides a foundation for troubleshooting boot failures because each stage has a distinct role and produces different kinds of evidence.

