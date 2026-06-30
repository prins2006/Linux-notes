# Extend Disk Using LVM (Logical Volume Management)

**Level:** Beginner to Production
**Operating System:** RHEL 7/8/9, Rocky Linux, AlmaLinux, CentOS, Ubuntu

---

# Table of Contents

1. Introduction
2. Why Extend Storage?
3. Traditional Partition vs LVM
4. LVM Architecture
5. Types of LVM Expansion
6. Storage Expansion Workflow
7. Scenario 1 – Extend Using Free Space in Existing VG
8. Scenario 2 – Add New Disk and Extend VG
9. Understanding Important Commands

---

# 1. Introduction

Storage requirements in production servers increase over time. Databases, application logs, backups, Docker containers, and virtual machines consume additional disk space. If the filesystem becomes full, applications may stop functioning correctly.

Traditional partitions are difficult to resize, while **Logical Volume Management (LVM)** allows storage to be expanded with minimal downtime.

LVM enables administrators to:

- Expand storage online.
- Combine multiple disks into one storage pool.
- Resize logical volumes.
- Resize filesystems.
- Add new disks without reinstalling the operating system.

---

# 2. Why Extend Storage?

Common reasons for extending disk space include:

- Database growth
- Log file growth
- Application data increase
- User home directories becoming full
- Virtual machine storage expansion
- Backup storage requirements

Example:

```
Filesystem      Size Used Avail Use%
/dev/vgdata/lvdata
                100G 99G   1G   99%
```

The logical volume is almost full. More storage is required.

---

# 3. Traditional Partition vs LVM

## Traditional Partition

```
Disk
│
├── sda1 (/boot)
├── sda2 (/)
└── sda3 (/data)
```

Problems:

- Fixed size
- Difficult to resize
- Often requires downtime

---

## LVM

```
Disk
 │
 ▼
Physical Volume
 │
 ▼
Volume Group
 │
 ▼
Logical Volume
 │
 ▼
Filesystem
```

Advantages:

- Easy expansion
- Online resizing
- Multiple disks
- Flexible storage

---

# 4. LVM Architecture

```
          +----------------+
          | Physical Disk  |
          |   /dev/sdb     |
          +-------+--------+
                  |
             pvcreate
                  |
                  ▼
          +----------------+
          | Physical Volume|
          |      PV        |
          +-------+--------+
                  |
             vgcreate/vgextend
                  |
                  ▼
          +----------------+
          | Volume Group   |
          |      VG        |
          +-------+--------+
                  |
             lvcreate/lvextend
                  |
                  ▼
          +----------------+
          | Logical Volume |
          |      LV        |
          +-------+--------+
                  |
             resize2fs
             xfs_growfs
                  |
                  ▼
            Mounted Filesystem
```

---

# 5. Types of LVM Expansion

There are two common methods.

## Method 1

Extend using free space already available in the Volume Group.

```
VG Size

500GB

Used

300GB

Free

200GB

Increase LV from

100GB

to

200GB
```

No new disk is required.

---

## Method 2

Add a new disk.

```
Current Disk

200GB

New Disk

300GB

↓

VG becomes

500GB

↓

Increase LV

200GB

to

450GB
```

---

# 6. Storage Expansion Workflow

Whenever storage needs to be increased, follow this order:

```
Add Disk
      │
      ▼
pvcreate
      │
      ▼
vgextend
      │
      ▼
lvextend
      │
      ▼
Resize Filesystem
      │
      ▼
Verify
```

Never resize the filesystem before extending the logical volume.

---

# 7. Scenario 1 – Extend Using Existing Free Space

Current layout:

```
VG Size

300 GB

LV Size

100 GB

VG Free

200 GB
```

Check Volume Groups:

```bash
vgs
```

Example:

```
VG      #PV #LV Attr VSize   VFree

vgdata    1   1 wz--n- 300G 200G
```

There is already 200 GB free inside the Volume Group.

Check Logical Volumes:

```bash
lvs
```

Example:

```
LV

VG

LSize

lvdata

vgdata

100G
```

Increase Logical Volume by 50 GB:

```bash
lvextend -L +50G /dev/vgdata/lvdata
```

Output:

```
Size of logical volume vgdata/lvdata changed from 100.00 GiB to 150.00 GiB.
```

Alternative:

Use all remaining free space.

```bash
lvextend -l +100%FREE /dev/vgdata/lvdata
```

---

# 8. Scenario 2 – Add a New Disk

Suppose the Volume Group has no free space.

Current:

```
VG

200GB

Used

200GB

Free

0GB
```

Attach a new disk.

Example:

```
/dev/sdc

100GB
```

Verify:

```bash
lsblk
```

Output:

```
NAME

SIZE

sda

100G

sdb

200G

sdc

100G
```

Create Physical Volume:

```bash
pvcreate /dev/sdc
```

Output:

```
Physical volume "/dev/sdc" successfully created.
```

Verify:

```bash
pvs
```

Example:

```
PV

VG

Size

PFree

/dev/sdc

100G

100G
```

Now extend the Volume Group:

```bash
vgextend vgdata /dev/sdc
```

Output:

```
Volume group "vgdata" successfully extended.
```

Verify:

```bash
vgs
```

Example:

```
VG

VSize

VFree

vgdata

300G

100G
```

The Volume Group now has additional free space that can be assigned to any Logical Volume.

---

# 9. Important Commands

## Display Physical Volumes

```bash
pvs
```

Detailed information:

```bash
pvdisplay
```

---

## Display Volume Groups

```bash
vgs
```

Detailed information:

```bash
vgdisplay
```

---

## Display Logical Volumes

```bash
lvs
```

Detailed information:

```bash
lvdisplay
```

---

## Display Filesystem Usage

```bash
df -h
```

---

## Display Block Devices

```bash
lsblk
```

---

# Summary

LVM expansion follows four major steps:

1. Add new disk (if required)
2. Create Physical Volume (`pvcreate`)
3. Extend Volume Group (`vgextend`)
4. Extend Logical Volume (`lvextend`)
5. Resize the filesystem (`resize2fs` or `xfs_growfs`)

The logical volume is expanded first, followed by the filesystem. Only after the filesystem is resized does the operating system recognize the additional usable space.

---

**End of Part 1**

**Part 2 will cover:**

- `lvextend` in depth
- `resize2fs` (EXT4)
- `xfs_growfs` (XFS)
- Online filesystem expansion
- Complete production lab
- Troubleshooting
- Best practices
- Interview questions