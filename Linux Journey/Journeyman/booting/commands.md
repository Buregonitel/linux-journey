# Booting - Commands Cheat Sheet

A practical reference for inspecting the Linux boot environment, kernel command line, PID 1, and boot-related information.

---

## 1. Inspect the Running Kernel

Show the kernel release:

```bash
uname -r
```

Show general system information:

```bash
uname -a
```

Show the kernel command line:

```bash
cat /proc/cmdline
```

The command line contains parameters passed to the kernel during boot.

---

## 2. Inspect PID 1

Show PID 1:

```bash
ps -p 1 -f
```

Inspect the executable behind PID 1:

```bash
readlink /proc/1/exe
```

Inspect PID 1 status:

```bash
cat /proc/1/status
```

Useful fields include:

```text
Name
Pid
PPid
Uid
Gid
State
```

---

## 3. Inspect the Process Tree

Show a process hierarchy:

```bash
ps -ef --forest
```

This helps visualize parent-child relationships after the init system starts user space.

A simplified model:

```text
PID 1
├── service
├── service
└── process
```

---

## 4. Inspect `/proc`

The `/proc` filesystem exposes information about the running kernel and processes.

Useful boot-related paths:

```bash
cat /proc/cmdline
cat /proc/version
```

Inspect PID 1:

```bash
ls -l /proc/1
```

Inspect the executable:

```bash
readlink /proc/1/exe
```

---

## 5. Inspect Kernel Messages

Display kernel messages:

```bash
dmesg
```

Show recent messages:

```bash
dmesg | tail
```

Search for a topic:

```bash
dmesg | grep -i "firmware"
```

or:

```bash
dmesg | grep -i "mount"
```

On some systems, access to kernel messages may require elevated privileges.

---

## 6. Inspect Boot Logs with `journalctl`

On systems using `systemd`, view the current boot:

```bash
journalctl -b
```

Show kernel messages from the current boot:

```bash
journalctl -b -k
```

Show the previous boot:

```bash
journalctl -b -1
```

Show messages since boot:

```bash
journalctl -b --no-pager
```

Follow new log messages:

```bash
journalctl -f
```

---

## 7. Inspect `systemd`

If the system uses `systemd`, inspect its status:

```bash
systemctl status
```

Show the default target:

```bash
systemctl get-default
```

List active units:

```bash
systemctl list-units
```

List services:

```bash
systemctl list-units --type=service
```

Show the boot target:

```bash
systemctl list-units --type=target
```

---

## 8. Inspect Bootloader Configuration

On systems using GRUB, configuration may be generated from files under:

```text
/etc/default/grub
/etc/grub.d/
```

Inspect the main configuration source:

```bash
cat /etc/default/grub
```

Do not modify bootloader configuration merely for practice. Incorrect changes can affect the next boot.

---

## 9. Inspect `initramfs`

List common initramfs-related files:

```bash
ls -lh /boot
```

Look for initramfs images:

```bash
ls -lh /boot/*initramfs*
```

or:

```bash
ls -lh /boot/*initrd*
```

The exact filenames depend on the distribution.

---

## 10. Useful Boot Investigation Workflow

A basic diagnostic workflow:

```bash
uname -r
cat /proc/cmdline
ps -p 1 -f
readlink /proc/1/exe
journalctl -b -k
```

Then inspect the process hierarchy:

```bash
ps -ef --forest
```

This connects several stages of the boot process with the state of the currently running system.

---

## Quick Reference

| Command                 | Purpose                               |
| ----------------------- | ------------------------------------- |
| `uname -r`              | show kernel release                   |
| `uname -a`              | show kernel/system information        |
| `cat /proc/cmdline`     | show kernel command line              |
| `ps -p 1 -f`            | inspect PID 1                         |
| `readlink /proc/1/exe`  | identify PID 1 executable             |
| `cat /proc/1/status`    | inspect PID 1 status                  |
| `ps -ef --forest`       | show process hierarchy                |
| `dmesg`                 | show kernel messages                  |
| `journalctl -b`         | show logs for current boot            |
| `journalctl -b -k`      | show kernel messages for current boot |
| `systemctl status`      | inspect systemd state                 |
| `systemctl get-default` | show default systemd target           |
| `ls -lh /boot`          | inspect boot files                    |

