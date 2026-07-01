# Extend Disk using LVM (Logical Volume Manager)

## Table of Contents

1. Introduction
2. LVM Architecture
3. Why Extend a Disk?
4. Lab Environment
5. Existing LVM Layout
6. Method 1: Extend Using Free Space in Existing Volume Group
7. Method 2: Add a New Disk and Extend LVM
8. Step-by-Step Example
9. Filesystem Expansion
10. Verification Commands
11. Online vs Offline Extension
12. Extend Root (/) Partition
13. Extend Swap Logical Volume
14. Reduce Logical Volume (Important)
15. Common Errors
16. Troubleshooting
17. Best Practices
18. Interview Questions
19. Summary

---

# 1. Introduction

One of the biggest advantages of **Logical Volume Manager (LVM)** is that storage can be increased without reinstalling Linux or recreating partitions.

Traditional partitions have fixed sizes.

Example:

```
/dev/sda1 = 50GB
```

If the partition becomes full:

```
Disk Full
```

You cannot simply increase it unless free space exists immediately after the partition.

LVM solves this problem.

With LVM you can:

- Add new disks
- Combine multiple disks
- Extend storage online
- Resize filesystem
- Create snapshots
- Migrate storage

---

# 2. LVM Architecture

```
New Disk
   │
   ▼
Physical Volume (PV)
   │
   ▼
Volume Group (VG)
   │
   ▼
Logical Volume (LV)
   │
   ▼
Filesystem
   │
   ▼
Mount Point (/home,/data,/var)
```

Example

```
Disk:
/dev/sdb

↓

PV:
/dev/sdb1

↓

VG:
vg_data

↓

LV:
lv_data

↓

Filesystem:
ext4

↓

Mounted:
/data
```

---

# 3. Why Extend a Disk?

Suppose your application stores logs.

```
/var
```

Initially

```
50GB
```

After one year

```
Used:
49GB

Available:
1GB
```

Soon

```
Filesystem Full
```

Instead of reinstalling Linux, simply:

```
Add new disk

↓

Create PV

↓

Add PV to VG

↓

Extend LV

↓

Resize Filesystem
```

Done!

---

# 4. Lab Environment

Current system

```
Disk

/dev/sda = 100GB

Root VG

vg_root

Logical Volumes

lv_root
lv_home
lv_var
```

Suppose

```
lv_var

Size

20GB

Need

50GB
```

New disk added

```
/dev/sdb

30GB
```

Goal

```
Increase /var

20GB

↓

50GB
```

---

# 5. Existing LVM Layout

Check disks

```bash
lsblk
```

Example

```
NAME        SIZE TYPE MOUNTPOINT

sda         100G disk
├─sda1       1G part /boot
└─sda2      99G part

  ├─vg-root 40G /
  ├─vg-home 30G /home
  └─vg-var  20G /var
```

Display filesystem

```bash
df -h
```

Example

```
Filesystem              Size Used Avail

/dev/vg/lv_var          20G 18G 2G
```

---

# 6. Check Available Free Space in VG

Display VG

```bash
vgs
```

Example

```
VG

vg

VSize

100G

VFree

15G
```

If VFree exists, no new disk is required.

---

# 7. Method 1: Extend LV Using Existing Free Space

Current

```
VG Free

15GB
```

Extend LV

```bash
lvextend -L +10G /dev/vg/lv_var
```

Meaning

```
-L

Increase size

+10G

Add 10GB
```

New Size

```
20GB

↓

30GB
```

Filesystem still remains

```
20GB
```

Need resize.

---

# 8. Resize ext4 Filesystem

Command

```bash
resize2fs /dev/vg/lv_var
```

Now filesystem becomes

```
30GB
```

Verify

```bash
df -h
```

---

# 9. XFS Filesystem

Check filesystem

```bash
df -Th
```

Example

```
Filesystem

xfs
```

Resize

```bash
xfs_growfs /var
```

Notice

```
Never use resize2fs on XFS.
```

---

# 10. Method 2: Add New Disk

Suppose new disk added

```
/dev/sdb
```

Check

```bash
lsblk
```

Output

```
sdb

30G
```

---

# Step 1

Create partition

```bash
fdisk /dev/sdb
```

Commands

```
n
p
1

t
8e

w
```

Verify

```bash
lsblk
```

```
sdb1
```

---

# Step 2

Create Physical Volume

```bash
pvcreate /dev/sdb1
```

Output

```
Physical volume successfully created
```

Verify

```bash
pvs
```

---

# Step 3

Extend Volume Group

Current

```
VG

vg
```

Command

```bash
vgextend vg /dev/sdb1
```

Output

```
Volume group successfully extended
```

Check

```bash
vgs
```

Before

```
VFree

0GB
```

After

```
30GB
```

---

# Step 4

Extend Logical Volume

```bash
lvextend -L +30G /dev/vg/lv_var
```

or

```bash
lvextend -l +100%FREE /dev/vg/lv_var
```

Meaning

```
-l

Extent based

+100%FREE

Use all free space
```

---

# Step 5

Resize Filesystem

For ext4

```bash
resize2fs /dev/vg/lv_var
```

For XFS

```bash
xfs_growfs /var
```

---

# 11. Single Command (Recommended)

Modern LVM

```bash
lvextend -r -L +20G /dev/vg/lv_var
```

Meaning

```
-r

Resize filesystem automatically
```

Equivalent to

```
lvextend

+

resize2fs
```

One command does both.

---

# 12. Extend Root (/) Partition

Display

```bash
df -h /
```

Suppose

```
20GB

Need

40GB
```

Command

```bash
lvextend -r -L +20G /dev/vg/root
```

Verify

```bash
df -h /
```

No reboot required.

---

# 13. Extend Home Partition

Current

```
/home

50GB
```

Need

80GB

Command

```bash
lvextend -r -L +30G /dev/vg/home
```

Done.

---

# 14. Extend Using Percentage

Increase by

50%

```bash
lvextend -l +50%FREE /dev/vg/lv_home
```

Use all free space

```bash
lvextend -l +100%FREE /dev/vg/lv_home
```

---

# 15. Check Everything

Display PV

```bash
pvs
```

Display VG

```bash
vgs
```

Display LV

```bash
lvs
```

Detailed

```bash
lvdisplay
```

Filesystem

```bash
df -h
```

Block devices

```bash
lsblk
```

---

# 16. Online vs Offline Extension

## Online

Server continues running.

```
No downtime

Applications continue

Filesystem grows online
```

Supported

```
XFS

ext4
```

---

## Offline

Filesystem unmounted.

Used when

- Older filesystem
- Shrinking
- Maintenance

---

# 17. Extend Swap LV

Disable

```bash
swapoff -a
```

Extend

```bash
lvextend -L +4G /dev/vg/swap
```

Create swap signature

```bash
mkswap /dev/vg/swap
```

Enable

```bash
swapon -a
```

Verify

```bash
swapon --show
```

---

# 18. Reduce Logical Volume (Danger)

Reducing is risky.

Correct order

```
Backup

↓

Unmount

↓

Filesystem Check

↓

Shrink Filesystem

↓

Reduce LV
```

Wrong order

```
Reduce LV first

↓

Filesystem Corruption
```

Always back up data before reducing.

---

# 19. Common Errors

## Error

```
Insufficient free space
```

Reason

VG has no free extents.

Solution

```
Add new PV

or

Delete unused LV
```

---

## Error

```
resize2fs:
Bad magic number
```

Reason

Filesystem is XFS.

Solution

```
Use xfs_growfs
```

---

## Error

```
Device busy
```

Reason

Filesystem mounted incorrectly or in use.

Check

```bash
mount
```

or

```bash
lsof
```

---

# 20. Troubleshooting

Check filesystem type

```bash
df -Th
```

Check mounted filesystem

```bash
mount
```

Check LV

```bash
lvs
```

Check VG

```bash
vgs
```

Check PV

```bash
pvs
```

Check kernel messages

```bash
dmesg
```

---

# 21. Best Practices

- Always take a backup before resizing storage.
- Verify filesystem type (ext4 or XFS) before resizing.
- Use `lvextend -r` to resize LV and filesystem together.
- Monitor free space using `vgs`.
- Use `+100%FREE` only when you want to consume all remaining VG space.
- Verify changes with `lsblk`, `lvs`, `vgs`, and `df -h`.
- Avoid reducing logical volumes unless absolutely necessary.
- Document storage changes for future maintenance.

---

# 22. Interview Questions

### Q1. Why is LVM better than traditional partitions?

**Answer:**
LVM allows online resizing, snapshots, flexible storage management, and combining multiple disks into a single storage pool.

---

### Q2. What is the difference between `lvextend` and `resize2fs`?

**Answer:**
- `lvextend` increases the size of the logical volume.
- `resize2fs` expands the ext4 filesystem to use the new space.

---

### Q3. How do you extend an XFS filesystem?

**Answer:**

```bash
xfs_growfs <mount_point>
```

Example:

```bash
xfs_growfs /data
```

---

### Q4. What does `lvextend -r` do?

**Answer:**
It extends the logical volume and automatically resizes the filesystem in a single command.

---

### Q5. Which commands display LVM information?

| Command | Purpose |
|---------|----------|
| `pvs` | Show Physical Volumes |
| `vgs` | Show Volume Groups |
| `lvs` | Show Logical Volumes |
| `pvdisplay` | Detailed PV information |
| `vgdisplay` | Detailed VG information |
| `lvdisplay` | Detailed LV information |

---

# 23. Summary

LVM provides flexible storage management by separating physical storage from logical storage. Extending a filesystem typically involves checking available VG space, extending the logical volume with `lvextend`, and resizing the filesystem using `resize2fs` (for ext4) or `xfs_growfs` (for XFS). If no free space exists, a new disk can be added as a Physical Volume (PV), incorporated into the Volume Group (VG), and then allocated to the desired Logical Volume (LV). Modern systems simplify this process using `lvextend -r`, which resizes both the LV and filesystem in one step, often without requiring downtime.