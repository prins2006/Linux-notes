# System Backup using `dd` Command (Complete Guide)

## Table of Contents

1. Introduction
2. What is Backup?
3. Why Backup is Important?
4. Types of Backup
5. Backup Methods in Linux
6. What is dd?
7. How dd Works
8. dd Command Syntax
9. dd Parameters
10. Basic Examples
11. Disk Backup
12. Partition Backup
13. Disk Cloning
14. Restore Backup
15. Create Bootable USB
16. Backup MBR
17. Performance Testing
18. Secure Disk Erase
19. Compress Backup
20. Verify Backup
21. Advantages
22. Disadvantages
23. Best Practices
24. Troubleshooting
25. Interview Questions
26. Summary

---

# 1. Introduction

Data is one of the most valuable assets in any organization.

Hardware failures, accidental deletion, ransomware, power failures, software bugs, and natural disasters can all result in data loss.

A backup allows administrators to restore lost or damaged data.

Linux provides several backup tools such as:

- dd
- tar
- rsync
- dump
- cpio
- Clonezilla
- Bacula
- Amanda

Among these, **dd** is one of the oldest and most powerful utilities for creating exact disk copies.

---

# 2. What is Backup?

A backup is a duplicate copy of data stored in another location.

Purpose:

- Protect data
- Recover after failure
- Disaster recovery
- Business continuity

Example

```
Production Server

↓

Backup Server

↓

Restore if Needed
```

---

# 3. Why Backup is Important?

Without backup

```
Hard Disk Failure

↓

All Data Lost
```

With backup

```
Hard Disk Failure

↓

Restore Backup

↓

System Operational
```

Benefits

- Recover deleted files
- Recover entire server
- Recover boot loader
- Recover operating system
- Recover databases
- Disaster recovery

---

# 4. Types of Backup

## 4.1 Full Backup

Copies all selected data every time.

Example

```
Monday

Backup Everything

↓

100 GB
```

Advantages

- Fast restore
- Easy management
- Complete copy

Disadvantages

- Large storage
- Longer backup time

Examples

- dd
- tar
- Clonezilla

---

## 4.2 Incremental Backup

Copies only files changed since the last backup.

Example

```
Monday

Full Backup

↓

Tuesday

Changed Files

↓

Wednesday

Changed Files
```

Restore requires

```
Full Backup

+

All Incremental Backups
```

Advantages

- Small backups
- Fast backup
- Saves storage

Disadvantages

- Slow restore
- Backup chain dependency

Common Tools

- rsync
- Bacula

---

## 4.3 Differential Backup

Copies all changes since the last Full Backup.

Example

```
Monday

Full Backup

↓

Tuesday

Changed Files

↓

Wednesday

Tuesday + Wednesday Changes
```

Restore requires

```
Full Backup

+

Latest Differential Backup
```

Advantages

- Faster restore than Incremental
- Easier management

Disadvantages

- Backup size increases every day until the next Full Backup

---

# 5. Backup Methods in Linux

| Method | Description |
|----------|-------------|
| File Backup | Backup files and directories |
| Partition Backup | Backup one partition |
| Disk Backup | Backup complete disk |
| Image Backup | Sector-by-sector image |
| Snapshot | Point-in-time copy |
| Remote Backup | Backup to another server |
| Cloud Backup | Backup to cloud storage |

---

# 6. What is dd?

`dd` is a Linux command-line utility that performs low-level block copying.

Unlike `cp`, which copies files, `dd` copies raw data blocks.

It can copy:

- Entire hard disks
- SSDs
- USB drives
- Partitions
- ISO images
- Boot sectors
- Raw device data

Because it copies blocks directly, the destination is an exact copy of the source.

---

# 7. How dd Works

```
Input Device

↓

Read Block

↓

Memory Buffer

↓

Write Block

↓

Output Device
```

Example

```
/dev/sda

↓

dd

↓

backup.img
```

---

# 8. Syntax

```bash
dd if=<input> of=<output> [options]
```

Example

```bash
dd if=/dev/sda of=/backup/sda.img bs=64K status=progress
```

---

# 9. dd Parameters

| Option | Description |
|----------|-------------|
| if= | Input file/device |
| of= | Output file/device |
| bs= | Block size |
| count= | Number of blocks |
| skip= | Skip input blocks |
| seek= | Skip output blocks |
| status=progress | Display progress |
| conv=noerror | Continue after read errors |
| conv=sync | Pad unreadable blocks |

---

# 10. Basic Examples

## Copy One File

```bash
dd if=file1.txt of=file2.txt
```

---

## Create Empty File

```bash
dd if=/dev/zero of=test.img bs=1M count=100
```

Creates

```
100 MB File
```

---

# 11. Backup Entire Disk

Disk

```
/dev/sda
```

Backup

```bash
dd if=/dev/sda of=/backup/sda.img bs=64K status=progress
```

Explanation

```
Input

↓

/dev/sda

↓

Output

↓

backup image
```

---

# 12. Backup a Partition

```bash
dd if=/dev/sda1 of=/backup/root.img bs=64K status=progress
```

Creates an image of only one partition.

---

# 13. Clone Disk

Clone one disk to another.

```bash
dd if=/dev/sda of=/dev/sdb bs=64K status=progress
```

Result

```
Disk A

↓

Exact Copy

↓

Disk B
```

Warning

Everything on `/dev/sdb` is overwritten.

---

# 14. Restore Disk

Restore image

```bash
dd if=/backup/sda.img of=/dev/sda bs=64K status=progress
```

Restore partition

```bash
dd if=/backup/root.img of=/dev/sda1 bs=64K status=progress
```

---

# 15. Create Bootable USB

Example

```bash
dd if=rockylinux.iso of=/dev/sdb bs=4M status=progress oflag=sync
```

Important

Write to the USB device (`/dev/sdb`), **not** to a partition (`/dev/sdb1`).

---

# 16. Backup MBR

Backup

```bash
dd if=/dev/sda of=mbr_backup.bin bs=512 count=1
```

Restore

```bash
dd if=mbr_backup.bin of=/dev/sda bs=512 count=1
```

The MBR occupies the first 512 bytes of an MBR-partitioned disk.

---

# 17. Performance Testing

Write speed

```bash
dd if=/dev/zero of=testfile bs=1G count=1 oflag=direct status=progress
```

Read speed

```bash
dd if=testfile of=/dev/null bs=1G status=progress
```

---

# 18. Secure Disk Erase

Overwrite the disk with zeros

```bash
dd if=/dev/zero of=/dev/sdb bs=1M status=progress
```

Overwrite with random data

```bash
dd if=/dev/urandom of=/dev/sdb bs=1M status=progress
```

> For SSDs and NVMe drives, use manufacturer-supported secure erase or sanitize commands when appropriate.

---

# 19. Compress Backup

Create compressed backup

```bash
dd if=/dev/sda bs=64K status=progress | gzip > sda.img.gz
```

Restore

```bash
gunzip -c sda.img.gz | dd of=/dev/sda bs=64K status=progress
```

---

# 20. Verify Backup

Calculate checksum

```bash
sha256sum sda.img
```

Compare after restore

```bash
sha256sum restored.img
```

Check filesystem

```bash
fsck /dev/sda1
```

Verify mounted filesystem

```bash
df -h
```

---

# 21. Advantages

- Exact bit-for-bit copy
- Copies boot loader
- Copies partition table
- Useful for forensic imaging
- Good for disaster recovery
- Supports compression through pipes
- Creates bootable images

---

# 22. Disadvantages

- Slow on large disks
- Copies unused space
- No built-in compression
- No incremental backup support
- Can overwrite disks without confirmation
- Requires careful device selection

---

# 23. Best Practices

- Double-check `if=` and `of=` before pressing Enter.
- Use `status=progress` for large copies.
- Backup unmounted filesystems whenever possible.
- Store backups on separate storage.
- Verify backups with checksums.
- Compress large image files.
- Test restoration regularly.
- Maintain multiple backup copies (3-2-1 rule).

---

# 24. Troubleshooting

## Permission Denied

Run

```bash
sudo dd ...
```

---

## No Space Left

Check destination

```bash
df -h
```

---

## Wrong Device

Verify devices

```bash
lsblk
```

---

## Slow Copy

Increase block size

```bash
bs=64K
```

or

```bash
bs=1M
```

---

## Interrupted Copy

Restart the copy.

`dd` does not automatically resume interrupted operations.

---

# 25. Interview Questions

### Q1. What is `dd`?

A block-level copying utility used to copy disks, partitions, files, and device data.

---

### Q2. What is the difference between `cp` and `dd`?

| cp | dd |
|----|----|
| File-level copy | Block-level copy |
| Cannot clone disks | Can clone disks |
| Cannot copy boot sectors | Can copy boot sectors |

---

### Q3. Which command clones one disk?

```bash
dd if=/dev/sda of=/dev/sdb bs=64K status=progress
```

---

### Q4. Which option displays progress?

```bash
status=progress
```

---

### Q5. Which command creates a bootable USB?

```bash
dd if=linux.iso of=/dev/sdb bs=4M status=progress oflag=sync
```

---

### Q6. Can `dd` perform incremental backups?

No.

It always performs a block-level copy.

---

### Q7. What does `bs=` mean?

It specifies the block size used during copying.

---

# 26. Summary

The `dd` command is a powerful Linux utility for creating exact block-level copies of disks, partitions, and files. It is commonly used for disk imaging, system backup, disk cloning, bootable USB creation, MBR backup, and disaster recovery. While it is excellent for full image backups, it is not suitable for incremental backups because it copies raw blocks rather than tracking changed files. Because `dd` writes directly to storage devices without confirmation, administrators should always verify source and destination devices before executing the command.