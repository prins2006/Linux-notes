# System Updates and Repositories (rpm, yum)

# Introduction

Linux systems need regular updates for:

- Security patches
- Bug fixes
- Performance improvements
- New features
- Package installation

In Red Hat-based distributions such as:

- RHEL
- Rocky Linux
- AlmaLinux
- CentOS

the most common package management tools are:

```text
RPM
YUM
DNF
```

---

# Package Management Architecture

```text
Application
     |
     v
YUM / DNF
     |
     v
RPM Database
     |
     v
Package Files (.rpm)
```

---

# What is RPM?

## Meaning

```text
RPM = Red Hat Package Manager
```

RPM is a low-level package management tool used to:

- Install packages
- Remove packages
- Verify packages
- Query packages

Package extension:

```text
.rpm
```

Example:

```text
httpd-2.4.57.rpm
```

---

# What is YUM?

## Meaning

```text
YUM = Yellowdog Updater Modified
```

YUM is a high-level package manager.

It works with RPM packages and automatically handles:

- Dependencies
- Repository access
- Updates
- Package searches

---

# RPM vs YUM

| Feature | RPM | YUM |
|----------|------|------|
| Installs RPM Packages | Yes | Yes |
| Dependency Resolution | No | Yes |
| Repository Access | No | Yes |
| Package Search | Limited | Yes |
| Updates | Manual | Automatic |
| Recommended | Limited | Yes |

---

# RPM Package Structure

Example:

```text
httpd-2.4.57-1.el9.x86_64.rpm
```

Breakdown:

```text
httpd      -> Package Name
2.4.57     -> Version
1.el9      -> Release
x86_64     -> Architecture
rpm        -> Package Type
```

---

# RPM Database

Location:

```bash
/var/lib/rpm/
```

Stores installed package information.

---

# Query Installed Packages

List all packages:

```bash
rpm -qa
```

Example:

```bash
rpm -qa | less
```

---

# Search Package

```bash
rpm -qa | grep httpd
```

Example:

```text
httpd-2.4.57
```

---

# Package Information

```bash
rpm -qi httpd
```

Output:

```text
Name
Version
Vendor
Description
```

---

# List Package Files

```bash
rpm -ql httpd
```

Example:

```text
/etc/httpd/
/usr/sbin/httpd
```

---

# Find Package Owning a File

```bash
rpm -qf /usr/bin/wget
```

Output:

```text
wget-1.21.1
```

---

# Install RPM Package

```bash
rpm -ivh package.rpm
```

Options:

```text
-i Install
-v Verbose
-h Progress Bar
```

Example:

```bash
rpm -ivh nginx.rpm
```

---

# Upgrade RPM Package

```bash
rpm -Uvh package.rpm
```

Meaning:

```text
U = Upgrade
```

---

# Remove Package

```bash
rpm -e package_name
```

Example:

```bash
rpm -e httpd
```

---

# Verify Package

```bash
rpm -V httpd
```

Checks:

- Files changed
- Missing files
- Permission changes

---

# Check Package Dependencies

```bash
rpm -qpR package.rpm
```

Example:

```bash
rpm -qpR nginx.rpm
```

---

# Common RPM Problem

Example:

```bash
rpm -ivh nginx.rpm
```

Output:

```text
dependency failed
```

Reason:

```text
Required packages missing
```

Solution:

Use YUM instead.

---

# YUM Repositories

## What is a Repository?

A repository is a server containing RPM packages.

Example:

```text
repo.company.com
```

or

```text
mirror.rockylinux.org
```

---

# Repository Configuration Location

```bash
/etc/yum.repos.d/
```

Example:

```bash
ls /etc/yum.repos.d/
```

Output:

```text
baseos.repo
appstream.repo
epel.repo
```

---

# View Repository Configuration

```bash
cat /etc/yum.repos.d/baseos.repo
```

Example:

```text
[baseos]

name=BaseOS

baseurl=http://mirror.example.com

enabled=1

gpgcheck=1
```

---

# List Enabled Repositories

```bash
yum repolist
```

or

```bash
dnf repolist
```

---

# Show All Repositories

```bash
yum repolist all
```

---

# Search Packages

```bash
yum search nginx
```

Example:

```text
nginx.x86_64
```

---

# Package Information

```bash
yum info nginx
```

---

# Install Package

```bash
yum install nginx -y
```

Option:

```text
-y = Automatically answer yes
```

---

# Remove Package

```bash
yum remove nginx
```

---

# Update Package

```bash
yum update nginx
```

---

# Update Entire System

```bash
yum update
```

or

```bash
dnf update
```

Updates all installed packages.

---

# Check Available Updates

```bash
yum check-update
```

---

# Reinstall Package

```bash
yum reinstall nginx
```

---

# Install Multiple Packages

```bash
yum install wget vim git -y
```

---

# Display Package Dependencies

```bash
yum deplist nginx
```

---

# Clean Cache

View cache:

```bash
du -sh /var/cache/yum
```

Clean:

```bash
yum clean all
```

---

# Download Package Only

```bash
yum install --downloadonly nginx
```

---

# Group Packages

List groups:

```bash
yum grouplist
```

Install group:

```bash
yum groupinstall "Development Tools"
```

---

# Enable EPEL Repository

Install:

```bash
dnf install epel-release -y
```

Verify:

```bash
yum repolist
```

EPEL provides many additional packages.

---

# Important Repository Files

Repository configuration:

```bash
/etc/yum.repos.d/*.repo
```

YUM configuration:

```bash
/etc/yum.conf
```

Cache:

```bash
/var/cache/yum/
```

RPM database:

```bash
/var/lib/rpm/
```

---

# Production Update Process

## Step 1

Check updates:

```bash
yum check-update
```

---

## Step 2

Review packages:

```bash
yum update --assumeno
```

---

## Step 3

Take backup or snapshot.

---

## Step 4

Apply updates:

```bash
yum update -y
```

---

## Step 5

Reboot if kernel updated:

```bash
reboot
```

---

# Troubleshooting

## Repository Problems

Clean metadata:

```bash
yum clean all
```

Generate cache:

```bash
yum makecache
```

---

## Package Database Issues

Rebuild database:

```bash
rpm --rebuilddb
```

---

## Dependency Issues

Check:

```bash
yum deplist package
```

---

## Verify Package

```bash
rpm -V package
```

---

# RPM vs YUM vs DNF

| Feature | RPM | YUM | DNF |
|----------|------|------|------|
| Install Packages | Yes | Yes | Yes |
| Dependency Handling | No | Yes | Yes |
| Faster | No | Moderate | Yes |
| Repository Support | No | Yes | Yes |
| Modern Tool | No | Older | Yes |
| RHEL 8/9 Default | No | Alias | Yes |

---

# Real Production Examples

Install Apache:

```bash
yum install httpd -y
```

---

Update System:

```bash
yum update -y
```

---

Install Monitoring Tools:

```bash
yum install sysstat net-tools wget -y
```

---

Search Package:

```bash
yum search docker
```

---

Verify Package:

```bash
rpm -V openssh
```

---

# Interview Questions

### What is RPM?

Red Hat Package Manager used to manage `.rpm` packages.

---

### What is YUM?

Yellowdog Updater Modified, a package manager that handles dependencies automatically.

---

### Which command lists installed packages?

```bash
rpm -qa
```

---

### Which command installs a package using YUM?

```bash
yum install package
```

---

### Where are repository files stored?

```bash
/etc/yum.repos.d/
```

---

### Which command updates all packages?

```bash
yum update
```

---

### Which command verifies package integrity?

```bash
rpm -V package
```

---

### Which command lists enabled repositories?

```bash
yum repolist
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `rpm -qa` | List installed packages |
| `rpm -qi pkg` | Package information |
| `rpm -ql pkg` | List package files |
| `rpm -qf file` | Find package owner |
| `rpm -ivh pkg.rpm` | Install RPM |
| `rpm -Uvh pkg.rpm` | Upgrade RPM |
| `rpm -e pkg` | Remove package |
| `yum install pkg` | Install package |
| `yum remove pkg` | Remove package |
| `yum update` | Update system |
| `yum search pkg` | Search package |
| `yum repolist` | Show repositories |
| `yum clean all` | Clean cache |
| `rpm --rebuilddb` | Rebuild RPM database |

# Exam Tip

For modern Linux administration:

- **RPM** = Low-level package management
- **YUM/DNF** = High-level package management
- Repository files are stored in:

```bash
/etc/yum.repos.d/
```

Most common production commands:

```bash
yum install package -y
yum update -y
yum repolist
rpm -qa
```

These commands are used daily by Linux System Administrators and DevOps Engineers.