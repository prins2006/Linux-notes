# Advanced Storage Management with Stratis

> **Production-Level Guide**
>
> This guide explains Stratis from beginner to advanced level, including its architecture, installation, storage pools, filesystems, snapshots, monitoring, management, troubleshooting, and real-world examples.

---

# Table of Contents

1. Introduction
2. What is Stratis?
3. Why Stratis?
4. Stratis Architecture
5. Features
6. Stratis vs LVM
7. Stratis Components
8. Installation
9. Verify Installation
10. Start the Service
11. Architecture Workflow
12. Create a Storage Pool
13. Create a Filesystem
14. Mount the Filesystem
15. Persistent Mount
16. Check Pool Information
17. Add New Disk to Pool
18. Resize Filesystem
19. Create Snapshot
20. Restore Snapshot
21. Destroy Filesystem
22. Destroy Pool
23. Monitoring
24. Troubleshooting
25. Best Practices
26. Production Use Case
27. Interview Questions
28. Summary

---

# 1. Introduction

Managing storage using traditional partitions can be difficult because:

- Storage cannot easily grow.
- Filesystem resizing requires multiple steps.
- Snapshot management is complex.
- Storage pools are difficult to manage.

Modern Linux introduces **Stratis**, which combines multiple storage technologies into one easy-to-use storage management solution.

Think of Stratis as:

```
LVM
+
XFS
+
Device Mapper
+
Thin Provisioning
+
Snapshots
=
Stratis
```

Stratis simplifies advanced storage management.

---

# 2. What is Stratis?

**Stratis** is a local storage management solution developed by Red Hat.

It provides:

- Storage pools
- Thin provisioning
- Snapshots
- Automatic filesystem management
- Easy administration

Unlike LVM, administrators do not manually resize filesystems after extending storage.

---

# 3. Why Stratis?

Without Stratis:

```
Disk

↓

Partition

↓

PV

↓

VG

↓

LV

↓

Filesystem

↓

Mount
```

Many manual steps.

With Stratis:

```
Disk

↓

Pool

↓

Filesystem

↓

Mount
```

Much simpler.

---

# 4. Stratis Architecture

```
+----------------------+
|      Applications    |
+----------+-----------+
           |
           v
+----------------------+
|      Filesystem      |
|         XFS          |
+----------+-----------+
           |
           v
+----------------------+
|     Stratis Pool     |
+----------+-----------+
           |
           v
+----------------------+
| Block Devices (Disk) |
+----------------------+
```

---

# 5. Features

- Storage Pools
- Thin Provisioning
- XFS Integration
- Snapshots
- Automatic Growth
- Multiple Disks
- Simple Administration
- Device Management
- Easy Expansion

---

# 6. Stratis vs LVM

| Feature | LVM | Stratis |
|----------|-----|----------|
| Storage Pool | Yes | Yes |
| Snapshots | Yes | Yes |
| Thin Provisioning | Manual | Automatic |
| Filesystem Resize | Manual | Automatic |
| Uses XFS | Optional | Default |
| Easy Management | Medium | Easy |
| Production Ready | Yes | Yes |

---

# 7. Stratis Components

```
Disk

↓

Pool

↓

Filesystem

↓

Mount Point
```

Example

```
Disk

/dev/sdb

↓

Pool

mypool

↓

Filesystem

myfs

↓

Mount

/data
```

---

# 8. Installation

RHEL / Rocky / AlmaLinux

```bash
dnf install stratisd stratis-cli -y
```

Ubuntu (if available in repositories)

```bash
apt install stratis-cli
```

---

# 9. Verify Installation

```bash
rpm -q stratis-cli
```

or

```bash
stratis --version
```

---

# 10. Start Service

```bash
systemctl enable --now stratisd
```

Verify

```bash
systemctl status stratisd
```

---

# 11. Check Available Disks

```bash
lsblk
```

Example

```
sda

sdb

sdc
```

Assume

```
/dev/sdb
```

will be used.

---

# 12. Create Storage Pool

Syntax

```bash
stratis pool create POOL_NAME DEVICE
```

Example

```bash
stratis pool create mypool /dev/sdb
```

Verify

```bash
stratis pool list
```

Output

```
Pool Name

mypool
```

---

# 13. Create Filesystem

Syntax

```bash
stratis filesystem create POOL_NAME FS_NAME
```

Example

```bash
stratis filesystem create mypool datafs
```

Verify

```bash
stratis filesystem list
```

---

# 14. Mount Filesystem

Create directory

```bash
mkdir /data
```

Find device

```bash
ls /dev/stratis
```

Mount

```bash
mount /dev/stratis/mypool/datafs /data
```

Verify

```bash
df -h
```

---

# 15. Persistent Mount

Find UUID

```bash
blkid
```

Edit

```bash
vim /etc/fstab
```

Example

```
/dev/stratis/mypool/datafs /data xfs defaults 0 0
```

Test

```bash
mount -a
```

---

# 16. Pool Information

Display pools

```bash
stratis pool list
```

Detailed

```bash
stratis pool list --details
```

---

# 17. Filesystem Information

```bash
stratis filesystem list
```

Detailed

```bash
stratis filesystem list --details
```

---

# 18. Add Another Disk

Suppose

```
/dev/sdc
```

Add

```bash
stratis pool add-data mypool /dev/sdc
```

Verify

```bash
stratis pool list
```

Storage increases immediately.

---

# 19. Automatic Filesystem Growth

Unlike LVM

```
lvextend

↓

resize2fs
```

Stratis automatically grows the filesystem when pool capacity increases.

No manual filesystem resize is needed.

---

# 20. Create Snapshot

Syntax

```bash
stratis filesystem snapshot mypool datafs snap1
```

Verify

```bash
stratis filesystem list
```

Example

```
datafs

snap1
```

---

# 21. Delete Snapshot

```bash
stratis filesystem destroy mypool snap1
```

---

# 22. Destroy Filesystem

```bash
stratis filesystem destroy mypool datafs
```

---

# 23. Destroy Pool

Remove filesystems first.

Then

```bash
stratis pool destroy mypool
```

---

# 24. Monitoring

Pool

```bash
stratis pool list
```

Filesystem

```bash
stratis filesystem list
```

Block devices

```bash
lsblk
```

Filesystem usage

```bash
df -h
```

Disk usage

```bash
du -sh /data
```

---

# 25. Real Production Example

Company

```
Application Server
```

Current Storage

```
500GB
```

Application grows.

Need

```
+1TB
```

Instead of:

- Repartition
- Resize LVM
- Resize filesystem

Administrator simply adds:

```
/dev/sdc
```

Run

```bash
stratis pool add-data mypool /dev/sdc
```

Done.

Filesystem grows automatically.

No downtime required.

---

# 26. Troubleshooting

## Pool already exists

```
Pool already exists
```

Check

```bash
stratis pool list
```

---

## Device busy

```
Device busy
```

Check

```bash
lsblk
```

Unmount if needed.

---

## Service not running

```bash
systemctl start stratisd
```

---

## No filesystem found

Verify

```bash
stratis filesystem list
```

---

# 27. Best Practices

- Use dedicated disks for Stratis pools.
- Monitor free pool capacity regularly.
- Use snapshots before major updates.
- Keep regular backups; snapshots are not backups.
- Use XFS (the default filesystem).
- Document pool and filesystem layouts.
- Verify storage health before adding new disks.

---

# 28. Common Commands

| Command | Description |
|----------|-------------|
| `stratis pool create` | Create a storage pool |
| `stratis pool list` | List pools |
| `stratis pool add-data` | Add a disk to a pool |
| `stratis filesystem create` | Create a filesystem |
| `stratis filesystem list` | List filesystems |
| `stratis filesystem snapshot` | Create a snapshot |
| `stratis filesystem destroy` | Delete a filesystem or snapshot |
| `systemctl status stratisd` | Check Stratis service |
| `df -h` | View mounted filesystem usage |
| `lsblk` | Display block devices |

---

# 29. Interview Questions

### Q1. What is Stratis?

**Answer:**

Stratis is a storage management solution for Linux that simplifies managing storage pools, filesystems, snapshots, and thin provisioning by combining technologies such as Device Mapper and XFS behind an easy-to-use interface.

---

### Q2. Is Stratis a filesystem?

**Answer:**

No. Stratis is **not** a filesystem. It is a storage management layer that creates and manages XFS filesystems.

---

### Q3. Which filesystem does Stratis use?

**Answer:**

XFS.

---

### Q4. Does Stratis support snapshots?

**Answer:**

Yes, Stratis supports efficient snapshots of its managed filesystems.

---

### Q5. Can you add disks later?

**Answer:**

Yes. Additional block devices can be added to an existing Stratis pool without recreating the filesystem.

---

### Q6. Which service manages Stratis?

**Answer:**

`stratisd`

---

# 30. Summary

Stratis is an advanced storage management framework designed to simplify complex storage administration on Linux. It provides storage pools, automatic XFS filesystem management, thin provisioning, snapshots, and easy pool expansion. Compared to traditional LVM workflows, Stratis reduces the number of administrative steps required to manage storage while offering enterprise-grade features suitable for modern production environments.