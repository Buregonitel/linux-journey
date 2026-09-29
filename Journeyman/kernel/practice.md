# Kernel - Practice Journal

Hands-on practice for understanding the Linux kernel, system calls, kernel artifacts, and loadable modules.

> **Note:** The examples below are practice templates. Replace the example observations with your own results when documenting actual lab work.

---

## 1. Kernel Overview

### Goal

Understand the kernel's position between user-space applications and hardware.

### Practice

Inspect the running kernel:

```bash id="e3z2am"
uname -r
```

Inspect general information:

```bash id="11s9tr"
uname -a
```

Inspect the kernel command line:

```bash id="r2fk0s"
cat /proc/cmdline
```

### Result

Example:

```text id="2mny2n"
The system reported a running Linux kernel and exposed its boot parameters through /proc/cmdline.
```

### What I learned

The kernel is the privileged core that manages system resources and provides controlled services to user-space programs.

---

## 2. Privilege Levels

### Goal

Understand why Linux separates normal application execution from privileged kernel execution.

### Practice

Use `strace` to observe a normal user-space command:

```bash id="9v12l5"
strace echo "Hello"
```

Observe that the application interacts with the kernel through system calls rather than directly controlling hardware.

### Result

Example:

```text id="0xun1p"
The trace showed system calls used by the program to interact with the operating system.
```

### What I learned

Privilege separation allows applications to run with fewer privileges while the kernel performs protected operations on their behalf.

---

## 3. System Calls

### Goal

Observe how a user-space program requests kernel services.

### Practice

Create a small file:

```bash id="4u8t7w"
printf 'Linux kernel practice\n' > test.txt
```

Trace a command that reads it:

```bash id="ppk7w0"
strace cat test.txt
```

Focus on calls such as:

```text id="x9z8bi"
openat()
read()
write()
close()
```

A more focused trace:

```bash id="m7d5cx"
strace -e trace=file cat test.txt
```

Clean up:

```bash id="b2r3b7"
rm test.txt
```

### Result

Example:

```text id="h2x2fa"
The trace exposed file-related system calls used while reading the test file.
```

### What I learned

System calls form the controlled interface between user-space programs and kernel services.

---

## 4. Kernel Installation

### Goal

Understand the safe workflow for installing and verifying a distribution kernel.

### Practice

First inspect the current kernel:

```bash id="t8u3zq"
uname -r
```

Inspect available kernel files:

```bash id="7j3p1q"
ls -lh /boot
```

Inspect installed module trees:

```bash id="q1f0kp"
ls /lib/modules/
```

Document the current working kernel before making any changes:

```text id="m8k9zt"
Current kernel: <your result>
Available kernels/modules: <your result>
```

If working in a dedicated disposable lab environment, install a distribution kernel using the package manager appropriate for that system.

After installation, verify that the new kernel and its module tree are present.

Do not remove the currently working kernel during this exercise.

### Result

Example:

```text id="0b4jcb"
The installed kernel artifacts were visible in /boot and the corresponding module tree was available under /lib/modules/.
```

### What I learned

Kernel installation should preserve a known-good fallback. Verification should happen before relying on the new kernel for normal operation.

---

## 5. Kernel Location

### Goal

Find the important files associated with the running kernel.

### Practice

Identify the running release:

```bash id="3p9q8c"
uname -r
```

Inspect `/boot`:

```bash id="y1n2h5"
ls -lh /boot
```

Inspect the current module tree:

```bash id="2e5q6m"
ls -lh /lib/modules/$(uname -r)
```

Check the kernel configuration if available:

```bash id="9a6k2j"
ls -l /boot/config-$(uname -r)
```

### Result

Example:

```text id="tx4b2h"
The system contained kernel-related files in /boot and release-specific modules under /lib/modules/.
```

### What I learned

Kernel artifacts are distributed across several locations. The exact layout depends on the Linux distribution.

---

## 6. Kernel Modules

### Goal

Inspect and safely work with loadable kernel modules.

### Practice

List loaded modules:

```bash id="g0q6t1"
lsmod
```

Choose a harmless module that is already loaded and inspect its metadata:

```bash id="w8e1j3"
modinfo MODULE
```

Find the module file:

```bash id="z8q7ma"
modinfo -n MODULE
```

If a dedicated lab environment provides a safe module for testing, load it:

```bash id="v3b7u2"
sudo modprobe MODULE
```

Verify:

```bash id="w3p9kn"
lsmod | grep MODULE
```

If it is safe to remove:

```bash id="4n1f8s"
sudo modprobe -r MODULE
```

Verify again:

```bash id="j6q4n0"
lsmod | grep MODULE
```

### Result

Example:

```text id="j0y5p9"
The module metadata identified its file and dependencies. A lab-safe module could be loaded and removed using modprobe.
```

### What I learned

Kernel modules extend the running kernel and can often be managed without rebuilding the entire kernel.

---

# Mini Exercise

## Goal

Investigate the running kernel from several perspectives.

### Step 1 - Identify the Kernel

```bash id="f3k7d2"
uname -r
```

Record:

```text id="u0tq7p"
Running kernel: <your result>
```

### Step 2 - Locate Kernel Artifacts

```bash id="y5g1mc"
ls -lh /boot
```

Identify:

* kernel image
* initramfs/initrd
* configuration file
* other boot-related artifacts

### Step 3 - Inspect Modules

```bash id="c8v5z1"
lsmod | head
```

Choose one module from the list:

```bash id="q9j4x7"
modinfo MODULE
```

Record:

```text id="0t9yka"
Module: <your result>
Description: <your result>
Filename: <your result>
```

### Step 4 - Trace a System Call

Create a small file:

```bash id="n6r8w2"
printf 'kernel\n' > syscall-test.txt
```

Trace the file operation:

```bash id="s4h7p3"
strace -e trace=file cat syscall-test.txt
```

Clean up:

```bash id="j1k6v8"
rm syscall-test.txt
```

### Step 5 - Verify PID and Kernel Context

Inspect PID 1:

```bash id="r3m9q1"
ps -p 1 -f
```

Compare the running kernel with the module tree:

```bash id="z7x2c4"
ls -ld /lib/modules/$(uname -r)
```

### Step 6 - Document the Architecture

Complete:

```text id="v4n8s2"
User-space application
        ↓
System call
        ↓
Linux kernel
        ↓
Kernel subsystem / driver
        ↓
Hardware or kernel-managed resource
```

Add one concrete example from your `strace` output.

## Final Reflection

Document:

* What responsibilities belong to the kernel
* Why user space and kernel space are separated
* Which system calls you observed
* Where the running kernel's files are located
* Which modules were loaded
* What information `modinfo` provided
* Why a fallback kernel should be preserved during kernel updates

## Key Takeaway

The Linux kernel connects user-space software with protected system resources:

```text id="j7c0d3"
Applications
     ↓
System calls
     ↓
Kernel
 ┌───┼───────────────┐
 ↓   ↓               ↓
CPU Memory       Devices
 ↓   ↓               ↓
Processes Filesystems Drivers
```

Understanding this boundary makes the relationship between processes, devices, filesystems, system calls, and kernel modules much easier to reason about.

