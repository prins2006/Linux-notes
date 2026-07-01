# RAID (Redundant Array of Independent Disks)

> **Production-Level Guide**
>
> This guide explains RAID from basic to advanced concepts, including RAID levels, hardware vs software RAID, Linux RAID configuration using `mdadm`, monitoring, troubleshooting, performance, and production best practices.

---

# Table of Contents

1. Introduction
2. What is RAID?
3. Why RAID is Needed
4. RAID Terminology
5. Hardware RAID vs Software RAID
6. RAID Levels Overview
7. RAID 0
8. RAID 1
9. RAID 5
10. RAID 6
11. RAID 10
12. RAID Comparison Table
13. Linux Software RAID (mdadm)
14. Installing mdadm
15. Creating RAID Arrays
16. Formatting and Mounting RAID
17. Making RAID Persistent
18. Monitoring RAID
19. Simulating Disk Failure
20. Replacing a Failed Disk
21. Growing a RAID Array
22. Performance Considerations
23. RAID and LVM
24. RAID Best Practices
25. Troubleshooting
26. Interview Questions
27. Summary

---

# 1. Introduction

**RAID (Redundant Array of Independent Disks)** combines multiple physical disks into a single logical storage unit.

Depending on the RAID level, RAID can provide:

- Better performance
- Higher availability
- Data redundancy
- Fault tolerance
- Larger storage capacity

RAID is commonly used in:

- Database Servers
- Virtualization Hosts
- File Servers
- Backup Servers
- NAS/SAN Storage
- Enterprise Data Centers

---

# 2. What is RAID?

Without RAID:

```
Application
     │
     ▼
 Single Disk
```

If the disk fails:

```
Application
     │
     ▼
 Disk Failure
     │
     ▼
 Data Lost
```

With RAID:

```
Application
     │
     ▼
 RAID Array
     │
 ┌───┴────┐
 ▼        ▼
Disk1   Disk2
```

Depending on the RAID level, the system may continue working even if one or more disks fail.

---

# 3. Why RAID is Needed

Example:

A company stores customer databases on a single 2 TB disk.

If the disk fails:

- Database becomes unavailable.
- Downtime increases.
- Data may be permanently lost.
- Business operations stop.

With RAID:

- Data remains available.
- Failed disks can be replaced.
- Downtime is reduced.
- Storage performance can improve.

> **Important:** RAID is **not a backup**. Accidental deletion, malware, ransomware, or file corruption can still affect all disks in a RAID array.

---

# 4. RAID Terminology

| Term | Description |
|------|-------------|
| Disk | Physical storage device |
| RAID Array | Group of disks working together |
| Striping | Data spread across disks |
| Mirroring | Identical copy stored on another disk |
| Parity | Extra information used to rebuild data |
| Hot Spare | Idle disk that automatically replaces a failed disk |
| Rebuild | Recreating lost data after replacing a failed disk |

---

# 5. Hardware RAID vs Software RAID

## Hardware RAID

Uses a dedicated RAID controller.

```
CPU

↓

RAID Controller

↓

Disks
```

Advantages:

- Better performance
- Dedicated cache
- Lower CPU usage
- Enterprise reliability

Disadvantages:

- Expensive
- Controller dependency

---

## Software RAID

Managed by the Linux kernel using `mdadm`.

```
Linux Kernel

↓

mdadm

↓

Disks
```

Advantages:

- Free
- Flexible
- Easy to configure
- No dedicated hardware required

Disadvantages:

- Uses CPU resources

---

# 6. RAID Levels Overview

| RAID | Minimum Disks | Fault Tolerance | Performance |
|------|---------------|----------------|------------|
| RAID 0 | 2 | None | Excellent |
| RAID 1 | 2 | 1 Disk | Good |
| RAID 5 | 3 | 1 Disk | Very Good |
| RAID 6 | 4 | 2 Disks | Good |
| RAID 10 | 4 | Multiple (depending on mirror pairs) | Excellent |

---

# 7. RAID 0 (Striping)

```
Disk1     Disk2

A1         A2
A3         A4
A5         A6
```

Characteristics:

- Fastest RAID level
- No redundancy
- Full disk capacity available
- Any disk failure destroys the array

Example:

Two 1 TB disks:

```
1 TB + 1 TB = 2 TB usable
```

---

# 8. RAID 1 (Mirroring)

```
Disk1      Disk2

A          A
B          B
C          C
```

Characteristics:

- Data written to both disks
- One disk may fail without data loss
- Excellent availability

Example:

Two 1 TB disks:

```
Usable Capacity = 1 TB
```

---

# 9. RAID 5 (Striping with Parity)

Requires at least **3 disks**.

```
Disk1   Disk2   Disk3

A1      A2      P
B1      P       B2
P       C1      C2
```

Characteristics:

- Good balance of performance and redundancy
- One disk may fail
- Parity allows reconstruction

Capacity Formula:

```
(Number of Disks - 1) × Disk Size
```

Example:

```
4 × 1 TB

Usable = 3 TB
```

---

# 10. RAID 6 (Dual Parity)

Requires at least **4 disks**.

```
Disk1 Disk2 Disk3 Disk4

A1    A2    P1    P2
```

Characteristics:

- Two parity blocks
- Two disks may fail
- Better protection than RAID 5

Capacity Formula:

```
(Number of Disks - 2) × Disk Size
```

---

# 11. RAID 10 (1+0)

Combines RAID 1 and RAID 0.

```
Mirror Pair 1

Disk1 ⇄ Disk2

Mirror Pair 2

Disk3 ⇄ Disk4

↓

Striped Together
```

Characteristics:

- High performance
- Excellent redundancy
- Preferred for databases and virtualization

Minimum:

```
4 disks
```

---

# 12. RAID Comparison Table

| RAID | Speed | Redundancy | Capacity Efficiency |
|------|------|------------|---------------------|
| RAID 0 | Excellent | None | 100% |
| RAID 1 | Good | High | 50% |
| RAID 5 | Very Good | High | (N-1)/N |
| RAID 6 | Good | Very High | (N-2)/N |
| RAID 10 | Excellent | Very High | 50% |

---

# 13. Installing mdadm

RHEL/Rocky/AlmaLinux

```bash
dnf install mdadm -y
```

Ubuntu/Debian

```bash
apt install mdadm
```

Verify:

```bash
mdadm --version
```

---

# 14. Check Available Disks

```bash
lsblk
```

Example:

```
sdb
sdc
```

---

# 15. Create RAID 1

Create the array:

```bash
mdadm --create --verbose /dev/md0 \
--level=1 \
--raid-devices=2 \
/dev/sdb /dev/sdc
```

Verify:

```bash
cat /proc/mdstat
```

Detailed information:

```bash
mdadm --detail /dev/md0
```

---

# 16. Create a Filesystem

Format:

```bash
mkfs.ext4 /dev/md0
```

Create mount point:

```bash
mkdir /data
```

Mount:

```bash
mount /dev/md0 /data
```

Verify:

```bash
df -h
```

---

# 17. Make RAID Persistent

Save configuration:

```bash
mdadm --detail --scan >> /etc/mdadm.conf
```

Some distributions use:

```
/etc/mdadm/mdadm.conf
```

Add to `/etc/fstab`:

```
/dev/md0   /data   ext4   defaults   0 0
```

---

# 18. Monitor RAID

View RAID status:

```bash
cat /proc/mdstat
```

Detailed status:

```bash
mdadm --detail /dev/md0
```

List block devices:

```bash
lsblk
```

---

# 19. Simulate Disk Failure

Mark a disk as failed:

```bash
mdadm /dev/md0 --fail /dev/sdb
```

Remove it:

```bash
mdadm /dev/md0 --remove /dev/sdb
```

Verify:

```bash
mdadm --detail /dev/md0
```

---

# 20. Replace Failed Disk

Add the replacement disk:

```bash
mdadm /dev/md0 --add /dev/sdb
```

The rebuild begins automatically.

Monitor:

```bash
cat /proc/mdstat
```

---

# 21. Grow a RAID Array

Example (adding another disk to RAID 5):

```bash
mdadm --grow /dev/md0 --raid-devices=4
```

Then add the new disk:

```bash
mdadm /dev/md0 --add /dev/sdd
```

Finally, expand the filesystem.

For ext4:

```bash
resize2fs /dev/md0
```

For XFS:

```bash
xfs_growfs /data
```

---

# 22. RAID and LVM

A common enterprise design is:

```
Physical Disks

↓

Software RAID (mdadm)

↓

LVM

↓

Filesystem

↓

Applications
```

Benefits:

- RAID provides redundancy.
- LVM provides flexible resizing.
- Easy storage expansion.

---

# 23. Performance Considerations

| RAID Level | Read Speed | Write Speed |
|------------|------------|-------------|
| RAID 0 | Excellent | Excellent |
| RAID 1 | Excellent | Good |
| RAID 5 | Excellent | Moderate (parity overhead) |
| RAID 6 | Good | Moderate |
| RAID 10 | Excellent | Excellent |

---

# 24. Best Practices

- Use identical disks whenever possible.
- Monitor RAID health regularly.
- Keep a hot spare for critical systems.
- Replace failed disks immediately.
- Test rebuild procedures.
- Combine RAID with LVM for flexibility.
- Always maintain external backups.
- Monitor SMART disk health using `smartctl`.

---

# 25. Troubleshooting

## RAID Not Assembled

```bash
mdadm --assemble --scan
```

---

## View Current Arrays

```bash
cat /proc/mdstat
```

---

## Detailed Information

```bash
mdadm --detail /dev/md0
```

---

## Disk Not Recognized

Check:

```bash
lsblk
```

or

```bash
fdisk -l
```

---

## Filesystem Will Not Mount

Check:

```bash
fsck /dev/md0
```

> Run `fsck` only on an unmounted filesystem.

---

# 26. Interview Questions

### Q1. What is RAID?

RAID is a technology that combines multiple disks into a single logical storage unit to improve performance, redundancy, or both.

---

### Q2. Is RAID a backup?

No. RAID protects against disk failures but does not protect against accidental deletion, corruption, ransomware, or site disasters.

---

### Q3. Which RAID level has no redundancy?

RAID 0.

---

### Q4. Which RAID level mirrors data?

RAID 1.

---

### Q5. Which RAID level uses parity?

RAID 5 and RAID 6.

---

### Q6. Which RAID level offers the best performance and redundancy?

RAID 10.

---

### Q7. Which command creates a Linux software RAID array?

```bash
mdadm --create
```

---

### Q8. Which file shows RAID status?

```bash
cat /proc/mdstat
```

---

# 27. Summary

RAID improves storage performance, availability, and fault tolerance by combining multiple disks into a logical array. Linux software RAID is managed with `mdadm`, allowing administrators to create, monitor, and repair RAID arrays without dedicated hardware. RAID 0 focuses on performance, RAID 1 on mirroring, RAID 5 and RAID 6 on parity-based protection, and RAID 10 combines striping with mirroring for high performance and resilience. In production, RAID is often combined with LVM for flexible storage management, but it should always be complemented by a reliable backup strategy.