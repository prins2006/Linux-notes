
# Table of Contents

1. Introduction
2. What Happens When You Press the Power Button?
3. Complete Boot Flow
4. Stage 1 – Power Supply (PSU)
5. Stage 2 – CPU Reset
6. Stage 3 – BIOS / UEFI Firmware
7. Stage 4 – POST (Power-On Self-Test)
8. Stage 5 – Boot Device Selection
9. Stage 6 – Bootloader (GRUB2)
10. Stage 7 – Linux Kernel Loading
11. Stage 8 – Initramfs (initrd)
12. Stage 9 – systemd Initialization
13. Stage 10 – Login Prompt
14. Complete Production Example
15. Common Boot Problems
16. Boot Troubleshooting Commands
17. Production Best Practices
18. Interview Questions

---

# 1. Introduction

The **Computer Boot Process** is the sequence of steps a computer follows from the moment you press the **Power Button** until the operating system is fully loaded and ready for use.

Without the boot process:

* CPU cannot execute instructions.
* RAM cannot be initialized.
* Storage cannot be accessed.
* Operating System cannot start.

Think of booting as waking up a human.

```
Wake Up
↓

Open Eyes
↓

Check Surroundings
↓

Wear Clothes

↓

Go to Office
```

Computer Boot:

```
Power ON
↓

BIOS/UEFI Starts
↓

Hardware Check
↓

Bootloader Loads
↓

Kernel Loads
↓

systemd Starts Services
↓

Login Screen
```

---

# 2. Complete Boot Flow

```
+----------------------+
| Power Button Pressed |
+----------+-----------+
           |
           v
+----------------------+
| Power Supply (PSU)   |
+----------+-----------+
           |
           v
+----------------------+
| CPU Reset            |
+----------+-----------+
           |
           v
+----------------------+
| BIOS / UEFI Firmware |
+----------+-----------+
           |
           v
+----------------------+
| POST Hardware Check  |
+----------+-----------+
           |
           v
+----------------------+
| Boot Device Search   |
+----------+-----------+
           |
           v
+----------------------+
| GRUB Bootloader      |
+----------+-----------+
           |
           v
+----------------------+
| Linux Kernel         |
+----------+-----------+
           |
           v
+----------------------+
| Initramfs            |
+----------+-----------+
           |
           v
+----------------------+
| systemd (PID 1)      |
+----------+-----------+
           |
           v
+----------------------+
| Login Prompt         |
+----------------------+
```

---

# 3. Stage 1 – Power Supply (PSU)

When the power button is pressed:

```
Electricity
↓

Power Supply (PSU)

↓

3.3V
5V
12V

↓

Motherboard
CPU
RAM
SSD
Fans
```

The PSU converts AC electricity into DC voltages required by the computer.

### Example

If the PSU is faulty:

```
Power Button

↓

Nothing Happens
```

No boot process starts.

---

# 4. Stage 2 – CPU Reset

When power becomes stable:

The motherboard resets the CPU.

The CPU always starts execution from a predefined firmware address stored in ROM.

```
CPU

↓

Reset

↓

Firmware Address

↓

BIOS/UEFI
```

---

# 5. Stage 3 – BIOS / UEFI

BIOS = Basic Input Output System

UEFI = Unified Extensible Firmware Interface

Firmware stored on the motherboard.

Its responsibilities:

* Detect CPU
* Detect RAM
* Detect Keyboard
* Detect Storage
* Detect GPU
* Detect USB Devices

---

## BIOS Boot

```
Power

↓

BIOS

↓

MBR

↓

GRUB

↓

Kernel
```

---

## UEFI Boot

```
Power

↓

UEFI

↓

EFI Partition

↓

GRUB EFI

↓

Kernel
```

---

# BIOS vs UEFI

| BIOS       | UEFI                 |
| ---------- | -------------------- |
| Older      | Modern               |
| MBR        | GPT                  |
| 2 TB Limit | Supports Large Disks |
| Text Mode  | GUI Support          |
| Slow       | Faster               |
| Legacy     | Modern Standard      |

---

# 6. Stage 4 – POST (Power-On Self-Test)

POST checks whether the hardware is working.

Checks include:

* CPU
* RAM
* Keyboard
* Graphics Card
* Disk
* Fan
* CMOS Battery

---

## Successful POST

```
Memory OK

CPU OK

SSD OK

Keyboard OK

↓

Continue Boot
```

---

## Failed POST

Example:

```
No RAM Installed
```

Result:

```
Beep Beep Beep

No Display
```

The system never reaches GRUB.

---

# 7. Stage 5 – Boot Device Selection

BIOS reads the configured boot order.

Example:

```
1. NVMe SSD

2. SATA SSD

3. USB

4. DVD

5. Network PXE
```

The first bootable device is selected.

---

# Production Example

Server Boot Order

```
NVMe SSD

↓

Rocky Linux

↓

GRUB
```

If SSD fails:

```
Network PXE Boot

↓

Install OS Automatically
```

---

# 8. Stage 6 – Bootloader (GRUB2)

GRUB = Grand Unified Bootloader

Responsibilities:

* Load Linux Kernel
* Load Initramfs
* Show Boot Menu
* Recovery Mode
* Boot Multiple OS

---

Example

```
GRUB Menu

Rocky Linux

Ubuntu

Windows

Rescue Mode
```

---

GRUB Configuration

```
/boot/grub2/grub.cfg
```

UEFI:

```
/boot/efi/EFI/
```

---

Useful Commands

```
grub2-mkconfig -o /boot/grub2/grub.cfg

grubby --default-kernel

grubby --info ALL
```

---

# 9. Stage 7 – Linux Kernel

Kernel is loaded into RAM.

Kernel Responsibilities

* Memory Management
* CPU Scheduling
* Process Management
* Drivers
* Filesystem
* Networking

---

Kernel File

```
/boot/vmlinuz-<version>
```

Example

```
/boot/vmlinuz-5.14.0
```

---

Boot Parameters Example

```
ro quiet splash
```

Meaning

```
ro

Read-only root filesystem

quiet

Hide boot messages

splash

Show logo
```

---

# 10. Stage 8 – Initramfs

Initramfs is a temporary root filesystem loaded into RAM.

Purpose

* Load Drivers
* Detect Storage
* Detect LVM
* Detect RAID
* Detect Filesystem
* Mount Real Root Filesystem

---

Flow

```
Kernel

↓

Initramfs

↓

SSD Found

↓

Mount /

↓

Switch Root
```

---

File

```
/boot/initramfs-5.14.img
```

---

# 11. Stage 9 – systemd

After mounting the real root filesystem:

Kernel starts:

```
systemd
```

systemd always runs as:

```
PID 1
```

Verify:

```
ps -p 1
```

Output

```
PID TTY TIME CMD

1 ? 00:00 systemd
```

---

systemd Starts

* Network
* SSH
* Firewall
* Logging
* DNS
* Web Server
* Database

---

Useful Commands

```
systemctl status

systemctl list-units

systemctl list-dependencies

systemctl isolate rescue.target
```

---

# 12. Stage 10 – Login Prompt

After all services start:

```
Rocky Linux 9

login:
```

Or GUI:

```
Username

Password
```

The computer is now ready.

---

# 13. Production Boot Example

Suppose an organization has:

```
Dell PowerEdge Server

↓

UEFI

↓

NVMe SSD

↓

GRUB2

↓

Rocky Linux

↓

Apache

↓

MariaDB

↓

Application
```

Boot Sequence

```
Power ON

↓

UEFI

↓

POST

↓

GRUB

↓

Kernel

↓

Initramfs

↓

systemd

↓

Network

↓

SSH

↓

Firewall

↓

MariaDB

↓

Apache

↓

Application Available
```

Users can now access:

```
https://company.com
```

---

# 14. Common Boot Problems

## Problem 1

```
No Boot Device Found
```

Reason

* SSD Missing
* Wrong Boot Order

---

## Problem 2

```
GRUB Rescue>
```

Reason

* Deleted Bootloader
* Damaged Partition

---

## Problem 3

```
Kernel Panic
```

Reason

* Corrupt Kernel
* Missing Drivers
* Damaged Initramfs

---

## Problem 4

```
Emergency Mode
```

Reason

```
Wrong /etc/fstab
```

---

# 15. Boot Troubleshooting Commands

Check boot logs:

```bash
journalctl -b
```

Previous boot:

```bash
journalctl -b -1
```

Kernel messages:

```bash
dmesg
```

Current target:

```bash
systemctl get-default
```

Failed services:

```bash
systemctl --failed
```

View boot time:

```bash
systemd-analyze
```

Critical chain:

```bash
systemd-analyze critical-chain
```

---

# 16. Best Practices

* Use UEFI with GPT for modern systems.
* Keep multiple kernel versions installed.
* Back up the GRUB configuration before changes.
* Test bootloader updates in staging before production.
* Monitor boot time using `systemd-analyze`.
* Review boot logs after kernel upgrades.
* Verify `/etc/fstab` before rebooting.
* Enable Secure Boot where organizational policies require it.

---

# 17. Summary

```
Power Button
↓

PSU
↓

CPU Reset
↓

BIOS / UEFI
↓

POST
↓

Boot Device
↓

GRUB2
↓

Linux Kernel
↓

Initramfs
↓

systemd
↓

Services
↓

Login
```

Every successful Linux boot follows this sequence. Understanding each stage helps administrators diagnose issues quickly, maintain reliable production systems, and recover from boot failures efficiently.

---

# 18. Interview Questions

### 1. What is the computer boot process?

The sequence of steps that starts a computer and loads the operating system into memory.

### 2. What is POST?

Power-On Self-Test performed by BIOS/UEFI to verify hardware before booting.

### 3. What is the role of GRUB?

GRUB is the bootloader that loads the Linux kernel and initramfs and can present a menu for multiple operating systems.

### 4. What is initramfs?

A temporary root filesystem in RAM used to load drivers and mount the real root filesystem before handing control to the operating system.

### 5. Why is `systemd` important?

`systemd` runs as PID 1, initializes the userspace environment, and starts system services required for a fully operational Linux system.
