# Linux Packages - Practice Journal

Hands-on practice for understanding Linux software distribution, archives, repositories, package databases, package managers, dependencies, and source compilation.

---

## 1. Software Distribution

### Goal

Understand how software moves from an upstream project to an installed Linux package.

### Practice

Inspect installed packages using the package tools available in the lab.

On Debian-based systems:

```bash
dpkg -l | head
```

On RPM-based systems:

```bash
rpm -qa | head
```

Identify:

* package name;
* package version;
* package format;
* package manager.

### Result

Record:

```text
Distribution:
Package format:
Package manager:
Example package:
```

### What I learned

Linux distributions package software so that it can be installed, updated, tracked, and removed using standardized package-management tools.

---

## 2. Package Repositories

### Goal

Understand how package repositories provide metadata and packages to package managers.

### Practice

On a Debian-based system, inspect configured APT sources:

```bash
cat /etc/apt/sources.list
```

If the file is not present, inspect:

```bash
ls /etc/apt/sources.list.d/
```

Refresh package metadata:

```bash
sudo apt update
```

Observe the repository information retrieved by APT.

### Result

Record:

* configured repository locations;
* whether the package index update completed successfully;
* any warnings or errors.

### What I learned

Repository metadata allows APT to discover available packages and their versions and relationships.

---

## 3. `tar` and `gzip`

### Goal

Create, inspect, compress, and safely extract an archive.

### Practice

Create a test directory:

```bash
mkdir archive-lab
touch archive-lab/file1.txt
touch archive-lab/file2.txt
```

Create a tar archive:

```bash
tar -cf archive.tar archive-lab/
```

List its contents:

```bash
tar -tf archive.tar
```

Create a gzip-compressed archive:

```bash
tar -czf archive.tar.gz archive-lab/
```

Inspect it before extraction:

```bash
tar -tzf archive.tar.gz
```

Test the gzip stream:

```bash
gzip -t archive.tar.gz
```

### Result

Record:

```text
Archive:
Compressed archive:
Number of files:
Archive contents verified:
```

### What I learned

`tar` packages multiple filesystem objects into an archive, while `gzip` provides compression. Inspecting an archive before extraction helps verify its contents.

---

## 4. Package Dependencies

### Goal

Understand how package metadata expresses dependencies, versions, and conflicts.

### Practice

On Debian-based systems, inspect package metadata:

```bash
apt show package-name
```

Look for fields such as:

```text
Depends:
Recommends:
Suggests:
Conflicts:
```

On RPM-based systems, inspect package information:

```bash
rpm -qi package-name
```

Inspect requirements where available:

```bash
rpm -qR package-name
```

### Result

Record:

```text
Package:
Version:
Dependencies:
Conflicts:
Other relationships:
```

### What I learned

Package metadata allows package managers to determine what software capabilities and versions are required and which packages may conflict.

---

## 5. `rpm` and `dpkg`

### Goal

Practice working with the low-level package database tools.

### Practice - Debian

List installed packages:

```bash
dpkg -l | head
```

Query a package:

```bash
dpkg -l package-name
```

Inspect a local package if available:

```bash
dpkg -I package.deb
```

### Practice - RPM

List installed packages:

```bash
rpm -qa | head
```

Query a package:

```bash
rpm -q package-name
```

Inspect a local package if available:

```bash
rpm -qip package.rpm
```

### Result

Record which package database and package format your lab uses.

### What I learned

`dpkg` and `rpm` are low-level tools that work directly with their respective package formats and local package databases.

---

## 6. `apt` and `dnf`

### Goal

Practice repository-based package management.

### Practice - Debian / Ubuntu

Refresh package metadata:

```bash
sudo apt update
```

Inspect package information:

```bash
apt show package-name
```

Install a test package:

```bash
sudo apt install package-name
```

Remove it when finished:

```bash
sudo apt remove package-name
```

Upgrade available packages:

```bash
sudo apt upgrade
```

### Practice - RPM-based systems

Search for a package:

```bash
dnf search package-name
```

Install it:

```bash
sudo dnf install package-name
```

Remove it:

```bash
sudo dnf remove package-name
```

Upgrade packages:

```bash
sudo dnf upgrade
```

### Result

Record:

```text
Package manager:
Package searched:
Package installed:
Package removed:
Upgrade result:
```

### What I learned

High-level package managers use repository metadata and dependency information to perform package operations.

---

## 7. Compiling Source Code

### Goal

Understand the basic workflow for building and installing software from source.

### Practice

Use a small, disposable source project from the lab.

Inspect its files:

```bash
ls
```

Read the documentation:

```bash
less README.md
```

or:

```bash
less README
```

Check for build instructions and dependencies.

For a project using Autotools, a typical workflow may be:

```bash
./configure
make
make test
sudo make install
```

> Follow the project's own documentation if it uses a different build system.

After installation, verify the program:

```bash
command -v program
```

If supported:

```bash
program --version
```

### Result

Record:

```text
Source project:
Version:
Build system:
Configure result:
Build result:
Test result:
Install location:
Verification result:
```

### What I learned

Building from source requires more manual control than installing a distribution package. The process normally involves verification, configuration, compilation, testing, installation, and tracking.

---

# Final Mini Exercise

## Goal

Combine archive inspection, package metadata, package management, and source-build concepts into one workflow.

### Step 1 - Create an archive

```bash
mkdir package-lab
touch package-lab/file1.txt
touch package-lab/file2.txt
```

Create a compressed archive:

```bash
tar -czf package-lab.tar.gz package-lab/
```

### Step 2 - Inspect before extraction

```bash
tar -tzf package-lab.tar.gz
```

Verify the archive:

```bash
gzip -t package-lab.tar.gz
```

### Step 3 - Inspect package metadata

On Debian-based systems:

```bash
apt show package-name
```

On RPM-based systems:

```bash
rpm -qi package-name
```

Record:

```text
Package:
Version:
Dependencies:
```

### Step 4 - Inspect the package database

Debian:

```bash
dpkg -l | head
```

RPM:

```bash
rpm -qa | head
```

### Step 5 — Identify the repository manager

Debian / Ubuntu:

```bash
apt --version
```

RPM-based:

```bash
dnf --version
```

### Step 6 - Review source-build workflow

Document the sequence:

```text
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
Verify installation
     ↓
Track installed files
```

---

# Reflection

After completing the exercises, answer:

1. What is the difference between an upstream project and a distribution package?
2. What is the role of a package repository?
3. Why does a package manager need metadata?
4. What is the difference between `dpkg` and `apt`?
5. What is the difference between `rpm` and `dnf`?
6. Why should an archive be inspected before extraction?
7. What is the role of package dependencies?
8. What is the difference between installing a package and compiling software from source?
9. Why can source-installed software be harder to track?
10. What information should be verified before installing software from an external source?

---

## Key Takeaway

Linux software management combines several layers:

```text
Upstream Source
      ↓
Distribution Packaging
      ↓
Package Repository
      ↓
Package Metadata
      ↓
Package Manager
      ↓
Installed Software
```

Software can also bypass the distribution package system and be built directly from source. Understanding both workflows makes it easier to manage software consistently and recognize the trade-offs between packaged and source-built software.

