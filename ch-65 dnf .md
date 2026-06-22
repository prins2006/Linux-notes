# DNF Command - System Upgrade and Patch Management

# Introduction

DNF (**Dandified YUM**) is the modern package manager used in:

- RHEL 8 / RHEL 9
- Rocky Linux 8 / 9
- AlmaLinux 8 / 9
- Fedora

DNF replaces YUM and provides:

- Faster package management
- Better dependency resolution
- Improved performance
- System updates and patch management

Most Linux Administrators use DNF daily to:

- Install software
- Update packages
- Apply security patches
- Upgrade operating systems
- Manage repositories

---

# What is Patch Management?

Patch Management is the process of:

```text
Identify Updates
       ↓
Test Updates
       ↓
Install Updates
       ↓
Verify System
```

Purpose:

- Fix security vulnerabilities
- Resolve bugs
- Improve performance
- Maintain compliance
- Prevent attacks

---

# DNF Architecture

```text
Repositories
      ↓
     DNF
      ↓
 RPM Database
      ↓
 Installed Packages
```

DNF downloads packages from repositories and installs them using RPM.

---

# Check DNF Version

```bash
dnf --version
```

Example:

```text
4.14.0
```

---

# Display Help

```bash
dnf help
```

or

```bash
man dnf
```

---

# Repository Management

## Show Enabled Repositories

```bash
dnf repolist
```

Example:

```text
repo id          repo name

baseos           BaseOS

appstream        AppStream
```

---

## Show All Repositories

```bash
dnf repolist all
```

---

## Repository Files Location

```bash
/etc/yum.repos.d/
```

Example:

```bash
ls /etc/yum.repos.d/
```

---

# Package Search

Search package:

```bash
dnf search nginx
```

---

Search exact package:

```bash
dnf list nginx
```

---

# Package Information

```bash
dnf info nginx
```

Displays:

- Version
- Repository
- Size
- Description

---

# Install Package

```bash
dnf install nginx
```

Automatic confirmation:

```bash
dnf install nginx -y
```

---

# Install Multiple Packages

```bash
dnf install wget vim git net-tools -y
```

---

# Remove Package

```bash
dnf remove nginx
```

---

# Reinstall Package

```bash
dnf reinstall nginx
```

Useful for repairing corrupted installations.

---

# List Installed Packages

```bash
dnf list installed
```

---

Specific package:

```bash
dnf list installed nginx
```

---

# System Update

## Check Available Updates

```bash
dnf check-update
```

Shows packages that need updating.

---

## Update Entire System

```bash
sudo dnf update
```

or

```bash
sudo dnf upgrade
```

Example:

```bash
sudo dnf update -y
```

---

# Difference Between Update and Upgrade

## Update

```bash
dnf update
```

Updates installed packages.

---

## Upgrade

```bash
dnf upgrade
```

Updates packages and handles obsolete packages more intelligently.

In modern DNF:

```text
update ≈ upgrade
```

---

# Security Updates Only

Show security advisories:

```bash
dnf updateinfo list security
```

---

Apply only security patches:

```bash
dnf update --security
```

Example:

```bash
sudo dnf update --security -y
```

Production environments often use this method.

---

# View Available Security Fixes

```bash
dnf updateinfo summary
```

Example:

```text
Security: 10
Bugfix: 15
Enhancement: 4
```

---

# Package History

Show transaction history:

```bash
dnf history
```

Example:

```text
ID Command Date
```

---

# View Transaction Details

```bash
dnf history info 10
```

---

# Undo a Transaction

```bash
dnf history undo 10
```

Useful when an update causes problems.

---

# Rollback Updates

```bash
dnf history rollback 5
```

Returns system to a previous state.

---

# Download Packages Without Installing

```bash
dnf download nginx
```

---

# Clean Cache

Show cache:

```bash
du -sh /var/cache/dnf
```

Clean cache:

```bash
dnf clean all
```

---

# Rebuild Cache

```bash
dnf makecache
```

Downloads repository metadata.

---

# Package Dependency Information

Show dependencies:

```bash
dnf deplist nginx
```

---

# Find Package Owner

Example:

```bash
rpm -qf /usr/bin/wget
```

Output:

```text
wget-1.21.1
```

---

# Group Package Management

List groups:

```bash
dnf grouplist
```

---

Install group:

```bash
dnf groupinstall "Development Tools"
```

---

Remove group:

```bash
dnf groupremove "Development Tools"
```

---

# Enable EPEL Repository

Install:

```bash
dnf install epel-release -y
```

Verify:

```bash
dnf repolist
```

---

# Check Kernel Version Before Upgrade

```bash
uname -r
```

Example:

```text
5.14.0-427.el9
```

---

# Upgrade Kernel

```bash
dnf update kernel
```

Verify:

```bash
rpm -qa | grep kernel
```

Reboot:

```bash
reboot
```

---

# Production Patch Management Process

## Step 1: Check Current Version

```bash
cat /etc/redhat-release
```

---

## Step 2: Check Available Updates

```bash
dnf check-update
```

---

## Step 3: Review Security Updates

```bash
dnf updateinfo list security
```

---

## Step 4: Take Backup or VM Snapshot

Examples:

```text
VM Snapshot
LVM Snapshot
Cloud Snapshot
```

---

## Step 5: Apply Security Updates

```bash
dnf update --security
```

---

## Step 6: Apply Full Updates

```bash
dnf update -y
```

---

## Step 7: Verify Services

```bash
systemctl --failed
```

---

## Step 8: Reboot if Required

```bash
reboot
```

---

## Step 9: Verify System

```bash
uname -r
```

```bash
dnf history
```

---

# Major Version Upgrade Example

Example:

```text
RHEL 8 → RHEL 9
```

Tool:

```bash
leapp
```

Check:

```bash
leapp preupgrade
```

Upgrade:

```bash
leapp upgrade
```

Reboot:

```bash
reboot
```

Production upgrades should always be tested first.

---

# Troubleshooting DNF

## Repository Error

Clean cache:

```bash
dnf clean all
```

Rebuild metadata:

```bash
dnf makecache
```

---

## Dependency Error

```bash
dnf check
```

Repair:

```bash
dnf distro-sync
```

---

## Verify Installed Package

```bash
rpm -V openssh
```

---

## Check Logs

DNF log:

```bash
cat /var/log/dnf.log
```

---

Transaction history:

```bash
dnf history
```

---

# DNF vs YUM

| Feature | YUM | DNF |
|----------|------|------|
| Dependency Resolution | Good | Better |
| Speed | Slower | Faster |
| Memory Usage | Higher | Lower |
| Transaction History | Basic | Improved |
| Current Standard | Legacy | Modern |
| RHEL 8/9 Default | No | Yes |

---

# Common Administrator Commands

Check updates:

```bash
dnf check-update
```

Update system:

```bash
dnf update -y
```

Install package:

```bash
dnf install package
```

Remove package:

```bash
dnf remove package
```

View history:

```bash
dnf history
```

Security updates:

```bash
dnf update --security
```

Clean cache:

```bash
dnf clean all
```

---

# Interview Questions

### What does DNF stand for?

```text
Dandified YUM
```

---

### Which package manager is used in RHEL 9?

```text
DNF
```

---

### How do you check available updates?

```bash
dnf check-update
```

---

### How do you apply only security updates?

```bash
dnf update --security
```

---

### Which command shows transaction history?

```bash
dnf history
```

---

### How do you rollback a transaction?

```bash
dnf history rollback <ID>
```

---

### Where are repository files stored?

```bash
/etc/yum.repos.d/
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `dnf repolist` | List repositories |
| `dnf search pkg` | Search package |
| `dnf info pkg` | Package details |
| `dnf install pkg` | Install package |
| `dnf remove pkg` | Remove package |
| `dnf check-update` | Check updates |
| `dnf update` | Update system |
| `dnf update --security` | Security patches only |
| `dnf history` | Transaction history |
| `dnf history undo ID` | Undo transaction |
| `dnf clean all` | Clean cache |
| `dnf makecache` | Refresh repository cache |

# Exam Tip

For production Linux servers, the most important update commands are:

```bash
dnf check-update
```

```bash
dnf update --security
```

```bash
dnf update -y
```

```bash
dnf history
```

These commands are used for patch management, compliance, vulnerability remediation, and system maintenance in enterprise Linux environments.