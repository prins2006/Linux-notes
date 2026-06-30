# Add Disk and Create Standard Partition (Production-Level Guide)

**Level:** Beginner → Advanced → Production
**Operating Systems:** RHEL 7/8/9, Rocky Linux 8/9, AlmaLinux, CentOS, Ubuntu, Debian

---

# Table of Contents

1. Introduction
2. What is a Standard Partition?
3. When to Add a New Disk
4. Prerequisites
5. Complete Workflow
6. Step 1 – Verify the New Disk
7. Step 2 – Create a Standard Partition
8. Step 3 – Inform the Kernel
9. Step 4 – Create a Filesystem
10. Step 5 – Create a Mount Point
11. Step 6 – Mount the Partition
12. Step 7 – Make the Mount Persistent
13. Step 8 – Verify the Configuration
14. Production Example
15. Common Commands
16. Troubleshooting
17. Best Practices
18. Interview Questions

---

# 1. Introduction

Adding a new disk is one of the most common Linux administration tasks. A newly attached disk cannot store files until it has been:

1. Detected by Linux.
2. Partitioned.
3. Formatted with a filesystem.
4. Mounted.
5. Added to `/etc/fstab` if it should remain mounted after a reboot.

---

# 2. What is a Standard Partition?

A **standard partition** is a regular disk partition created directly on a storage device (for example, `/dev/sdb1`). It is not part of LVM, RAID, or any other advanced storage technology.

Example:

```text
Physical Disk

/dev/sdb (200 GB)

↓

Standard Partition

/dev/sdb1 (200 GB)
  
↓

Filesystem (XFS / ext4)

↓

Mount Point (/data)
```

---

# 3. When to Add a New Disk

Common production scenarios:

* Increase application storage.
* Add a database volume.
* Create a backup partition.
* Store log files separately.
* Expand file server capacity.
* Prepare storage for virtual machines.

---

# 4. Prerequisites

Before starting:

* Root or sudo access.
* A new disk attached to the system.
* Verify the disk is **not** already in use.
* Take backups before modifying existing partitions.

---

# 5. Complete Workflow

```text
New Disk Added
      │
      ▼
Detect Disk
      │
      ▼
Create Partition
      │
      ▼
Create Filesystem
      │
      ▼
Create Mount Point
      │
      ▼
Mount Partition
      │
      ▼
Update /etc/fstab
      │
      ▼
Verify
```

---

# 6. Step 1 – Verify the New Disk

List disks:

```bash
lsblk
```

Example:

```text
NAME   SIZE TYPE MOUNTPOINT

sda    100G disk

├── sda1   1G part /boot

└── sda2  99G part /

sdb    200G disk
```

Or use:

```bash
sudo fdisk -l
```

Example:

```text
Disk /dev/sdb: 200 GiB
```

At this point, `/dev/sdb` has **no partitions**.

---

# 7. Step 2 – Create a Standard Partition

Start `fdisk`:

```bash
sudo fdisk /dev/sdb
```

Interactive session:

```text
Command (m for help): n

Partition type: primary

Partition number: 1

First sector: Press Enter

Last sector: Press Enter

Command (m for help): w
```

Meaning:

* `n` → Create a new partition.
* `Enter` → Use the default start sector.
* `Enter` → Use all remaining space.
* `w` → Write changes to disk and exit.

Verify:

```bash
lsblk
```

Example:

```text
sdb

└── sdb1 200G
```

---

# 8. Step 3 – Inform the Kernel

Sometimes the kernel detects the new partition automatically.

If not, reload the partition table:

```bash
sudo partprobe
```

Or:

```bash
sudo partx -u /dev/sdb
```

Verify:

```bash
lsblk
```

---

# 9. Step 4 – Create a Filesystem

Create an XFS filesystem (default on RHEL/Rocky):

```bash
sudo mkfs.xfs /dev/sdb1
```

Or create an ext4 filesystem:

```bash
sudo mkfs.ext4 /dev/sdb1
```

Example output:

```text
meta-data=/dev/sdb1

data blocks=...
```

Verify:

```bash
blkid
```

Example:

```text
/dev/sdb1: UUID="d7a8..." TYPE="xfs"
```

---

# 10. Step 5 – Create a Mount Point

Create a directory:

```bash
sudo mkdir /data
```

Verify:

```bash
ls -ld /data
```

Output:

```text
drwxr-xr-x root root /data
```

---

# 11. Step 6 – Mount the Partition

Mount it:

```bash
sudo mount /dev/sdb1 /data
```

Verify:

```bash
df -h
```

Example:

```text
Filesystem      Size Used Avail Mounted on

/dev/sdb1       200G   1G 199G /data
```

Also verify:

```bash
mount | grep /data

lsblk
```

---

# 12. Step 7 – Make the Mount Persistent

Find the UUID:

```bash
blkid
```

Example:

```text
UUID="d7a8b2e1-1234..."
```

Edit `/etc/fstab`:

```bash
sudo vi /etc/fstab
```

Add:

```text
UUID=d7a8b2e1-1234...   /data   xfs   defaults   0   0
```

Test the configuration:

```bash
sudo mount -a
```

If there are no errors, the entry is valid.

---

# 13. Step 8 – Verify the Configuration

Check the mount:

```bash
df -h
```

Check block devices:

```bash
lsblk
```

Check UUID:

```bash
blkid
```

Reboot the system:

```bash
sudo reboot
```

After reboot:

```bash
df -h

lsblk
```

The partition should still be mounted at `/data`.

---

# 14. Production Example

A company adds a **500 GB SSD** to a Rocky Linux database server.

Steps:

```text
Add SSD

↓

Detect /dev/sdb

↓

Create /dev/sdb1

↓

Format as XFS

↓

Mount at /database

↓

Update /etc/fstab

↓

Restart Server

↓

Database Uses New Storage
```

Commands:

```bash
lsblk

sudo fdisk /dev/sdb

sudo mkfs.xfs /dev/sdb1

sudo mkdir /database

sudo mount /dev/sdb1 /database

blkid

sudo vi /etc/fstab

sudo mount -a
```

---

# 15. Common Commands

```bash
lsblk

fdisk -l

sudo fdisk /dev/sdb

partprobe

mkfs.xfs /dev/sdb1

mkfs.ext4 /dev/sdb1

mkdir /data

mount /dev/sdb1 /data

blkid

df -h

mount

umount /data

mount -a
```

---

# 16. Troubleshooting

### New Disk Not Visible

```bash
lsblk

fdisk -l

dmesg | grep sd
```

---

### Partition Missing

```bash
sudo fdisk -l
```

Run:

```bash
sudo partprobe
```

---

### Mount Failed

Check:

```bash
blkid

cat /etc/fstab

mount -a
```

---

### Wrong Filesystem

Verify:

```bash
lsblk -f
```

---

### Filesystem Corruption

For ext4:

```bash
sudo fsck /dev/sdb1
```

For XFS:

```bash
sudo xfs_repair /dev/sdb1
```

---

# 17. Best Practices

* Use GPT partition tables for new disks larger than 2 TB.
* Use XFS on RHEL/Rocky unless another filesystem is required.
* Use UUIDs in `/etc/fstab` instead of device names.
* Test `/etc/fstab` with `mount -a` before rebooting.
* Keep backups before changing partition layouts.
* Verify the target disk carefully to avoid modifying the wrong device.

---

# 18. Interview Questions

### 1. What is a standard partition?

A regular partition created directly on a disk, such as `/dev/sdb1`, without using LVM or RAID.

### 2. Which command lists all disks?

```bash
lsblk
```

or

```bash
sudo fdisk -l
```

### 3. Which command creates a partition?

```bash
sudo fdisk /dev/sdb
```

### 4. Which command creates an XFS filesystem?

```bash
sudo mkfs.xfs /dev/sdb1
```

### 5. Which file stores permanent mount information?

```text
/etc/fstab
```

### 6. How do you verify that a partition is mounted?

```bash
df -h

lsblk

mount
```

### 7. Why should UUID be used in `/etc/fstab`?

Because UUIDs remain consistent even if device names (such as `/dev/sdb`) change after hardware changes or reboots.
