# Storage and Disk Partition (`df`, `fdisk`)

**Level:** Beginner → Advanced → Production
**Operating Systems:** RHEL 7/8/9, Rocky Linux 8/9, AlmaLinux, CentOS, Ubuntu, Debian

---

# Table of Contents

1. Introduction
2. What is Storage?
3. Storage Types
4. Disk Structure
5. What is a Partition?
6. MBR vs GPT
7. Linux Device Naming
8. Filesystem Overview
9. The `df` Command
10. The `fdisk` Command
11. Creating a New Partition
12. Formatting a Partition
13. Mounting a Partition
14. Permanent Mount with `/etc/fstab`
15. Production Example
16. Common Commands
17. Troubleshooting
18. Best Practices
19. Interview Questions

---

# 1. Introduction

Storage is one of the most important components of a Linux system. It provides permanent space for:

* Operating System
* User Data
* Databases
* Application Files
* Logs
* Backups
* Virtual Machines

Before Linux can use a disk, it must usually be:

1. Detected by the system
2. Partitioned
3. Formatted with a filesystem
4. Mounted

---

# 2. What is Storage?

Storage is hardware that permanently stores data, even when the computer is powered off.

Examples:

* HDD (Hard Disk Drive)
* SATA SSD
* NVMe SSD
* USB Drive
* SAN Storage
* NAS Storage

Example:

```text
Computer

↓

NVMe SSD (500 GB)

↓

Partitions

↓

Filesystem

↓

Files
```

---

# 3. Storage Types

| Storage  | Speed     | Usage                     |
| -------- | --------- | ------------------------- |
| HDD      | Slow      | Archives, backups         |
| SATA SSD | Fast      | General servers           |
| NVMe SSD | Very Fast | Databases, virtualization |
| SAN      | High      | Enterprise storage        |
| NAS      | Medium    | Shared file storage       |

---

# 4. Disk Structure

Example of a 500 GB disk:

```text
/dev/sda

├── sda1 500 MB   EFI
├── sda2 2 GB     /boot
├── sda3 100 GB   /
├── sda4 397 GB   /home
```

Each partition can have its own filesystem.

---

# 5. What is a Partition?

A partition is a logical section of a physical disk.

Advantages:

* Separate OS and data
* Easier backup
* Better security
* Flexible storage management

Example:

```text
1 TB Disk

↓

Partition 1 → OS

Partition 2 → Home

Partition 3 → Database

Partition 4 → Backup
```

---

# 6. MBR vs GPT

| Feature                   | MBR       | GPT               |
| ------------------------- | --------- | ----------------- |
| Maximum Disk Size         | 2 TB      | Greater than 2 TB |
| Maximum Partitions        | 4 Primary | 128 (typical)     |
| Boot Mode                 | BIOS      | UEFI              |
| Redundant Partition Table | No        | Yes               |
| Modern Recommendation     | No        | Yes               |

---

# 7. Linux Device Naming

Examples:

```text
/dev/sda

/dev/sdb

/dev/nvme0n1
```

Partitions:

```text
/dev/sda1

/dev/sda2

/dev/sdb1

/dev/nvme0n1p1
```

Display disks:

```bash
lsblk
```

Example:

```text
NAME        SIZE TYPE MOUNTPOINT

sda         100G disk

├── sda1      1G part /boot

├── sda2     99G part /

sdb         500G disk
```

---

# 8. Filesystem Overview

Common Linux filesystems:

| Filesystem | Usage                           |
| ---------- | ------------------------------- |
| ext4       | Default on many Linux systems   |
| XFS        | Default on RHEL/Rocky           |
| Btrfs      | Snapshots and advanced features |
| vfat       | EFI System Partition            |

Create an XFS filesystem:

```bash
mkfs.xfs /dev/sdb1
```

Create an ext4 filesystem:

```bash
mkfs.ext4 /dev/sdb1
```

---

# 9. The `df` Command

The `df` command reports **mounted filesystem disk space usage**.

Syntax:

```bash
df [options]
```

Display human-readable output:

```bash
df -h
```

Example:

```text
Filesystem      Size Used Avail Use% Mounted on

/dev/sda2        50G  18G   30G  38% /

/dev/sda1       1.0G 220M  804M  22% /boot

/dev/sdb1       500G 120G  380G  24% /data
```

Useful options:

```bash
df -h        # Human-readable
df -T        # Show filesystem type
df -i        # Show inode usage
df -h /data  # Check a specific mount point
```

Production use:

```bash
df -h
```

Check for disks nearing 100% usage before they cause application failures.

---

# 10. The `fdisk` Command

`fdisk` is used to view and manage partition tables on disks.

Display partitions:

```bash
sudo fdisk -l
```

Example:

```text
Disk /dev/sdb: 500 GiB

Device     Start       End   Sectors Size Type

/dev/sdb1   2048 104857599 104855552  50G Linux filesystem
```

Start interactive mode:

```bash
sudo fdisk /dev/sdb
```

Common interactive commands:

| Key | Action                 |
| --- | ---------------------- |
| `m` | Help                   |
| `p` | Print partition table  |
| `n` | New partition          |
| `d` | Delete partition       |
| `t` | Change partition type  |
| `w` | Write changes and exit |
| `q` | Quit without saving    |

---

# 11. Creating a New Partition

Example:

```bash
sudo fdisk /dev/sdb
```

Interactive sequence:

```text
Command (m for help): n

Partition type: primary

Partition number: 1

First sector: Enter

Last sector: +100G

Command (m for help): w
```

Verify:

```bash
lsblk
```

---

# 12. Formatting a Partition

Create an ext4 filesystem:

```bash
sudo mkfs.ext4 /dev/sdb1
```

Create an XFS filesystem:

```bash
sudo mkfs.xfs /dev/sdb1
```

Verify:

```bash
blkid
```

---

# 13. Mounting a Partition

Create a mount point:

```bash
sudo mkdir /data
```

Mount:

```bash
sudo mount /dev/sdb1 /data
```

Verify:

```bash
df -h

mount

lsblk
```

---

# 14. Permanent Mount with `/etc/fstab`

Find the UUID:

```bash
blkid
```

Example:

```text
UUID="2f73d1d0-8f..." TYPE="xfs"
```

Edit `/etc/fstab`:

```text
UUID=2f73d1d0-8f...   /data   xfs   defaults   0   0
```

Test:

```bash
sudo mount -a
```

---

# 15. Production Example

A database server receives a new 1 TB SSD.

Steps:

1. Detect the disk:

```bash
lsblk
```

2. Create a partition:

```bash
sudo fdisk /dev/sdb
```

3. Format it:

```bash
sudo mkfs.xfs /dev/sdb1
```

4. Create mount point:

```bash
sudo mkdir /database
```

5. Mount:

```bash
sudo mount /dev/sdb1 /database
```

6. Add an entry to `/etc/fstab`.

7. Verify:

```bash
df -h

mount

lsblk
```

The database now stores data on the dedicated SSD.

---

# 16. Common Commands

```bash
lsblk

fdisk -l

df -h

df -T

blkid

mount

umount /data

mkfs.ext4 /dev/sdb1

mkfs.xfs /dev/sdb1

mount /dev/sdb1 /data

mount -a
```

---

# 17. Troubleshooting

### Disk Not Detected

```bash
lsblk

fdisk -l

dmesg | grep sd
```

### Mount Failed

Check:

```bash
blkid

cat /etc/fstab

mount -a
```

### Filesystem Corruption

For ext4:

```bash
fsck /dev/sdb1
```

For XFS:

```bash
xfs_repair /dev/sdb1
```

---

# 18. Best Practices

* Use GPT on modern systems.
* Use UUIDs in `/etc/fstab` instead of device names.
* Verify `/etc/fstab` with `mount -a` before rebooting.
* Monitor disk usage regularly with `df -h`.
* Keep adequate free space (often at least 15–20%) on production filesystems.
* Use separate partitions for databases, logs, and backups where appropriate.
* Take backups before modifying partitions.

---

# 19. Interview Questions

### 1. What is the purpose of the `df` command?

It displays mounted filesystem disk space usage.

### 2. What is the difference between `df` and `fdisk`?

| `df`                         | `fdisk`                          |
| ---------------------------- | -------------------------------- |
| Shows filesystem usage       | Manages disk partitions          |
| Works on mounted filesystems | Works on partition tables        |
| Does not create partitions   | Can create and delete partitions |

### 3. Which command lists all disks and partitions?

```bash
sudo fdisk -l
```

### 4. How do you create a new partition?

```bash
sudo fdisk /dev/sdb
```

Use `n` to create a new partition and `w` to save it.

### 5. How do you permanently mount a partition?

Add its UUID and mount point to `/etc/fstab`, then test with:

```bash
sudo mount -a
```
