# Add Disk and Create New LVM Partition
## Complete Guide to `pvcreate`, `vgcreate`, and `lvcreate`

**Level:** Beginner to Production  
**Operating System:** RHEL / CentOS / Rocky Linux / AlmaLinux / Ubuntu

---

# Table of Contents

1. Introduction
2. LVM Architecture
3. Prerequisites
4. Add a New Disk
5. Verify the New Disk
6. Create an LVM Partition (Optional)
7. Create a Physical Volume (pvcreate)
8. Create a Volume Group (vgcreate)
9. Create a Logical Volume (lvcreate)
10. Create a Filesystem
11. Mount the Logical Volume
12. Configure Permanent Mount
13. Verify LVM Configuration
14. Complete Production Example
15. Troubleshooting
16. Best Practices
17. Important Commands Summary

---

# 1. Introduction

Logical Volume Management (LVM) is a storage management technology in Linux that provides flexibility in managing disk storage. Unlike traditional partitions, LVM allows you to:

- Combine multiple disks into one storage pool.
- Resize storage without repartitioning.
- Extend storage online.
- Create snapshots.
- Manage storage dynamically.

Instead of directly using a disk partition, LVM introduces three layers:

```
Disk
   ↓
Physical Volume (PV)
   ↓
Volume Group (VG)
   ↓
Logical Volume (LV)
   ↓
Filesystem
   ↓
Mount Point
```

---

# 2. LVM Architecture

```
             +--------------------+
             |   Physical Disk    |
             |    /dev/sdb        |
             +---------+----------+
                       |
                 pvcreate
                       |
             +---------v----------+
             | Physical Volume    |
             |      (PV)          |
             +---------+----------+
                       |
                 vgcreate
                       |
             +---------v----------+
             | Volume Group (VG)  |
             |    Storage Pool    |
             +---------+----------+
                       |
                 lvcreate
                       |
             +---------v----------+
             | Logical Volume     |
             |      (LV)          |
             +---------+----------+
                       |
                 mkfs.ext4/xfs
                       |
                  Mount Point
```

---

# 3. Prerequisites

Before creating an LVM partition, ensure:

- Root or sudo access.
- LVM package installed.
- A new disk attached to the system.

Check installed disks:

```bash
lsblk
```

Example:

```
NAME   SIZE TYPE MOUNTPOINT
sda    100G disk
├─sda1   1G part /boot
├─sda2  99G part /
sdb    200G disk
```

Here:

- `sda` → Existing system disk
- `sdb` → Newly added disk

---

# 4. Add a New Disk

A new disk may come from:

- VMware
- VirtualBox
- Hyper-V
- AWS EBS
- Azure Managed Disk
- Physical Server

After attaching the disk, Linux detects it as:

```
/dev/sdb
```

or

```
/dev/sdc
```

or

```
/dev/sdd
```

depending on the system.

Verify:

```bash
lsblk
```

or

```bash
fdisk -l
```

---

# 5. Verify the New Disk

Display all block devices:

```bash
lsblk
```

Example:

```
NAME   SIZE TYPE
sda    100G disk
├─sda1
├─sda2

sdb    200G disk
```

Check detailed information:

```bash
fdisk -l
```

Example:

```
Disk /dev/sdb: 200 GiB
```

If the disk has no partitions, it is ready for LVM.

---

# 6. Create an LVM Partition (Optional)

You can use the entire disk directly with LVM or create a dedicated LVM partition.

Using `fdisk`:

```bash
fdisk /dev/sdb
```

Steps:

```
n   (new partition)

p   (primary)

1

Enter

Enter

t

8e (Linux LVM)

w
```

Verify:

```bash
lsblk
```

Output:

```
sdb
└── sdb1
```

The partition `/dev/sdb1` is now available.

---

# 7. Create a Physical Volume (pvcreate)

## What is a Physical Volume?

A Physical Volume (PV) is the first layer of LVM. It converts a disk or partition into an LVM-managed storage device.

Command:

```bash
pvcreate /dev/sdb1
```

Example:

```
Physical volume "/dev/sdb1" successfully created.
```

If using the entire disk:

```bash
pvcreate /dev/sdb
```

Verify:

```bash
pvs
```

Example:

```
PV         VG   Fmt  Attr PSize   PFree
/dev/sdb1       lvm2 ---  200.00g 200.00g
```

Detailed information:

```bash
pvdisplay
```

Shows:

- UUID
- Size
- Free space
- Metadata
- Physical Extents

---

# 8. Create a Volume Group (vgcreate)

## What is a Volume Group?

A Volume Group (VG) is a storage pool created from one or more Physical Volumes.

Create a Volume Group:

```bash
vgcreate vgdata /dev/sdb1
```

Output:

```
Volume group "vgdata" successfully created.
```

Verify:

```bash
vgs
```

Example:

```
VG      #PV #LV Size
vgdata    1   0 200G
```

Detailed view:

```bash
vgdisplay
```

Example:

```
VG Name

vgdata

VG Size

200 GB

Free PE

51199
```

---

# 9. Create a Logical Volume (lvcreate)

## What is a Logical Volume?

A Logical Volume behaves like a normal disk partition but is flexible and can be resized later.

Create a 100 GB Logical Volume:

```bash
lvcreate -L 100G -n lvdata vgdata
```

Explanation:

```
-L

Size

100G

100 Gigabytes

-n

Logical Volume Name

lvdata

vgdata

Volume Group Name
```

Output:

```
Logical volume "lvdata" created.
```

Verify:

```bash
lvs
```

Example:

```
LV      VG      Size
lvdata  vgdata 100G
```

Detailed information:

```bash
lvdisplay
```

Device path:

```
/dev/vgdata/lvdata
```

---

# 10. Create a Filesystem

LVM only creates storage.

A filesystem is required before storing data.

### EXT4

```bash
mkfs.ext4 /dev/vgdata/lvdata
```

### XFS

```bash
mkfs.xfs /dev/vgdata/lvdata
```

Example:

```
Creating filesystem...
Writing inode tables...
Done.
```

---

# 11. Mount the Logical Volume

Create a mount point:

```bash
mkdir /data
```

Mount:

```bash
mount /dev/vgdata/lvdata /data
```

Verify:

```bash
df -h
```

Example:

```
Filesystem

Size

Used

Mounted on

/dev/mapper/vgdata-lvdata

100G

1G

/data
```

---

# 12. Configure Permanent Mount

Find UUID:

```bash
blkid
```

Example:

```
UUID="9f3c..."
```

Edit:

```bash
vim /etc/fstab
```

Add:

```
/dev/vgdata/lvdata    /data    ext4    defaults    0 0
```

or

```
UUID=xxxxxxxx    /data    ext4    defaults    0 0
```

Test:

```bash
mount -a
```

No output means configuration is correct.

---

# 13. Verify LVM Configuration

Check disks:

```bash
lsblk
```

Check Physical Volumes:

```bash
pvs
```

Check Volume Groups:

```bash
vgs
```

Check Logical Volumes:

```bash
lvs
```

Check mounted filesystems:

```bash
df -h
```

---

# 14. Complete Production Example

Suppose:

```
New Disk

/dev/sdb

Size

200 GB
```

### Step 1

```bash
pvcreate /dev/sdb
```

### Step 2

```bash
vgcreate vgdata /dev/sdb
```

### Step 3

```bash
lvcreate -L 150G -n lvbackup vgdata
```

### Step 4

```bash
mkfs.xfs /dev/vgdata/lvbackup
```

### Step 5

```bash
mkdir /backup
```

### Step 6

```bash
mount /dev/vgdata/lvbackup /backup
```

### Step 7

```bash
echo "/dev/vgdata/lvbackup /backup xfs defaults 0 0" >> /etc/fstab
```

Result:

```
Disk

↓

PV

↓

VG

↓

LV

↓

Filesystem

↓

Mounted at

/backup
```

---

# 15. Troubleshooting

### PV already exists

```
Device already initialized.
```

Solution:

```bash
pvremove /dev/sdb
```

---

### VG name already exists

Check:

```bash
vgs
```

Choose another name.

---

### LV creation fails

Reason:

```
Not enough free space.
```

Check:

```bash
vgs
```

---

### Filesystem not mounting

Check:

```bash
blkid
```

Verify `/etc/fstab` entry.

---

# 16. Best Practices

- Use meaningful names (`vg_data`, `lv_backup`).
- Prefer UUIDs in `/etc/fstab`.
- Leave free space in the Volume Group for future expansion.
- Use XFS for large production file systems.
- Regularly back up important data before making storage changes.
- Monitor storage usage using `df -h` and `lvs`.

---

# 17. Important Commands Summary

| Command | Purpose |
|----------|---------|
| `lsblk` | List block devices |
| `fdisk -l` | Show disk information |
| `pvcreate` | Create Physical Volume |
| `pvs` | List Physical Volumes |
| `pvdisplay` | Detailed PV information |
| `vgcreate` | Create Volume Group |
| `vgs` | List Volume Groups |
| `vgdisplay` | Detailed VG information |
| `lvcreate` | Create Logical Volume |
| `lvs` | List Logical Volumes |
| `lvdisplay` | Detailed LV information |
| `mkfs.ext4` | Create ext4 filesystem |
| `mkfs.xfs` | Create XFS filesystem |
| `mount` | Mount filesystem |
| `df -h` | Display mounted filesystems |
| `blkid` | Display UUID information |

---

# Storage Flow Summary

```
New Disk (/dev/sdb)
        │
        ▼
pvcreate
        │
        ▼
Physical Volume (PV)
        │
        ▼
vgcreate
        │
        ▼
Volume Group (VG)
        │
        ▼
lvcreate
        │
        ▼
Logical Volume (LV)
        │
        ▼
mkfs.ext4 / mkfs.xfs
        │
        ▼
Mount Point (/data)
        │
        ▼
Application Stores Data
```

---

# Interview Questions

1. What is the difference between a Physical Volume (PV) and a Volume Group (VG)?
2. Can `pvcreate` be performed on an entire disk or only on a partition?
3. What is the purpose of a Volume Group?
4. Why is a Logical Volume more flexible than a traditional partition?
5. Why do we need to create a filesystem after `lvcreate`?
6. What is the purpose of `/etc/fstab`?
7. Which commands verify PV, VG, and LV?
8. What happens if the filesystem is not mounted after creating an LV?
9. Can multiple disks be combined into a single Volume Group?
10. What are the advantages of using LVM in production environments?