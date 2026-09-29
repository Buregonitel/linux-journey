# Kernel - Commands Cheat Sheet

A practical reference for inspecting the Linux kernel, system calls, kernel files, and loadable modules.

---

## 1. Kernel Information

Show the running kernel release:

```bash id="y6c7m0"
uname -r
```

Show general kernel/system information:

```bash id="a0c3se"
uname -a
```

Show the kernel command line:

```bash id="3n9z0r"
cat /proc/cmdline
```

Show kernel version information:

```bash id="3g9u1f"
cat /proc/version
```

---

## 2. User Space vs Kernel Space

A useful conceptual model:

```text id="r7c1hi"
Application
    ↓
Library
    ↓
System call
    ↓
Kernel
    ↓
Hardware / kernel resource
```

Applications normally execute without direct access to privileged CPU operations.

---

## 3. `strace`

Trace a program's system calls:

```bash id="q5zzxs"
strace command
```

Example:

```bash id="e8f74p"
strace cat file.txt
```

Trace only selected system-call categories:

```bash id="f53zpt"
strace -e trace=file cat file.txt
```

Trace process-related calls:

```bash id="d5ty3d"
strace -e trace=process command
```

Write the trace to a file:

```bash id="c4awp9"
strace -o trace.txt command
```

Inspect the trace:

```bash id="h5eh7v"
less trace.txt
```

### Common calls you may encounter

```text id="xj9h14"
openat()   → open a file/path
read()     → read data
write()    → write data
close()    → close a file descriptor
execve()   → execute a program
fork()/clone() → create a process/thread
exit()     → terminate a process
```

The exact calls depend on the program and its environment.

---

## 4. Kernel Locations

Inspect `/boot`:

```bash id="c8d4br"
ls -lh /boot
```

Inspect version-specific modules:

```bash id="zn6p8x"
ls -lh /lib/modules/$(uname -r)
```

Show the current kernel's module directory:

```bash id="9o7qf6"
readlink -f /lib/modules/$(uname -r)
```

Useful subdirectories may include:

```text id="kccv26"
kernel/
modules.*
```

The exact structure depends on the distribution.

---

## 5. Kernel Configuration

Some systems expose the running kernel configuration through:

```bash id="y7kz17"
cat /proc/config.gz
```

if available.

Another common location is:

```bash id="85gdvf"
/boot/config-$(uname -r)
```

Inspect it with:

```bash id="nq4i9k"
less /boot/config-$(uname -r)
```

Search for a configuration option:

```bash id="m3r1n4"
grep '^CONFIG_' /boot/config-$(uname -r) | head
```

---

## 6. Kernel Modules

List currently loaded modules:

```bash id="q8gq8a"
lsmod
```

Show information about a module:

```bash id="9paf3v"
modinfo MODULE
```

Find a module file:

```bash id="73gjj5"
modinfo -n MODULE
```

Load a module:

```bash id="i6g3qv"
sudo modprobe MODULE
```

Remove a module:

```bash id="cr0yr7"
sudo modprobe -r MODULE
```

Show module dependencies:

```bash id="b2f7m3"
modinfo MODULE
```

Search available modules:

```bash id="f0k7xz"
find /lib/modules/$(uname -r) -type f
```

---

## 7. Module Parameters

Inspect module information:

```bash id="2v4w1q"
modinfo MODULE
```

Look for parameters:

```text id="42i2g5"
parm:
```

A module can sometimes be loaded with parameters:

```bash id="8v5y6u"
sudo modprobe MODULE parameter=value
```

Do not experiment with unknown module parameters on a production system.

---

## 8. Module Dependencies

`modprobe` handles module dependencies automatically.

Load:

```bash id="2gk3dz"
sudo modprobe MODULE
```

Remove:

```bash id="u3e8fa"
sudo modprobe -r MODULE
```

Inspect the currently loaded module set:

```bash id="4lkvw2"
lsmod
```

If a module is required by another loaded module, removal may fail until its dependents are handled.

---

## 9. Inspect Kernel Messages

Display kernel messages:

```bash id="7d4czn"
dmesg
```

Show recent messages:

```bash id="i5x2h0"
dmesg | tail -n 30
```

Search for module-related messages:

```bash id="av9kz5"
dmesg | grep -i module
```

On `systemd` systems:

```bash id="y4hr7a"
journalctl -b -k
```

---

## 10. Verify the Running Kernel

After booting a kernel:

```bash id="f7r8m1"
uname -r
```

Compare the result with installed module directories:

```bash id="zv6t2p"
ls /lib/modules/
```

Check that the directory for the running release exists:

```bash id="0q8k3m"
ls -ld /lib/modules/$(uname -r)
```

This helps confirm that the running kernel has its corresponding module tree available.

---

## Quick Reference

| Command                        | Purpose                          |
| ------------------------------ | -------------------------------- |
| `uname -r`                     | show running kernel release      |
| `uname -a`                     | show kernel/system information   |
| `cat /proc/cmdline`            | show kernel boot parameters      |
| `cat /proc/version`            | show kernel version information  |
| `strace command`               | trace system calls               |
| `strace -e trace=file command` | trace file-related calls         |
| `strace -o file command`       | save trace to a file             |
| `ls -lh /boot`                 | inspect kernel/boot artifacts    |
| `ls /lib/modules/$(uname -r)`  | inspect current module tree      |
| `lsmod`                        | list loaded modules              |
| `modinfo MODULE`               | show module metadata             |
| `modprobe MODULE`              | load a module                    |
| `modprobe -r MODULE`           | remove a module                  |
| `dmesg`                        | inspect kernel messages          |
| `journalctl -b -k`             | inspect current-boot kernel logs |

---

## Safety Notes

### Kernel installation

Keep a known-good kernel available before testing a newly installed one.

### Module management

Before removing a module, check whether it is currently required:

```bash
lsmod
```

Inspect its metadata:

```bash
modinfo MODULE
```

### `strace`

`strace` is generally safe for observation, but tracing a program can change its timing and behavior slightly.

### Never assume a kernel path

Kernel filenames and bootloader layouts vary between distributions. Prefer inspecting the actual system:

```bash
uname -r
ls -lh /boot
ls /lib/modules/
```

