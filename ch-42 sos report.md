# Linux SOS Report (`sosreport`)

## What is sosreport?

`sosreport` is a Linux troubleshooting and diagnostic tool that collects system configuration, logs, and hardware information into a compressed archive.

It is mainly used by:
- System Administrators
- Support Engineers
- Red Hat Support
- Production Troubleshooting Teams

The generated report helps diagnose Linux server problems without manually collecting information.

---

# Why do we use sosreport?

Suppose a production server has:
- High CPU usage
- Memory leaks
- Network problems
- Service failures
- Kernel crashes
- Performance issues

Instead of asking for multiple log files, the administrator runs:

```bash
sosreport
```

It collects all required diagnostic information into one archive.

---

# Features

✅ Collects system configuration

✅ Collects log files

✅ Collects hardware details

✅ Collects networking information

✅ Collects storage information

✅ Collects running services

✅ Creates compressed archive

---

# Installation

## RHEL/CentOS/Rocky/AlmaLinux

```bash
sudo yum install sos
```

or

```bash
sudo dnf install sos
```

---

## Ubuntu/Debian

```bash
sudo apt update
sudo apt install sosreport
```

Check installation:

```bash
sos report --version
```

or

```bash
sosreport --version
```

---

# Basic Command

Older versions:

```bash
sudo sosreport
```

Newer versions:

```bash
sudo sos report
```

Example:

```bash
sudo sos report
```

Output:

```
sos report

This command will collect diagnostic and configuration information.

Press ENTER to continue.
```

---

# Generated Archive

Example:

```
/var/tmp/sosreport-server01-2026-06.tar.xz
```

or

```
/tmp/sosreport-server01.tar.xz
```

Contains:

```
logs/
network/
storage/
kernel/
services/
packages/
processes/
hardware/
```

---

# Common Options

## Generate Report

```bash
sudo sos report
```

---

## Batch Mode

No user interaction.

```bash
sudo sos report --batch
```

Useful for automation.

---

## Specify Case Number

```bash
sudo sos report --case-id 12345678
```

Used when opening support tickets.

---

## Add Description

```bash
sudo sos report --label "CPU Issue"
```

---

## Save Report in Specific Directory

```bash
sudo sos report --tmp-dir /backup
```

---

## List Available Plugins

```bash
sudo sos report -l
```

Example:

```
networking
kernel
systemd
storage
apache
docker
mysql
podman
kubernetes
```

---

## Enable Specific Plugin

```bash
sudo sos report -e networking
```

---

## Disable Plugin

```bash
sudo sos report -n docker
```

---

# What Information Does sosreport Collect?

## System Information

- Hostname
- Kernel version
- OS version
- Installed packages

Commands similar to:

```bash
hostnamectl
uname -a
cat /etc/os-release
```

---

## Hardware Information

Collects:

- CPU
- RAM
- BIOS
- Motherboard

Uses:

```bash
dmidecode
lscpu
free
```

---

## Network Information

Collects:

```
ip addr
ip route
netstat
ss
resolv.conf
```

---

## Storage Information

Collects:

```
lsblk
fdisk
mount
df
blkid
```

---

## Running Processes

Collects:

```
ps
top
systemctl
```

---

## Logs

Collects:

```
/var/log/messages
/var/log/syslog
/var/log/secure
journalctl
dmesg
```

---

## Services

Collects:

```
systemctl
service status
```

---

# Production Example

Suppose users cannot access a web application.

Administrator checks:

## Step 1

Check hostname

```bash
hostnamectl
```

---

## Step 2

Check service

```bash
systemctl status httpd
```

---

## Step 3

Generate sos report

```bash
sudo sos report
```

---

## Step 4

Archive generated:

```
/var/tmp/sosreport-web01.tar.xz
```

---

## Step 5

Send archive to support team.

Support engineers analyze:

- Logs
- Network
- Services
- Storage
- Hardware

to identify the problem.

---

# Archive Contents

Example:

```
sosreport-server01/

├── hostname
├── uname
├── networking/
├── logs/
├── packages/
├── processes/
├── storage/
├── systemd/
├── hardware/
├── kernel/
├── journal/
└── sos_commands/
```

---

# Extract sos Report

Example:

```bash
tar -xvf sosreport-server01.tar.xz
```

Extract to directory:

```
sosreport-server01/
```

View files:

```bash
cd sosreport-server01
ls
```

---

# Important Files Inside Report

## Hostname

```
hostname
```

---

## Kernel

```
uname
```

---

## Network

```
networking/
```

---

## System Logs

```
logs/
```

---

## Journal Logs

```
journal/
```

---

## Services

```
systemd/
```

---

## Storage

```
storage/
```

---

## Process Information

```
processes/
```

---

# Security Considerations

sosreport may contain:

- IP addresses
- Hostnames
- Usernames
- Mounted filesystems
- Network configuration
- Package information
- Log files

Before sharing externally:

Review contents carefully.

Some versions support:

```bash
sudo sos report --clean
```

This removes sensitive information where possible.

---

# Common Production Commands

Generate report:

```bash
sudo sos report
```

Batch mode:

```bash
sudo sos report --batch
```

List plugins:

```bash
sudo sos report -l
```

Enable plugin:

```bash
sudo sos report -e networking
```

Disable plugin:

```bash
sudo sos report -n docker
```

Specify case ID:

```bash
sudo sos report --case-id 123456
```

Extract report:

```bash
tar -xvf sosreport-server01.tar.xz
```

---

# Real Production Use Cases

## Server Performance Issues

Collect CPU, RAM, and process information.

---

## Network Troubleshooting

Collect routing tables and interface details.

---

## Storage Problems

Collect disk and filesystem information.

---

## Service Failures

Collect systemd service status and logs.

---

## Kernel Panic

Collect kernel messages and crash logs.

---

## Red Hat Support Cases

Attach sosreport archive with support ticket.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| sudo sos report | Generate report |
| sudo sos report --batch | Non-interactive mode |
| sudo sos report -l | List plugins |
| sudo sos report -e networking | Enable plugin |
| sudo sos report -n docker | Disable plugin |
| sudo sos report --case-id 12345 | Add support case ID |
| sudo sos report --tmp-dir /backup | Save to custom location |
| tar -xvf sosreport.tar.xz | Extract archive |

---

# Interview Questions

## Q1. What is sosreport?

A Linux diagnostic tool that collects system information, logs, and hardware details into a compressed archive for troubleshooting.

---

## Q2. Why is sosreport used?

To simplify troubleshooting by collecting all required diagnostic information in one package.

---

## Q3. Where is the sosreport archive stored?

Usually:

```
/var/tmp/
```

or

```
/tmp/
```

---

## Q4. Does sosreport require root privileges?

Yes, because it collects system-wide configuration and logs.

---

## Q5. What type of information does sosreport collect?

- System configuration
- Hardware details
- Network settings
- Storage information
- Running services
- Log files
- Installed packages
- Kernel information

---

## Q6. What is the difference between `sosreport` and `sos report`?

- `sosreport` is the older command.
- `sos report` is the newer syntax used in modern Linux distributions.
- Both generate diagnostic reports for troubleshooting.

---