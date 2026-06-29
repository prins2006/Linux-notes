#

---

# Overview

The Linux boot process is the sequence of steps performed from the moment the computer is powered on until the user reaches the login prompt or graphical desktop.

Modern Linux systems use **systemd** instead of the older **SysVinit**, providing faster boot times, parallel service startup, improved dependency management, and enhanced logging.

---

# Complete Boot Flow

```text
Power ON
      │
      ▼
Power Supply (PSU)
      │
      ▼
CPU Reset
      │
      ▼
BIOS / UEFI Firmware
      │
      ▼
POST (Power-On Self-Test)
      │
      ▼
Boot Device Selection
      │
      ▼
EFI System Partition (ESP)
      │
      ▼
GRUB2 Bootloader
      │
      ▼
Linux Kernel (vmlinuz)
      │
      ▼
Initramfs (Temporary Root Filesystem)
      │
      ▼
Mount Real Root Filesystem (/)
      │
      ▼
systemd (PID 1)
      │
      ▼
Start Targets
      │
      ▼
Start System Services
      │
      ▼
Login Prompt / GUI
```

---

# Stage 1 – Power ON

When the power button is pressed:

1. The PSU converts AC power into DC voltages (3.3V, 5V, and 12V).
2. The motherboard receives stable power.
3. The CPU resets and begins executing firmware instructions.

Example:

```text
Power Button
      │
      ▼
Power Supply
      │
      ▼
Motherboard
      │
      ▼
CPU Starts
```

---

# Stage 2 – BIOS / UEFI Firmware

Modern servers and desktops primarily use **UEFI**.

Responsibilities:

* Initialize CPU and RAM
* Detect storage devices
* Detect USB devices
* Detect graphics adapter
* Read boot order
* Locate the EFI System Partition (ESP)

Example Boot Order:

```text
1. NVMe SSD
2. SATA SSD
3. USB Drive
4. PXE Network
```

---

# Stage 3 – POST

POST verifies hardware before booting.

Hardware Checked:

* CPU
* RAM
* Storage
* Keyboard
* Graphics
* Cooling Fans

Example:

```text
CPU OK

RAM OK

SSD OK

Continue Boot
```

If POST fails:

```text
Memory Error

Beep Codes

Boot Stops
```

---

# Stage 4 – EFI System Partition (ESP)

Unlike older BIOS systems that boot from the MBR, UEFI loads boot files from the **EFI System Partition**.

Typical mount point:

```text
/boot/efi
```

Example structure:

```text
/boot/efi/

└── EFI/

    ├── rocky/

    ├── ubuntu/

    └── BOOT/
```

The firmware loads the GRUB2 EFI executable from this partition.

---

# Stage 5 – GRUB2 Bootloader

GRUB2 loads:

* Linux Kernel
* Initramfs
* Kernel Parameters

Example menu:

```text
Rocky Linux

Advanced Options

Rescue Mode
```

Useful commands:

```bash
grub2-mkconfig -o /boot/grub2/grub.cfg

grubby --default-kernel

grubby --info ALL
```

---

# Stage 6 – Linux Kernel

The kernel is loaded into memory and initializes:

* CPU Scheduler
* Memory Management
* Device Drivers
* Process Management
* Networking
* Filesystems

Kernel file:

```text
/boot/vmlinuz-<version>
```

Example:

```text
/boot/vmlinuz-5.14.0
```

---

# Stage 7 – Initramfs

Initramfs is a temporary root filesystem stored in RAM.

Purpose:

* Load storage drivers
* Detect LVM
* Detect RAID
* Unlock encrypted disks
* Locate the root filesystem

Example:

```text
Kernel

↓

Initramfs

↓

Detect SSD

↓

Mount Root Filesystem

↓

Switch Root
```

---

# Stage 8 – Mount Root Filesystem

The real Linux root filesystem (`/`) is mounted.

Example:

```text
/

├── boot

├── etc

├── home

├── usr

├── var

└── root
```

The temporary Initramfs is then discarded.

---

# Stage 9 – systemd (PID 1)

After mounting the root filesystem, the kernel starts **systemd**, which always runs as **PID 1**.

Verify:

```bash
ps -p 1
```

Example output:

```text
PID TTY TIME CMD

1 ? 00:00 systemd
```

Responsibilities:

* Start services
* Mount filesystems
* Configure networking
* Manage system targets
* Launch login manager

---

# Stage 10 – Start Targets

systemd activates a target that determines the operating mode.

Common targets:

| Target            | Purpose           |
| ----------------- | ----------------- |
| multi-user.target | Text mode (CLI)   |
| graphical.target  | Graphical desktop |
| rescue.target     | Rescue mode       |
| emergency.target  | Emergency shell   |

Commands:

```bash
systemctl get-default

systemctl isolate rescue.target

systemctl isolate graphical.target
```

---

# Stage 11 – Start System Services

systemd starts required services, many of them in parallel for faster boot.

Common services:

```text
NetworkManager

sshd

firewalld

chronyd

crond

rsyslog

httpd

mariadb
```

Useful commands:

```bash
systemctl status

systemctl list-units

systemctl --failed
```

---

# Stage 12 – Login Prompt

When startup is complete:

Console:

```text
Rocky Linux

login:
```

Or a graphical login screen appears.

The Linux system is now ready for users.

---

# Production Example

A production web server running Rocky Linux 9 boots as follows:

```text
Power ON

↓

UEFI

↓

POST

↓

EFI System Partition

↓

GRUB2

↓

Linux Kernel

↓

Initramfs

↓

Root Filesystem

↓

systemd

↓

NetworkManager

↓

firewalld

↓

sshd

↓

MariaDB

↓

Apache (httpd)

↓

Application Available
```

Users can now access:

```text
https://company.example
```

---

# Why Modern Linux Uses systemd

Compared to the older SysVinit system, **systemd** offers:

* Parallel service startup for faster boot times.
* Automatic dependency management.
* Built-in logging with `journalctl`.
* Better process supervision and recovery.
* Improved service control with `systemctl`.
* Consistent management of services, mounts, devices, timers, and sockets.

These improvements make systemd the standard init system for modern enterprise Linux distributions.

---

# Summary

```text
Power ON
      │
      ▼
UEFI
      │
      ▼
POST
      │
      ▼
EFI System Partition
      │
      ▼
GRUB2
      │
      ▼
Linux Kernel
      │
      ▼
Initramfs
      │
      ▼
Root Filesystem
      │
      ▼
systemd (PID 1)
      │
      ▼
Targets
      │
      ▼
System Services
      │
      ▼
Login Prompt
```

This boot sequence is followed by modern Linux distributions such as Rocky Linux 9, RHEL 9, AlmaLinux 9, Ubuntu 24.04, Fedora, and Debian 12.
