# Booting

This section covers the Linux boot process from platform firmware to the first user-space process.

The lessons explain how control moves through firmware, the bootloader, the Linux kernel, `initramfs`, the real root filesystem, and finally PID 1, which initializes and manages the user-space environment.

## Lessons Completed

1. **Boot Process Overview** — the main stages of the transition from platform firmware to the first user-space process.
2. **Boot Process: BIOS** — legacy BIOS and modern UEFI firmware and how they locate the next boot stage.
3. **Boot Process: Bootloader** — selecting Linux boot artifacts, constructing the kernel command line, and transferring control to the kernel.
4. **Boot Process: Kernel** — hardware initialization, early `initramfs` user space, reaching the real root filesystem, and starting PID 1.
5. **Boot Process: Init** — how PID 1 initializes user space, manages services, reaps child processes, and coordinates shutdown.

## Skills Practiced

* Understanding the Linux boot sequence
* Distinguishing firmware, bootloader, kernel, and user-space initialization
* Understanding BIOS and UEFI roles
* Understanding the purpose of a bootloader
* Reading the kernel command line
* Understanding `initramfs`
* Identifying PID 1
* Understanding the role of `init`/system initialization
* Inspecting boot-related information from a running Linux system
* Connecting process management with system startup and shutdown

## Boot Sequence

A simplified Linux boot flow is:

```text
Firmware
   ↓
BIOS / UEFI
   ↓
Bootloader
   ↓
Linux kernel + initramfs
   ↓
Kernel initialization
   ↓
Real root filesystem
   ↓
PID 1 / init system
   ↓
Services and user space
```

Each stage prepares the environment required by the next stage.

## 1. Firmware

The system begins with platform firmware.

Two important firmware models are:

* **BIOS** — the traditional firmware interface.
* **UEFI** — the modern firmware environment used on most current systems.

The firmware performs early hardware initialization and determines how to locate the next boot stage.

## 2. Bootloader

The bootloader takes control after firmware.

Its responsibilities can include:

* locating Linux boot files
* selecting a kernel
* selecting an `initramfs`
* constructing or modifying the kernel command line
* passing control to the Linux kernel

A common Linux bootloader is **GRUB**, although Linux systems can use other boot mechanisms.

## 3. Linux Kernel

The kernel is responsible for bringing the operating system environment to life.

During early initialization it:

* initializes hardware and kernel subsystems
* starts the early user space provided by `initramfs`
* prepares access to the real root filesystem
* transitions from the early environment to the real system
* starts the first user-space process

## 4. `initramfs`

`initramfs` provides a temporary early user-space environment.

It can contain the tools, modules, and configuration needed to make the real root filesystem available.

For example, the real root filesystem may require:

* storage drivers
* filesystem support
* RAID setup
* logical volume activation
* disk unlocking

The exact contents depend on the system configuration.

## 5. PID 1

After the kernel has established the real root environment, it starts the first user-space process.

This process receives:

```text
PID = 1
```

PID 1 is responsible for important system-level initialization tasks.

On many modern Linux distributions, PID 1 is `systemd`.

Inspect it with:

```bash
ps -p 1 -f
```

or:

```bash
readlink /proc/1/exe
```

## 6. Init System

The init system coordinates the running user-space environment.

Typical responsibilities include:

* starting services
* managing service dependencies
* supervising processes
* collecting terminated child processes
* coordinating system shutdown

The exact implementation depends on the Linux distribution and its init system.

## Key Concepts

| Concept         | Role                                         |
| --------------- | -------------------------------------------- |
| BIOS            | Legacy platform firmware                     |
| UEFI            | Modern platform firmware                     |
| Bootloader      | Selects boot artifacts and starts the kernel |
| Kernel          | Initializes the operating system             |
| `initramfs`     | Temporary early user space                   |
| Root filesystem | Main filesystem used by the running system   |
| PID 1           | First user-space process                     |
| Init system     | Initializes and manages user space           |

## What I Learned

* Linux booting is a chain of control transfers rather than a single operation.
* Firmware prepares the platform and locates the next boot stage.
* The bootloader selects the kernel and related boot artifacts.
* The kernel initializes hardware and uses `initramfs` when early user space is required.
* The system eventually switches to the real root filesystem.
* PID 1 becomes the first user-space process.
* The init system manages services, processes, and system shutdown.

## Documentation

* [Commands Cheat Sheet](commands.md)
* [Practice Journal](practice.md)

