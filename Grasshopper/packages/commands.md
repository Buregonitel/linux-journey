# Linux Packages - Command Cheat Sheet

A practical reference for archives, package repositories, package databases, package managers, and source builds.

---

## 1. `tar`

`tar` creates and extracts archives.

### Create an archive

```bash
tar -cf archive.tar directory/
```

Options:

* `-c` - create
* `-f` - specify archive file

### List archive contents

```bash
tar -tf archive.tar
```

### Extract an archive

```bash
tar -xf archive.tar
```

Options:

* `-t` - list contents
* `-x` - extract

---

## 2. `tar` with `gzip`

Create a gzip-compressed tar archive:

```bash
tar -czf archive.tar.gz directory/
```

Extract it:

```bash
tar -xzf archive.tar.gz
```

List its contents without extracting:

```bash
tar -tzf archive.tar.gz
```

The `z` option tells `tar` to use gzip compression.

---

## 3. `gzip`

Compress a file:

```bash
gzip file.txt
```

This normally produces:

```text
file.txt.gz
```

Decompress it:

```bash
gzip -d file.txt.gz
```

Or:

```bash
gunzip file.txt.gz
```

### Test a gzip file

```bash
gzip -t file.txt.gz
```

This checks whether the compressed data is valid without extracting it.

---

## 4. Inspect Archives Before Extraction

Before extracting an archive from an external source, inspect its contents:

```bash
tar -tf archive.tar
```

For a gzip-compressed archive:

```bash
tar -tzf archive.tar.gz
```

This helps identify:

* unexpected paths;
* absolute paths;
* unexpected files;
* directory structure.

A useful rule is:

```text
Inspect → verify → extract
```

---

## 5. Package Information

Package metadata can describe:

* package name;
* version;
* dependencies;
* required capabilities;
* conflicts;
* relationships with libraries and other packages.

Package managers use this information when resolving installations and upgrades.

---

# Debian / Ubuntu

## 6. `dpkg`

`dpkg` is the low-level package management tool for Debian packages.

### List installed packages

```bash
dpkg -l
```

### Check whether a package is installed

```bash
dpkg -l package-name
```

### Inspect a local `.deb` archive

```bash
dpkg -I package.deb
```

### Install a local `.deb`

```bash
sudo dpkg -i package.deb
```

### Remove a package

```bash
sudo dpkg -r package-name
```

`dpkg` works directly with Debian package files and the local package database. Dependency resolution is normally handled by higher-level tools such as APT.

---

## 7. APT Repositories

APT uses configured repository sources to discover packages.

Common configuration locations:

```text
/etc/apt/sources.list
/etc/apt/sources.list.d/
```

Inspect configured sources:

```bash
cat /etc/apt/sources.list
```

List additional source files:

```bash
ls /etc/apt/sources.list.d/
```

---

## 8. `apt update`

Refresh local package indexes:

```bash
sudo apt update
```

This does not normally install or upgrade packages. It retrieves current repository metadata so APT can work with up-to-date package information.

---

## 9. `apt install`

Install a package:

```bash
sudo apt install package-name
```

APT resolves required dependencies using repository metadata.

---

## 10. `apt remove`

Remove an installed package:

```bash
sudo apt remove package-name
```

The exact treatment of configuration files depends on the operation and package.

---

## 11. `apt upgrade`

Upgrade installed packages:

```bash
sudo apt upgrade
```

A typical maintenance sequence is:

```bash
sudo apt update
sudo apt upgrade
```

The first command refreshes repository metadata; the second applies available upgrades.

---

# RPM-based Systems

## 12. `rpm`

`rpm` is the low-level package tool for RPM packages.

### List installed packages

```bash
rpm -qa
```

### Query a package

```bash
rpm -q package-name
```

### Inspect a local RPM package

```bash
rpm -qip package.rpm
```

### Verify an installed package

```bash
rpm -V package-name
```

### Install a local RPM

```bash
sudo rpm -i package.rpm
```

### Remove a package

```bash
sudo rpm -e package-name
```

`rpm` works directly with the local package database and RPM package files.

---

## 13. `dnf`

DNF is a higher-level package manager for RPM-based systems.

### Search for a package

```bash
dnf search package-name
```

### Install a package

```bash
sudo dnf install package-name
```

### Remove a package

```bash
sudo dnf remove package-name
```

### Upgrade installed packages

```bash
sudo dnf upgrade
```

### Check for available updates

```bash
dnf check-update
```

DNF uses repository metadata and resolves dependencies.

---

## 14. `yum`

`yum` is the traditional package management interface used by many RPM-based systems.

Typical operations include:

```bash
sudo yum install package-name
sudo yum remove package-name
sudo yum update
```

On modern distributions, `yum` may be provided as a compatibility interface around DNF.

---

## 15. Package Manager Comparison

| Task                     | Debian / Ubuntu | RPM-based                              |
| ------------------------ | --------------- | -------------------------------------- |
| Package format           | `.deb`          | `.rpm`                                 |
| Low-level tool           | `dpkg`          | `rpm`                                  |
| Repository manager       | `apt`           | `dnf` / `yum`                          |
| Refresh metadata         | `apt update`    | Repository metadata handled by DNF/YUM |
| Install package          | `apt install`   | `dnf install`                          |
| Remove package           | `apt remove`    | `dnf remove`                           |
| Upgrade packages         | `apt upgrade`   | `dnf upgrade`                          |
| Query installed packages | `dpkg -l`       | `rpm -qa`                              |

---

## 16. Source Code Compilation

A source build usually consists of several stages.

### Inspect the source tree

```bash
ls
```

Read the project's documentation:

```bash
less README
```

or:

```bash
less README.md
```

### Configure

For projects using Autotools:

```bash
./configure
```

### Build

```bash
make
```

### Test

If the project provides a test target:

```bash
make test
```

Some projects use a different command, so the project's documentation should be followed.

### Install

```bash
sudo make install
```

The installation location and procedure depend on the project's build system.

---

## 17. Verify a Source Build

Before compiling software from source, check:

* source origin;
* project documentation;
* required dependencies;
* build instructions;
* version information;
* available tests.

After installation, verify that the expected executable or files are present.

For example:

```bash
command -v program
```

and:

```bash
program --version
```

when supported.

---

## 18. Tracking Source-installed Software

Unlike package-managed software, software installed directly with commands such as:

```bash
sudo make install
```

may not automatically appear in the distribution package database.

Before installing from source, determine:

* where files will be installed;
* how the software will be removed;
* how upgrades will be handled;
* how installed files will be tracked.

A package manager is generally easier to maintain because it records package ownership and metadata.

---

## Quick Reference

| Command        | Purpose                            |
| -------------- | ---------------------------------- |
| `tar -cf`      | Create tar archive                 |
| `tar -tf`      | List archive contents              |
| `tar -xf`      | Extract tar archive                |
| `tar -czf`     | Create gzip-compressed tar archive |
| `tar -tzf`     | List `.tar.gz` contents            |
| `gzip`         | Compress a file                    |
| `gzip -d`      | Decompress gzip data               |
| `gzip -t`      | Test gzip integrity                |
| `dpkg -l`      | List Debian packages               |
| `dpkg -I`      | Inspect `.deb` package             |
| `dpkg -i`      | Install local `.deb`               |
| `dpkg -r`      | Remove Debian package              |
| `apt update`   | Refresh APT metadata               |
| `apt install`  | Install package                    |
| `apt remove`   | Remove package                     |
| `apt upgrade`  | Upgrade packages                   |
| `rpm -qa`      | List installed RPM packages        |
| `rpm -q`       | Query RPM package                  |
| `rpm -V`       | Verify installed RPM package       |
| `rpm -i`       | Install local RPM                  |
| `rpm -e`       | Remove RPM package                 |
| `dnf install`  | Install RPM-based package          |
| `dnf remove`   | Remove package                     |
| `dnf upgrade`  | Upgrade packages                   |
| `yum install`  | Install package                    |
| `yum remove`   | Remove package                     |
| `make`         | Build source code                  |
| `make test`    | Run project tests                  |
| `make install` | Install built software             |

---

## Safe Package Workflow

### Debian / Ubuntu

```bash
sudo apt update
sudo apt install package-name
sudo apt upgrade
```

### RPM-based

```bash
sudo dnf install package-name
sudo dnf upgrade
```

### Local archive

Before extracting an archive:

```bash
tar -tzf archive.tar.gz
```

Before installing a local package:

```bash
dpkg -I package.deb
```

or:

```bash
rpm -qip package.rpm
```

The general principle is:

```text
Inspect → verify → install/extract → test → track
```

