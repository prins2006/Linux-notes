# Adding Swap Space in Linux

## Table of Contents

1. Introduction to Swap Space
2. Why Swap is Needed
3. How Swap Works
4. Types of Swap
5. Check Existing Swap
6. Method 1: Add Swap Using a Swap File
7. Method 2: Add Swap Using a Swap Partition
8. Add Swap Using LVM
9. Make Swap Permanent
10. Verify Swap
11. Swap Priority
12. Swappiness
13. Remove Swap
14. Monitoring Swap Usage
15. Troubleshooting
16. Best Practices
17. Interview Questions
18. Summary

---

# 1. Introduction to Swap Space

**Swap Space** is a reserved area on a disk that Linux uses as **virtual memory** when the system's physical RAM becomes full.

Instead of terminating applications when RAM is exhausted, Linux moves less frequently used memory pages from RAM to swap.

> Swap is slower than RAM because it uses disk storage instead of physical memory.

---

# 2. Why Swap is Needed

Suppose a server has:

```
RAM : 4 GB
Swap: 0 GB
```

Applications running:

```
Apache      : 1 GB
MySQL       : 1.5 GB
Java App    : 2 GB
```

Total memory required:

```
4.5 GB
```

Since RAM is only 4 GB:

```
RAM Full
```

Without swap:

```
Out Of Memory (OOM)

↓

Linux may kill processes.
```

With swap:

```
RAM Full

↓

Inactive pages moved to Swap

↓

Applications continue running
```

---

# 3. How Swap Works

```
Application

      │

      ▼

RAM (Fast)

      │

RAM Full?

      │

     Yes

      ▼

Move Inactive Pages

      │

      ▼

Swap Space (Disk)

      │

      ▼

RAM becomes available
```

---

# 4. Types of Swap

## 1. Swap Partition

A dedicated disk partition.

Example:

```
/dev/sda2
```

Advantages

- Better performance
- Traditional method
- Fixed size

Disadvantages

- Hard to resize

---

## 2. Swap File

A normal file used as swap.

Example

```
/swapfile
```

Advantages

- Easy to create
- Easy to resize
- No partition changes

Disadvantages

- Slightly slower than a partition (usually negligible on modern systems)

---

## 3. LVM Swap

Swap created as a Logical Volume.

Example

```
/dev/vg_system/lv_swap
```

Advantages

- Can be resized using LVM
- Flexible management

---

# 5. Check Existing Swap

Display active swap:

```bash
swapon --show
```

Example

```
NAME      TYPE      SIZE USED PRIO
/swapfile file        2G   0B   -2
```

Check memory:

```bash
free -h
```

Example

```
              total   used   free
Mem:           8Gi    3Gi    4Gi
Swap:          2Gi      0    2Gi
```

Display `/proc/swaps`:

```bash
cat /proc/swaps
```

---

# 6. Method 1: Add Swap Using a Swap File

## Step 1: Check Available Disk Space

```bash
df -h
```

---

## Step 2: Create a Swap File

Create a 2 GB file:

```bash
fallocate -l 2G /swapfile
```

If `fallocate` is unavailable:

```bash
dd if=/dev/zero of=/swapfile bs=1M count=2048
```

Explanation:

- `if=/dev/zero` → Read zeros
- `of=/swapfile` → Output file
- `bs=1M` → Block size
- `count=2048` → 2048 MB (2 GB)

---

## Step 3: Set Correct Permissions

```bash
chmod 600 /swapfile
```

Verify:

```bash
ls -lh /swapfile
```

Output:

```
-rw------- 1 root root 2.0G /swapfile
```

---

## Step 4: Format as Swap

```bash
mkswap /swapfile
```

Output:

```
Setting up swapspace version 1
```

---

## Step 5: Enable Swap

```bash
swapon /swapfile
```

Verify:

```bash
swapon --show
```

---

# 7. Make Swap Permanent

Edit:

```bash
vim /etc/fstab
```

Add:

```
/swapfile none swap defaults 0 0
```

Test:

```bash
mount -a
```

Reboot and verify:

```bash
free -h
```

---

# 8. Method 2: Add Swap Using a Swap Partition

Assume a new disk:

```
/dev/sdb
```

## Create a Partition

```bash
fdisk /dev/sdb
```

Commands:

```
n
p
1

t
82

w
```

> Type `82` is the traditional Linux swap partition type (MBR). On GPT systems, tools such as `parted` or `gdisk` can create a partition with the Linux swap GUID.

Verify:

```bash
lsblk
```

---

## Format Partition

```bash
mkswap /dev/sdb1
```

---

## Enable Swap

```bash
swapon /dev/sdb1
```

Verify:

```bash
swapon --show
```

---

## Permanent Entry

Add to `/etc/fstab`:

```
/dev/sdb1 none swap defaults 0 0
```

---

# 9. Add Swap Using LVM

Assume:

```
VG : vg_system

LV : lv_swap
```

Create a 4 GB Logical Volume:

```bash
lvcreate -L 4G -n lv_swap vg_system
```

Format:

```bash
mkswap /dev/vg_system/lv_swap
```

Enable:

```bash
swapon /dev/vg_system/lv_swap
```

Permanent entry:

```
/dev/vg_system/lv_swap none swap defaults 0 0
```

---

# 10. Verify Swap

Check active swap:

```bash
swapon --show
```

Memory usage:

```bash
free -h
```

Detailed status:

```bash
cat /proc/swaps
```

---

# 11. Swap Priority

When multiple swap devices exist, Linux uses **priority** to determine which one is used first.

Show priorities:

```bash
swapon --show
```

Example:

```
NAME       SIZE PRIO
/swapfile   2G   -2
/dev/sdb1   4G   -1
```

Set a higher priority:

```bash
swapon -p 10 /swapfile
```

Permanent configuration:

```
/swapfile none swap defaults,pri=10 0 0
```

---

# 12. Swappiness

**Swappiness** controls how aggressively Linux uses swap.

Check value:

```bash
cat /proc/sys/vm/swappiness
```

Example:

```
60
```

Typical values:

| Value | Behavior |
|--------|----------|
| 0 | Avoid swap as much as possible |
| 10 | Good for database servers |
| 30 | Moderate use |
| 60 | Default on many distributions |
| 100 | Aggressively use swap |

Temporary change:

```bash
sysctl vm.swappiness=10
```

Permanent:

Edit:

```bash
vim /etc/sysctl.conf
```

Add:

```
vm.swappiness = 10
```

Apply:

```bash
sysctl -p
```

---

# 13. Remove Swap

Disable:

```bash
swapoff /swapfile
```

Delete entry from `/etc/fstab`.

Remove file:

```bash
rm -f /swapfile
```

Verify:

```bash
swapon --show
```

---

# 14. Monitoring Swap Usage

Memory summary:

```bash
free -h
```

Active swap:

```bash
swapon --show
```

Virtual memory statistics:

```bash
vmstat 2
```

Top processes:

```bash
top
```

or

```bash
htop
```

---

# 15. Troubleshooting

## Error

```
swapon: insecure permissions
```

Fix:

```bash
chmod 600 /swapfile
```

---

## Error

```
swapon: Invalid argument
```

Cause:

Swap area not initialized.

Fix:

```bash
mkswap /swapfile
swapon /swapfile
```

---

## Swap Not Enabled After Reboot

Check:

```bash
cat /etc/fstab
```

Verify entry:

```
/swapfile none swap defaults 0 0
```

---

# 16. Best Practices

- Use a swap file on modern Linux systems unless a dedicated partition is required.
- Set file permissions to `600`.
- Always add swap entries to `/etc/fstab`.
- Monitor swap usage with `free -h` and `vmstat`.
- Tune `vm.swappiness` according to workload.
- Use LVM for flexible swap resizing.
- Ensure enough free disk space before creating swap.

---

# 17. Interview Questions

### Q1. What is swap space?

**Answer:**

Swap is disk space used as virtual memory when RAM is insufficient.

---

### Q2. Difference between RAM and Swap?

| RAM | Swap |
|------|------|
| Very fast | Much slower |
| Physical memory | Disk-based virtual memory |
| Used first | Used when RAM is under pressure |

---

### Q3. Which command creates a swap area?

```bash
mkswap
```

---

### Q4. Which command enables swap?

```bash
swapon
```

---

### Q5. Which command disables swap?

```bash
swapoff
```

---

### Q6. Which file stores permanent swap configuration?

```
/etc/fstab
```

---

### Q7. How do you check active swap?

```bash
swapon --show
```

or

```bash
free -h
```

---

# 18. Summary

Swap space extends available memory by using disk storage as virtual memory. Linux supports swap files, swap partitions, and LVM-based swap. The general process is to create the swap area (`fallocate` or `dd` for a file, or create a partition/LV), initialize it with `mkswap`, enable it with `swapon`, and make it persistent by adding an entry to `/etc/fstab`. Administrators should monitor swap usage, configure an appropriate `vm.swappiness` value, and remember that swap is a safety mechanism—not a replacement for sufficient physical RAM.