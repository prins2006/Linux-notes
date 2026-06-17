# Linux Kernel

## What is a Linux Kernel?

The **Linux Kernel** is the core component of the Linux operating system. It acts as a bridge between the hardware and user applications.

The kernel manages:
- CPU
- Memory
- Devices
- File systems
- Processes
- Networking

Without the kernel, applications cannot communicate with the hardware.

---

# Linux Architecture

```
+----------------------+
|    User Applications |
+----------------------+
|      Shell           |
+----------------------+
|    Linux Kernel      |
+----------------------+
|       Hardware       |
+----------------------+
```

### Explanation

1. User runs a command.
2. Shell interprets the command.
3. Kernel processes the request.
4. Hardware performs the operation.
5. Kernel returns the result to the user.

---

# What Does the Kernel Do?

The kernel is responsible for:

- Process Management
- Memory Management
- Device Management
- File System Management
- Networking
- Security
- System Calls

---

# 1. Process Management

The kernel:
- Creates processes.
- Schedules CPU time.
- Terminates processes.
- Manages multitasking.

Example:

```bash
ps -ef
```

View running processes.

---

# 2. Memory Management

The kernel manages:
- RAM allocation
- Virtual memory
- Swap memory
- Memory protection

Check memory:

```bash
free -h
```

Example:

```
Mem: 8Gi
Swap: 2Gi
```

---

# 3. Device Management

The kernel controls hardware devices.

Examples:
- Keyboard
- Mouse
- USB
- Hard disks
- Network cards

List devices:

```bash
lsblk
```

Check USB devices:

```bash
lsusb
```

---

# 4. File System Management

The kernel manages:
- File creation
- Reading
- Writing
- Permissions
- Mounting

Check mounted filesystems:

```bash
mount
```

Disk usage:

```bash
df -h
```

---

# 5. Networking

The kernel handles:
- IP communication
- Routing
- TCP/UDP
- Firewalls

Check interfaces:

```bash
ip addr
```

Routing table:

```bash
ip route
```

---

# 6. Security

Kernel provides:
- User permissions
- Process isolation
- Access control
- Security modules

Check SELinux:

```bash
getenforce
```

---

# 7. System Calls

Applications communicate with the kernel using system calls.

Examples:
- open()
- read()
- write()
- fork()
- exec()
- close()

---

# Types of Linux Kernel

## 1. Monolithic Kernel

Linux uses a monolithic kernel.

Characteristics:
- All core services run in kernel space.
- High performance.
- Supports loadable modules.

Examples:
- Linux
- UNIX

---

## 2. Microkernel

Only essential functions run in kernel space.

Examples:
- Minix
- QNX

---

## 3. Hybrid Kernel

Combination of monolithic and microkernel.

Examples:
- Windows NT
- macOS (XNU)

---

# Kernel Space vs User Space

| User Space | Kernel Space |
|------------|-------------|
| Applications | Kernel |
| Limited privileges | Full privileges |
| Cannot access hardware directly | Direct hardware access |
| Example: Firefox | Example: Device driver |

---

# Kernel Modules

Kernel modules extend functionality without rebooting.

Examples:
- USB driver
- Network driver
- Filesystem driver

---

# List Loaded Modules

```bash
lsmod
```

Example:

```
Module          Size
usb_storage
ext4
vfat
```

---

# Load Module

```bash
sudo modprobe module_name
```

Example:

```bash
sudo modprobe loop
```

---

# Remove Module

```bash
sudo modprobe -r module_name
```

Example:

```bash
sudo modprobe -r loop
```

---

# Kernel Version

Check kernel version:

```bash
uname -r
```

Example:

```
6.8.0-31-generic
```

---

# Display Complete Kernel Information

```bash
uname -a
```

Example:

```
Linux server01 6.8.0-31-generic x86_64 GNU/Linux
```

---

# Kernel Messages

View kernel messages:

```bash
dmesg
```

Latest messages:

```bash
dmesg | tail
```

---

# Boot Process and Kernel

```
BIOS/UEFI
    |
    v
Bootloader (GRUB)
    |
    v
Linux Kernel
    |
    v
systemd
    |
    v
Services
    |
    v
Login Prompt
```

---

# Kernel Image

Kernel file:

```
/boot/vmlinuz
```

Example:

```
/boot/vmlinuz-6.8.0-31-generic
```

---

# Initial RAM Disk

Temporary filesystem loaded during boot.

Files:

```
/boot/initrd.img
```

or

```
/boot/initramfs
```

---

# Check Installed Kernels

Ubuntu:

```bash
dpkg --list | grep linux-image
```

RHEL:

```bash
rpm -qa | grep kernel
```

---

# Kernel Upgrade

Ubuntu:

```bash
sudo apt update
sudo apt upgrade
```

---

RHEL:

```bash
sudo yum update kernel
```

or

```bash
sudo dnf update kernel
```

Reboot:

```bash
sudo reboot
```

---

# Production Use Cases

## Check Running Kernel

```bash
uname -r
```

Verify correct kernel after updates.

---

## Troubleshoot Hardware

```bash
dmesg
```

Check driver issues.

---

## Check Loaded Drivers

```bash
lsmod
```

Verify required modules.

---

## Load Network Driver

```bash
sudo modprobe e1000
```

---

## Verify Disk Detection

```bash
dmesg | grep sd
```

---

# Common Kernel Commands

| Command | Purpose |
|----------|----------|
| uname -r | Kernel version |
| uname -a | Complete kernel info |
| dmesg | Kernel messages |
| dmesg | tail | Latest kernel logs |
| lsmod | Loaded modules |
| modprobe module | Load module |
| modprobe -r module | Remove module |
| cat /proc/version | Kernel information |
| hostnamectl | System and kernel info |

---

# Linux Kernel vs Shell

| Kernel | Shell |
|----------|----------|
| Core of OS | User interface |
| Manages hardware | Accepts commands |
| Runs in kernel space | Runs in user space |
| Handles system calls | Interacts with kernel |
| Example: Linux Kernel | Example: Bash |

---

# Real Production Example

Suppose a new network card is installed.

### Step 1

Check kernel version:

```bash
uname -r
```

---

### Step 2

Check hardware detection:

```bash
dmesg
```

---

### Step 3

Verify driver:

```bash
lsmod
```

---

### Step 4

Load missing module:

```bash
sudo modprobe driver_name
```

---

### Step 5

Verify network interface:

```bash
ip addr
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| uname -r | Show kernel version |
| uname -a | Show complete kernel info |
| dmesg | Show kernel messages |
| dmesg | tail | Latest kernel logs |
| lsmod | List kernel modules |
| modprobe module | Load module |
| modprobe -r module | Remove module |
| free -h | Memory information |
| ip addr | Network interfaces |
| cat /proc/version | Kernel details |

---

# Interview Questions

## Q1. What is the Linux Kernel?

The Linux Kernel is the core component of the operating system that manages hardware resources and provides services to applications.

---

## Q2. What are the main functions of the kernel?

- Process management
- Memory management
- Device management
- File system management
- Networking
- Security
- System calls

---

## Q3. What type of kernel does Linux use?

Linux uses a **Monolithic Kernel** with support for dynamically loadable modules.

---

## Q4. How do you check the running kernel version?

```bash
uname -r
```

---

## Q5. What is a kernel module?

A kernel module is a piece of code that can be loaded or removed from the running kernel to add functionality, such as hardware drivers.

---

## Q6. What is the difference between kernel space and user space?

- Kernel space has full access to hardware and system resources.
- User space runs applications with limited privileges and interacts with the kernel through system calls.

---

## Q7. What command is used to view kernel boot and hardware messages?

```bash
dmesg
```

---