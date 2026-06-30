# Logical Volume Management (LVM)
# Complete Beginner to Production Guide
## Part 1 - Storage Fundamentals & LVM Architecture

---

# Table of Contents

1. Introduction
2. What is Storage?
3. Traditional Disk Partitioning
4. Problems with Traditional Partitions
5. What is LVM?
6. Why LVM was Created
7. LVM Architecture
8. Components of LVM
9. Physical Volume (PV)
10. Volume Group (VG)
11. Logical Volume (LV)
12. Physical Extents (PE)
13. Logical Extents (LE)
14. LVM Metadata
15. Storage Flow
16. Complete Architecture Diagram
17. Advantages
18. Disadvantages
19. Production Use Cases
20. Basic Commands Overview
21. Summary

---

# 1. Introduction

Storage is one of the most important components of every Linux server.

Every application stores its data somewhere.

Examples:

• Database
• Web Server
• Mail Server
• Docker
• Kubernetes
• Virtual Machines
• Backup Server

Without storage,
there is no operating system.

---

# Traditional Storage

Disk
 ↓

Partition

↓

Filesystem

↓

Mount Point

Example

/dev/sda

↓

/dev/sda1

↓

ext4

↓

/

---

Example

Disk

500 GB

Partition 1

100 GB

Partition 2

100 GB

Partition 3

300 GB

The sizes become fixed.

If Partition 1 becomes full,
space from Partition 3 cannot be used directly.

This is the biggest problem.

---

# 2. What is Storage?

Storage is a device used for storing data permanently.

Examples

Hard Disk (HDD)

Solid State Drive (SSD)

NVMe SSD

SAN

NAS

Cloud Block Storage

AWS EBS

Azure Managed Disk

Google Persistent Disk

---

Example

Application

↓

Filesystem

↓

Storage

↓

Physical Disk

---

# 3. Traditional Partitioning

Linux stores data inside partitions.

Disk

↓

Partition

↓

Filesystem

↓

Directory

Example

Disk

/dev/sda

Partitions

/dev/sda1

/dev/sda2

/dev/sda3

Filesystem

ext4

xfs

Mount

/

/home

/data

---

Creating Partition

Disk

500GB

Create

100GB

200GB

200GB

Done.

Sizes cannot grow automatically.

---

Problem

Database grows.

Database partition becomes full.

Disk still has free space.

Cannot use it without repartitioning.

Downtime required.

---

# 4. Problems with Traditional Partitioning

Problem 1

Fixed Size

100GB means exactly 100GB.

Cannot expand easily.

---

Problem 2

Downtime

Changing partitions often requires reboot.

---

Problem 3

Poor Flexibility

Unused disk space cannot easily be shared.

---

Problem 4

Migration is difficult.

---

Problem 5

Backup complexity.

---

# 5. What is LVM?

LVM stands for

Logical Volume Manager.

It is a storage virtualization layer.

Instead of using partitions directly,

Linux creates virtual storage.

Application

↓

Filesystem

↓

Logical Volume

↓

Volume Group

↓

Physical Volume

↓

Disk

---

Think Like This

Physical Disk

↓

Storage Pool

↓

Virtual Disk

Applications use the virtual disk.

---

Simple Definition

LVM combines multiple disks into one storage pool.

Then creates flexible partitions from that pool.

---

# Why LVM?

Suppose

Disk1 = 500GB

Disk2 = 500GB

Traditional

Cannot combine easily.

LVM

Creates

1000GB Storage Pool

Now create

100GB

300GB

250GB

150GB

Any size.

---

# Real Life Example

Imagine Water Tanks.

Tank A

500 Liters

Tank B

500 Liters

LVM joins them.

Now

1000 Liter Tank

Applications only see

1000 Liters.

---

# 6. Why was LVM Created?

Large servers required

Growing storage.

Databases

Virtualization

Cloud

Backup

Traditional partitions failed.

LVM solved this.

---

Production Examples

Oracle Database

SAP

MySQL

PostgreSQL

VMware

OpenStack

Docker

Kubernetes Worker Nodes

All commonly use LVM.

---

# 7. LVM Architecture

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

↓

Application

---

Detailed Diagram

           +----------------------+
           |    Physical Disk     |
           |    /dev/sdb          |
           +----------+-----------+
                      |
                      |
                pvcreate
                      |
                      v
           +----------------------+
           | Physical Volume      |
           |       PV             |
           +----------+-----------+
                      |
                      |
                 vgcreate
                      |
                      v
           +----------------------+
           | Volume Group (VG)    |
           | Storage Pool         |
           +----------+-----------+
                      |
            +---------+----------+
            |                    |
      lvcreate              lvcreate
            |                    |
            v                    v
      +-----------+       +-----------+
      | LV_HOME   |       | LV_DATA   |
      +-----------+       +-----------+
            |                    |
         mkfs.ext4            mkfs.xfs
            |                    |
         Mount                 Mount

---

# 8. Components of LVM

Three major components

1. PV

2. VG

3. LV

Everything revolves around these.

---

# 9. Physical Volume (PV)

Physical Volume is the actual disk.

Examples

/dev/sdb

/dev/sdc

/dev/sdd

Command

pvcreate /dev/sdb

Now

Disk becomes

Physical Volume.

---

Information stored

UUID

Metadata

Physical Extents

Allocation information

---

View PV

pvs

or

pvdisplay

Example Output

PV

/dev/sdb

Size

100 GB

Free

100 GB

---

# 10. Volume Group (VG)

VG is a storage pool.

It combines one or more PVs.

Example

Disk1

100GB

Disk2

100GB

Disk3

100GB

↓

VG

300GB

Applications never use VG directly.

VG only provides storage.

---

Create VG

vgcreate vgdata /dev/sdb /dev/sdc

Now

VG Size

200GB

---

Display

vgs

vgdisplay

---

# 11. Logical Volume (LV)

Logical Volume behaves like a partition.

Applications use LV.

Filesystem goes on LV.

Example

VG

300GB

Create

LV1

100GB

LV2

50GB

LV3

150GB

---

Command

lvcreate -L 50G -n lvbackup vgdata

Result

/dev/vgdata/lvbackup

---

# 12. Physical Extents (PE)

LVM divides disks into equal blocks.

These blocks are called

Physical Extents.

Default

4MB

Example

100GB Disk

↓

4MB blocks

↓

Thousands of PE

---

Think Like

Notebook

Each page

=

Physical Extent

---

# 13. Logical Extents (LE)

Logical Volume also divides into blocks.

Called

Logical Extents.

PE

maps to

LE

PE == LE size

Usually

4MB

---

# Mapping

PE1

↓

LE1

PE2

↓

LE2

PE3

↓

LE3

---

# 14. LVM Metadata

Metadata stores

VG Name

PV UUID

LV UUID

PE allocation

Free Space

Snapshots

Mirror information

Without metadata

LVM cannot work.

---

# 15. Storage Flow

Application

↓

Filesystem

↓

Logical Volume

↓

Volume Group

↓

Physical Volume

↓

Hard Disk

---

Example

Apache

↓

/var/www

↓

LV_WEB

↓

VG_WEB

↓

PV1

↓

SSD

---

# 16. Complete Architecture

        Application
             |
             |
      +--------------+
      | Filesystem   |
      +--------------+
             |
      +--------------+
      | Logical Vol. |
      +--------------+
             |
      +--------------+
      | Volume Group |
      +--------------+
        |          |
        |          |
 +-----------+ +-----------+
 | PV sdb    | | PV sdc    |
 +-----------+ +-----------+
        |           |
      Disk1       Disk2

---

# 17. Advantages

✔ Flexible

✔ Easy expansion

✔ Multiple disks

✔ Snapshots

✔ Easy migration

✔ Better management

✔ Online resizing

✔ Production ready

---

# 18. Disadvantages

Slight complexity

Requires understanding

Metadata corruption affects storage

Not suitable for removable USB drives

---

# 19. Production Use Cases

Database Servers

Docker Hosts

Virtualization

VM Storage

Backup Servers

File Servers

Cloud Servers

Enterprise Linux

---

# 20. Common Commands

Display PV

pvs

Detailed PV

pvdisplay

Display VG

vgs

Detailed VG

vgdisplay

Display LV

lvs

Detailed LV

lvdisplay

List Block Devices

lsblk

Filesystem Usage

df -h

Show UUID

blkid

---

# 21. Summary

Disk

↓

Physical Volume

↓

Volume Group

↓

Logical Volume

↓

Filesystem

↓

Mount

↓

Application

Remember

PV = Physical Disk

VG = Storage Pool

LV = Virtual Partition

Filesystem = ext4/xfs

Mount Point = Directory

Application stores data inside mounted filesystem.

---

# Key Interview Questions

1. What is LVM?

2. Difference between partition and LVM?

3. What is PV?

4. What is VG?

5. What is LV?

6. What are PE and LE?

7. Why is LVM better than traditional partitions?

8. Can multiple disks be combined in LVM?

9. Does LVM support snapshots?

10. Where is LVM metadata stored?

---

# End of Part 1

Next Part:

✔ Install LVM
✔ Create PV
✔ Create VG
✔ Create LV
✔ Format Filesystem
✔ Mount
✔ /etc/fstab
✔ Extend Storage
✔ Reduce Storage
✔ Remove LVM
✔ Complete Production Lab