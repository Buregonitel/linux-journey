# Linux Packages

This section covers how Linux software is distributed, packaged, stored in repositories, installed, upgraded, removed, and built from source code.

## Lessons Completed

1. **Software Distribution**

   * Understand how upstream source projects, distribution maintainers, packages, and package formats form a Linux software supply chain.

2. **Package Repositories**

   * Understand how repositories publish signed package indexes.
   * Understand how APT discovers configured package sources on Debian-based systems.

3. **tar and gzip**

   * Archive files with `tar`.
   * Compress streams with `gzip`.
   * Inspect archives before extracting them safely.

4. **Package Dependencies**

   * Understand how package metadata expresses required capabilities and versions.
   * Understand conflicts and relationships with shared libraries.

5. **rpm and dpkg**

   * Understand how `dpkg` and `rpm` inspect and modify their package databases.
   * Work with local package archives.

6. **yum and apt**

   * Understand repository-based package management with APT and DNF.
   * Check, install, remove, and update packages.

7. **Compiling Source Code**

   * Verify source code.
   * Configure, build, test, install, and track software compiled from source.

---

## Skills Practiced

* Understanding the Linux software supply chain
* Working with package repositories
* Understanding package indexes and repository metadata
* Creating and inspecting `tar` archives
* Compressing data with `gzip`
* Inspecting archive contents before extraction
* Understanding package dependencies
* Working with Debian packages using `dpkg`
* Working with RPM packages using `rpm`
* Managing packages with APT
* Understanding DNF/YUM package management
* Installing, removing, and updating packages
* Building software from source code

---

## Key Concepts

### Linux Software Distribution

A simplified software distribution chain looks like:

```text
Upstream Source
      ↓
Distribution Maintainer
      ↓
Package Build
      ↓
Package Repository
      ↓
Package Manager
      ↓
Installed System
```

Distributions package software so that it can be installed, updated, tracked, and removed consistently.

---

## Package Formats

Different Linux distribution families commonly use different package formats.

| Distribution family | Package format | Low-level tool | Repository manager |
| ------------------- | -------------- | -------------- | ------------------ |
| Debian / Ubuntu     | `.deb`         | `dpkg`         | `apt`              |
| RPM-based           | `.rpm`         | `rpm`          | `dnf` / `yum`      |

The package format and the package manager are related but serve different roles.

---

## Package Repositories

A package repository provides packages and metadata that package managers can use to discover available software.

APT uses configured sources to locate package repositories.

Common configuration locations include:

```text
/etc/apt/sources.list
/etc/apt/sources.list.d/
```

Repository metadata allows a package manager to determine:

* available packages;
* package versions;
* dependencies;
* package relationships;
* repository information.

---

## `tar` and `gzip`

`tar` creates archives containing multiple files and directories.

Example:

```bash
tar -cf archive.tar directory/
```

`gzip` compresses data.

A common combination is:

```bash
tar -czf archive.tar.gz directory/
```

Before extracting an archive, inspect its contents:

```bash
tar -tf archive.tar.gz
```

This is especially useful when working with archives from external sources.

---

## Package Dependencies

Packages can depend on:

* specific package versions;
* required capabilities;
* shared libraries;
* other packages.

They may also declare:

* conflicts;
* replacements;
* relationships with other packages.

Package managers use this metadata to resolve installation and upgrade requirements.

---

## `dpkg`

`dpkg` is a low-level package management tool for Debian packages.

It can inspect and modify the local package database and work directly with `.deb` files.

Example:

```bash
dpkg -l
```

---

## `rpm`

`rpm` is a low-level package management tool for RPM packages.

It can inspect installed packages and work with local `.rpm` archives.

Example:

```bash
rpm -qa
```

---

## APT

APT is a higher-level package management system for Debian-based distributions.

Typical operations include:

```bash
apt update
apt install package
apt remove package
apt upgrade
```

`apt` uses repository metadata to resolve packages and dependencies.

---

## DNF and YUM

DNF is a repository-based package manager commonly used by modern RPM-based distributions.

Typical operations include:

```bash
dnf install package
dnf remove package
dnf upgrade
```

`yum` is the traditional package management interface used by many RPM-based systems. On systems where it is available, it provides familiar repository-based package operations.

---

## Building from Source

Software can also be built directly from source code.

A simplified workflow is:

```text
Download source
      ↓
Verify source
      ↓
Configure
      ↓
Build
      ↓
Test
      ↓
Install
      ↓
Track installed files
```

A common build workflow may use tools such as:

```bash
./configure
make
make test
sudo make install
```

The exact process depends on the project's build system and documentation.

---

## What I Learned

After completing this section, I can:

* explain the Linux software distribution chain;
* distinguish package formats from package managers;
* understand the role of repositories and package indexes;
* create and inspect `tar` archives;
* use `gzip` for compression;
* inspect archives before extraction;
* understand package dependencies and conflicts;
* work with Debian packages using `dpkg`;
* work with RPM packages using `rpm`;
* manage packages with APT;
* understand DNF/YUM repository-based package management;
* install, remove, and update packages;
* describe the basic workflow for compiling software from source.

---

## Documentation

* [Commands Cheat Sheet](commands.md)
* [Practice Journal](practice.md)

