# Create Local Repository (YUM/DNF Server)

# Introduction

A **Local Repository (YUM Repository Server)** is a centralized server that stores RPM packages and provides them to client systems.

Instead of downloading packages from the Internet every time, systems download packages from the local repository.

Used in:

- Data Centers
- Enterprise Networks
- Production Environments
- Secure/Internal Networks
- Air-Gapped Environments

---

# Why Create a Local Repository?

Without Local Repository:

```text
Server1 ---> Internet
Server2 ---> Internet
Server3 ---> Internet
Server4 ---> Internet
```

Problems:

- Consumes Internet bandwidth
- Slow downloads
- Difficult to manage updates
- Internet dependency

---

With Local Repository:

```text
                Repository Server
                        |
     ---------------------------------------
     |             |             |         |
 Server1       Server2       Server3   Server4
```

Benefits:

✅ Faster package installation

✅ Centralized package management

✅ Reduced bandwidth usage

✅ Better security

✅ Controlled updates

---

# Repository Types

## Online Repository

Example:

```text
https://mirror.rockylinux.org
```

Requires Internet access.

---

## Local Repository

Example:

```text
http://192.168.1.100/repo
```

Hosted inside organization.

---

# Components Required

Repository Server:

```text
Apache HTTP Server
RPM Packages
createrepo Utility
```

Clients:

```text
YUM/DNF Configuration
```

---

# Lab Environment

Repository Server:

```text
IP: 192.168.1.100
Hostname: repo-server
```

Client:

```text
IP: 192.168.1.101
Hostname: client1
```

---

# Step 1: Install Required Packages

Install Apache:

```bash
sudo dnf install httpd -y
```

Install repository tools:

```bash
sudo dnf install createrepo_c -y
```

Verify:

```bash
rpm -qa | grep createrepo
```

---

# Step 2: Create Repository Directory

Example:

```bash
mkdir -p /var/www/html/repo
```

Verify:

```bash
ls -ld /var/www/html/repo
```

---

# Step 3: Copy RPM Packages

Create package directory:

```bash
mkdir /tmp/rpms
```

Copy RPMs:

```bash
cp *.rpm /var/www/html/repo/
```

Example:

```bash
cp nginx*.rpm /var/www/html/repo/
cp wget*.rpm /var/www/html/repo/
```

---

# Step 4: Create Repository Metadata

Generate repository information:

```bash
createrepo /var/www/html/repo
```

Output:

```text
Spawning worker...
Saving Primary metadata
Saving filelists metadata
```

New directory created:

```text
repodata/
```

Verify:

```bash
ls /var/www/html/repo
```

Example:

```text
nginx.rpm
wget.rpm
repodata
```

---

# What is repodata?

Repository metadata used by:

- YUM
- DNF

Contains:

```text
Package Names
Dependencies
Versions
Checksums
```

Required for repository functionality.

---

# Step 5: Start Apache Server

Start service:

```bash
systemctl start httpd
```

Enable at boot:

```bash
systemctl enable httpd
```

Check status:

```bash
systemctl status httpd
```

---

# Step 6: Configure Firewall

Open HTTP service:

```bash
firewall-cmd --permanent --add-service=http
```

Reload firewall:

```bash
firewall-cmd --reload
```

Verify:

```bash
firewall-cmd --list-services
```

---

# Step 7: Configure SELinux

Restore contexts:

```bash
restorecon -Rv /var/www/html
```

Verify:

```bash
ls -Zd /var/www/html/repo
```

---

# Step 8: Test Repository from Browser

Open:

```text
http://192.168.1.100/repo
```

or

```bash
curl http://192.168.1.100/repo
```

Verify RPM files are visible.

---

# Client Configuration

# Step 9: Create Repository File

Client system:

```bash
cd /etc/yum.repos.d/
```

Create:

```bash
vi local.repo
```

Contents:

```ini
[localrepo]
name=Local Repository
baseurl=http://192.168.1.100/repo
enabled=1
gpgcheck=0
```

---

# Repository Parameters

## Repository ID

```ini
[localrepo]
```

Unique repository name.

---

## Repository Description

```ini
name=Local Repository
```

---

## Repository URL

```ini
baseurl=http://192.168.1.100/repo
```

Location of packages.

---

## Enable Repository

```ini
enabled=1
```

Enable repository.

Disable:

```ini
enabled=0
```

---

## GPG Check

```ini
gpgcheck=0
```

Disable signature verification.

Production:

```ini
gpgcheck=1
```

Recommended.

---

# Step 10: Clear Existing Cache

```bash
dnf clean all
```

---

# Step 11: Rebuild Cache

```bash
dnf makecache
```

Output:

```text
Metadata cache created.
```

---

# Step 12: Verify Repository

```bash
dnf repolist
```

Output:

```text
repo id      repo name

localrepo    Local Repository
```

---

# Step 13: Install Package

Example:

```bash
dnf install nginx
```

DNF downloads package from:

```text
http://192.168.1.100/repo
```

---

# Offline Repository from DVD ISO

Common enterprise method.

Mount ISO:

```bash
mount /dev/cdrom /mnt
```

Create repo file:

```bash
vi /etc/yum.repos.d/dvd.repo
```

Contents:

```ini
[dvd]

name=DVD Repo

baseurl=file:///mnt

enabled=1

gpgcheck=0
```

---

Verify:

```bash
dnf repolist
```

---

# Updating Local Repository

Add new RPMs:

```bash
cp *.rpm /var/www/html/repo
```

Regenerate metadata:

```bash
createrepo --update /var/www/html/repo
```

Important:

```text
Always run createrepo after adding RPMs.
```

---

# Troubleshooting

## Repository Not Visible

Check Apache:

```bash
systemctl status httpd
```

---

Check Port 80:

```bash
ss -tulnp | grep :80
```

---

## Metadata Error

Recreate metadata:

```bash
createrepo --update /var/www/html/repo
```

---

## Repository Cache Error

Clear cache:

```bash
dnf clean all
```

Rebuild:

```bash
dnf makecache
```

---

## Firewall Issue

Check:

```bash
firewall-cmd --list-services
```

Open HTTP:

```bash
firewall-cmd --add-service=http --permanent
```

---

## SELinux Issue

Check:

```bash
getenforce
```

Restore labels:

```bash
restorecon -Rv /var/www/html
```

---

# Real Production Use Cases

## Data Center Repository

```text
Central Repository Server
```

Provides packages to:

- 100+ Linux servers
- No Internet required

---

## Security Controlled Environment

Only approved packages are stored.

Benefits:

```text
Compliance
Auditability
Security
```

---

## Air-Gapped Networks

Servers have:

```text
No Internet Access
```

Local repository provides updates.

---

# Interview Questions

### What is a YUM Repository?

A location containing RPM packages and metadata used by YUM/DNF.

---

### Which command creates repository metadata?

```bash
createrepo /path/to/repo
```

---

### Where are repository configuration files stored?

```bash
/etc/yum.repos.d/
```

---

### Which directory contains repository metadata?

```text
repodata/
```

---

### Which service is commonly used to host repositories?

```text
Apache (httpd)
```

---

### How do you refresh DNF cache?

```bash
dnf makecache
```

---

### How do you view enabled repositories?

```bash
dnf repolist
```

---

# Most Important Commands

Create repository:

```bash
createrepo /var/www/html/repo
```

Update repository:

```bash
createrepo --update /var/www/html/repo
```

Start Apache:

```bash
systemctl start httpd
```

View repositories:

```bash
dnf repolist
```

Refresh cache:

```bash
dnf makecache
```

Install package:

```bash
dnf install package
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `createrepo repo/` | Create repository metadata |
| `createrepo --update repo/` | Update repository |
| `dnf repolist` | Show repositories |
| `dnf makecache` | Refresh metadata |
| `dnf clean all` | Clear cache |
| `systemctl start httpd` | Start web server |
| `firewall-cmd --add-service=http` | Open HTTP port |
| `restorecon -Rv /var/www/html` | Fix SELinux contexts |

# Exam Tip

The most important steps in creating a Local YUM Repository are:

```bash
1. Install httpd and createrepo
2. Copy RPM packages
3. Run createrepo
4. Start Apache
5. Configure client repo file
6. Run dnf makecache
7. Install packages from repository
```

This is a very common RHCSA, RHCE, and Linux System Administrator interview topic.