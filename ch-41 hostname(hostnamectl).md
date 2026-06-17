# Linux System Administration
# Changing System Hostname (`hostnamectl`) and Finding System Information (`uname`, `dmidecode`)

---

# 1. What is a Hostname?

A hostname is a unique name assigned to a Linux system that identifies it on a network.

Examples:
```
web-server
db-server01
app-server
ubuntu-pc
```

Check the current hostname:

```bash
hostname
```

Output:

```
server01
```

---

# 2. hostnamectl Command

`hostnamectl` is part of systemd and is used to view and change the system hostname.

## Syntax

```bash
hostnamectl [OPTION]
```

---

## Display System Hostname Information

```bash
hostnamectl
```

Example:

```
Static hostname: server01
Pretty hostname:
Icon name: computer-vm
Chassis: vm
Machine ID: xxxxxxxxx
Boot ID: xxxxxxxxx
Operating System: Ubuntu 24.04 LTS
Kernel: Linux 6.8.0
Architecture: x86-64
```

### Explanation

| Field | Meaning |
|-------|----------|
| Static hostname | Permanent hostname |
| Pretty hostname | Friendly display name |
| Operating System | Installed Linux version |
| Kernel | Running kernel version |
| Architecture | CPU architecture |

---

## Display Static Hostname

```bash
hostnamectl --static
```

Output:

```
server01
```

---

## Change Permanent Hostname

Syntax:

```bash
sudo hostnamectl set-hostname newhostname
```

Example:

```bash
sudo hostnamectl set-hostname webserver01
```

Verify:

```bash
hostnamectl
```

Output:

```
Static hostname: webserver01
```

No reboot required.

---

## Set Pretty Hostname

Example:

```bash
sudo hostnamectl set-hostname "Production Web Server" --pretty
```

Verify:

```bash
hostnamectl
```

Output:

```
Pretty hostname: Production Web Server
Static hostname: webserver01
```

---

## Temporary Hostname

Temporary hostname lasts until reboot.

```bash
sudo hostname tempserver
```

Permanent:

```bash
sudo hostnamectl set-hostname tempserver
```

---

# Important Configuration Files

## /etc/hostname

Stores permanent hostname.

View:

```bash
cat /etc/hostname
```

Example:

```
webserver01
```

---

## /etc/hosts

Maps hostname to IP addresses.

View:

```bash
cat /etc/hosts
```

Example:

```
127.0.0.1 localhost
127.0.1.1 webserver01
```

After changing hostname, update this file if necessary.

---

# Production Example

Current hostname:

```
ubuntu
```

Change hostname:

```bash
sudo hostnamectl set-hostname app-server01
```

Update hosts file:

```bash
sudo nano /etc/hosts
```

Add:

```
127.0.1.1 app-server01
```

Verify:

```bash
hostname
hostnamectl
```

---

# Common hostnamectl Commands

| Command | Purpose |
|----------|----------|
| hostname | Display hostname |
| hostnamectl | Display hostname details |
| hostnamectl --static | Show static hostname |
| sudo hostnamectl set-hostname server01 | Change hostname |
| cat /etc/hostname | View hostname file |
| cat /etc/hosts | View hosts file |

---

# 3. uname Command

## What is uname?

`uname` stands for Unix Name.

It displays kernel and operating system information.

Syntax:

```bash
uname [OPTION]
```

---

## uname

```bash
uname
```

Output:

```
Linux
```

Shows operating system name.

---

## uname -a

Displays all available information.

```bash
uname -a
```

Example:

```
Linux server01 6.8.0-31-generic x86_64 GNU/Linux
```

### Breakdown

| Part | Meaning |
|-------|----------|
| Linux | Kernel name |
| server01 | Hostname |
| 6.8.0 | Kernel version |
| x86_64 | Architecture |
| GNU/Linux | Operating system |

---

## uname -r

Kernel release:

```bash
uname -r
```

Example:

```
6.8.0-31-generic
```

---

## uname -m

Machine architecture:

```bash
uname -m
```

Output:

```
x86_64
```

Possible values:

| Output | Meaning |
|----------|----------|
| x86_64 | 64-bit |
| i686 | 32-bit |
| aarch64 | ARM64 |

---

## uname -n

Displays hostname.

```bash
uname -n
```

---

## uname -s

Displays kernel name.

```bash
uname -s
```

Output:

```
Linux
```

---

## uname -p

Displays processor type.

```bash
uname -p
```

---

## uname -i

Displays hardware platform.

```bash
uname -i
```

---

# Common uname Commands

| Command | Purpose |
|----------|----------|
| uname | OS name |
| uname -a | Complete system info |
| uname -r | Kernel version |
| uname -m | CPU architecture |
| uname -n | Hostname |
| uname -s | Kernel name |
| uname -p | Processor |
| uname -i | Hardware platform |

---

# 4. dmidecode Command

## What is dmidecode?

`dmidecode` reads hardware information from BIOS/UEFI using DMI (Desktop Management Interface).

Requires root privileges.

Install:

```bash
sudo apt update
sudo apt install dmidecode
```

---

## Display Complete Hardware Information

```bash
sudo dmidecode
```

Shows:

- BIOS
- Motherboard
- Processor
- RAM
- Serial Number
- UUID
- Manufacturer

---

## System Information

```bash
sudo dmidecode -t system
```

Example:

```
Manufacturer: Dell Inc.
Product Name: PowerEdge R750
Serial Number: XXXXX
UUID: XXXXX
```

---

## BIOS Information

```bash
sudo dmidecode -t bios
```

Example:

```
Vendor: Dell
Version: 2.5.4
Release Date: 2025-01-15
```

---

## Processor Information

```bash
sudo dmidecode -t processor
```

Shows:

- CPU Model
- Socket
- Core Count
- Thread Count
- Maximum Speed

---

## Memory Information

```bash
sudo dmidecode -t memory
```

Example:

```
Size: 16384 MB
Type: DDR5
Speed: 5600 MT/s
Manufacturer: Samsung
```

---

## Motherboard Information

```bash
sudo dmidecode -t baseboard
```

Displays motherboard details.

---

## Chassis Information

```bash
sudo dmidecode -t chassis
```

Displays:

- Desktop
- Laptop
- Rack Server
- Virtual Machine

---

## Display Specific Hardware Information

### System Manufacturer

```bash
sudo dmidecode -s system-manufacturer
```

---

### Product Name

```bash
sudo dmidecode -s system-product-name
```

---

### BIOS Version

```bash
sudo dmidecode -s bios-version
```

---

### System Serial Number

```bash
sudo dmidecode -s system-serial-number
```

---

# Production Troubleshooting

Suppose a production server issue occurs.

## Step 1

Check hostname:

```bash
hostnamectl
```

---

## Step 2

Check kernel version:

```bash
uname -r
```

---

## Step 3

Check CPU architecture:

```bash
uname -m
```

---

## Step 4

Check hardware model:

```bash
sudo dmidecode -t system
```

---

## Step 5

Check BIOS version:

```bash
sudo dmidecode -t bios
```

---

## Step 6

Check installed RAM:

```bash
sudo dmidecode -t memory
```

---

# Real Production Use Cases

## Asset Inventory

```bash
sudo dmidecode -s system-serial-number
```

Find server serial number.

---

## Kernel Verification

```bash
uname -r
```

Verify required kernel after update.

---

## Hardware Upgrade Planning

```bash
sudo dmidecode -t memory
```

Check available RAM slots and capacity.

---

## Server Identification

```bash
hostnamectl
```

Verify correct production server.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| hostname | Display hostname |
| hostnamectl | Display hostname details |
| hostnamectl --static | Show static hostname |
| sudo hostnamectl set-hostname server01 | Change hostname |
| cat /etc/hostname | View hostname |
| cat /etc/hosts | View hosts mapping |
| uname -a | Complete system info |
| uname -r | Kernel version |
| uname -m | CPU architecture |
| uname -n | Hostname |
| sudo dmidecode | Complete hardware info |
| sudo dmidecode -t system | System details |
| sudo dmidecode -t bios | BIOS details |
| sudo dmidecode -t processor | CPU details |
| sudo dmidecode -t memory | RAM details |
| sudo dmidecode -s system-serial-number | Serial number |

---

# Interview Questions

## Q1. What is the difference between hostname and hostnamectl?

**hostname**
- Displays or temporarily changes hostname.

**hostnamectl**
- Displays detailed hostname information.
- Permanently changes hostname.
- Works with systemd.

---

## Q2. Does changing hostname with hostnamectl require reboot?

No. The change takes effect immediately.

---

## Q3. What does uname -a display?

It displays kernel name, hostname, kernel version, architecture, and operating system information.

---

## Q4. Why is dmidecode useful?

It provides hardware details such as BIOS version, motherboard, CPU, RAM, manufacturer, serial numbers, and UUID for troubleshooting and inventory management.

---

## Q5. Why update /etc/hosts after changing hostname?

To ensure the system can resolve its own hostname correctly and avoid local networking issues.

---