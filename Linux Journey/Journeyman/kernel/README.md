# Kernel

This section covers the Linux kernel as the core layer between user-space programs and system hardware.

The lessons focus on kernel responsibilities, CPU privilege levels, system calls, kernel installation and verification, kernel file locations, and loadable kernel modules.

## Lessons Completed

1. **Kernel Overview** - how the Linux kernel mediates access to hardware, resources, isolation, and user-space requests.
2. **Privilege Levels** - how processor privilege levels separate user-space execution from trusted kernel execution.
3. **System Calls** - how user-space programs request kernel services and how `strace` can safely expose those interactions.
4. **Kernel Installation** - installing, booting, and verifying a distribution kernel while keeping a tested fallback available.
5. **Kernel Location** - locating kernel images, `initramfs`, configuration, symbols, and version-specific modules.
6. **Kernel Modules** - inspecting, loading, configuring, and safely removing release-specific Linux kernel modules.

## Skills Practiced

* Understanding the role of the Linux kernel
* Distinguishing user space from kernel space
* Understanding CPU privilege levels
* Understanding the purpose of system calls
* Using `strace` to observe system-call activity
* Inspecting the running kernel
* Locating kernel images and related files
* Understanding `initramfs` and kernel modules
* Inspecting loaded kernel modules
* Loading and removing kernel modules safely
* Verifying a kernel installation before relying on it
* Maintaining a tested fallback kernel

## What the Kernel Does

The kernel is the central privileged component of a Linux system.

It provides controlled access to:

* CPU resources
* memory
* storage
* devices
* networking
* processes
* filesystems
* inter-process communication

User-space applications normally do not access hardware directly. Instead, they request services from the kernel through defined interfaces such as system calls.

A simplified model is:

```text id="0p0xj4"
User-space application
        ↓
    System call
        ↓
   Linux kernel
        ↓
Hardware / kernel-managed resources
```

## User Space and Kernel Space

Linux separates normal application execution from privileged kernel execution.

```text id="d7qf0a"
User Space
────────────────────────
Applications
Shells
Libraries
Services
────────────────────────
        ↓ system calls
────────────────────────
Kernel Space
────────────────────────
Kernel
Drivers
Memory management
Process management
Filesystems
Networking
────────────────────────
        ↓
     Hardware
```

This separation limits direct access to privileged operations and helps the kernel enforce resource and security boundaries.

## System Calls

System calls are the controlled interface through which user-space programs request kernel services.

Examples include operations related to:

* files
* processes
* memory
* networking
* signals

A simple program such as:

```bash id="iyr7sj"
cat file.txt
```

ultimately relies on kernel services to open, read, and close the file.

`strace` can make these interactions visible:

```bash id="3h1mqr"
strace cat file.txt
```

This is useful for understanding what a program asks the kernel to do.

## Kernel Installation

Kernel installation is different from installing an ordinary user-space package because the kernel participates directly in system startup.

A safe workflow includes:

1. Install the distribution kernel.
2. Verify the installed files.
3. Ensure the bootloader can select it.
4. Keep a known-good kernel available.
5. Boot the new kernel.
6. Verify the running version.

The fallback kernel is important because a newly installed kernel may fail to boot or may expose hardware/configuration problems.

## Kernel Location

Common kernel-related locations include:

```text id="0x4j6b"
/boot
/lib/modules/<kernel-release>
/proc
/sys
```

Typical `/boot` contents can include:

* kernel images
* `initramfs`/`initrd` images
* bootloader-related files
* configuration or metadata

Version-specific modules are commonly stored under:

```text id="wq1n1v"
/lib/modules/<kernel-release>/
```

The exact filenames and directory contents depend on the Linux distribution.

## Kernel Modules

Kernel modules extend the running kernel without requiring every feature to be permanently built into the kernel image.

Modules are commonly used for:

* hardware drivers
* filesystems
* networking features
* other optional kernel functionality

Useful commands include:

```bash id="86x7ot"
lsmod
modinfo MODULE
modprobe MODULE
modprobe -r MODULE
```

`modprobe` is generally preferred for managing modules because it understands module dependencies.

## What I Learned

* The Linux kernel is the privileged core that mediates access to hardware and system resources.
* User-space programs interact with the kernel through system calls.
* CPU privilege levels help separate unprivileged application execution from trusted kernel execution.
* `strace` can expose system-call activity without modifying the application.
* Kernel installation requires more care than ordinary package installation because the kernel participates in the boot process.
* Kernel images and related artifacts are commonly stored under `/boot`.
* Release-specific modules are commonly stored under `/lib/modules/<kernel-release>`.
* Loadable modules allow kernel functionality to be added and removed dynamically.
* A tested fallback kernel is an important part of a safe kernel update workflow.

## Documentation

* [Commands Cheat Sheet](commands.md)
* [Practice Journal](practice.md)

