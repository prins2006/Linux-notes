# 169. Downloading Files or Applications using `wget`

# Introduction

`wget` is a non-interactive command-line utility used to download files from the Internet or internal servers.

It supports:

- HTTP
- HTTPS
- FTP

Protocols and is commonly used by Linux Administrators, DevOps Engineers, and System Engineers.

---

# What Does wget Mean?

```text
wget = World Wide Web Get
```

It retrieves files from web servers and saves them locally.

---

# Why Use wget?

Common use cases:

- Download software packages
- Download ISO images
- Download scripts
- Download backups
- Download configuration files
- Automated downloads in shell scripts

---

# Install wget

## RHEL / Rocky / AlmaLinux

```bash
sudo dnf install wget -y
```

---

## Ubuntu / Debian

```bash
sudo apt install wget -y
```

---

# Check Version

```bash
wget --version
```

Example:

```text
GNU Wget 1.21.3
```

---

# Basic Syntax

```bash
wget URL
```

Example:

```bash
wget https://example.com/file.zip
```

---

# Download a File

```bash
wget https://example.com/file.tar.gz
```

Output:

```text
Saving to: file.tar.gz
```

File is saved in the current directory.

---

# Download and Save with Different Name

```bash
wget -O backup.tar.gz https://example.com/file.tar.gz
```

Option:

```text
-O = Output filename
```

---

# Download in Background

```bash
wget -b https://example.com/largefile.iso
```

Option:

```text
-b = Background
```

Check log:

```bash
tail -f wget-log
```

---

# Resume Interrupted Download

Very important in production.

```bash
wget -c https://example.com/file.iso
```

Option:

```text
-c = Continue
```

Example:

```text
Downloaded 4GB of 10GB
Network disconnected
```

Resume:

```bash
wget -c URL
```

Starts from 4GB instead of downloading again.

---

# Limit Download Speed

```bash
wget --limit-rate=500k URL
```

Example:

```bash
wget --limit-rate=1m URL
```

Useful to avoid consuming all bandwidth.

---

# Download Multiple Files

Create:

```bash
vi urls.txt
```

Example:

```text
https://example.com/file1.zip
https://example.com/file2.zip
https://example.com/file3.zip
```

Download:

```bash
wget -i urls.txt
```

Option:

```text
-i = Input file
```

---

# Download to Specific Directory

```bash
wget -P /backup https://example.com/file.tar.gz
```

Option:

```text
-P = Destination directory
```

---

# Download Without Certificate Check

⚠ Use only for testing.

```bash
wget --no-check-certificate URL
```

Useful when a server has a self-signed certificate.

---

# Download Using FTP

Example:

```bash
wget ftp://server/file.zip
```

---

# FTP Authentication

```bash
wget --ftp-user=username \
--ftp-password=password \
ftp://server/file.zip
```

---

# Download Entire Website (Mirror)

```bash
wget -m https://example.com
```

Option:

```text
-m = Mirror
```

Downloads website content recursively.

---

# Recursive Download

```bash
wget -r https://example.com/files/
```

Option:

```text
-r = Recursive
```

---

# Download Specific File Types

Example:

```bash
wget -r -A pdf https://example.com/docs/
```

Downloads only PDF files.

Option:

```text
-A = Accept file types
```

---

# Download with Authentication

```bash
wget \
--user=admin \
--password=secret \
https://example.com/file.zip
```

---

# Show HTTP Headers

```bash
wget --server-response URL
```

Useful for troubleshooting.

---

# Test URL Without Downloading

```bash
wget --spider URL
```

Example:

```bash
wget --spider https://example.com/file.iso
```

Checks whether file exists.

---

# Timestamping

Download only if newer version exists.

```bash
wget -N URL
```

Option:

```text
-N = Timestamping
```

Useful for update repositories.

---

# Quiet Mode

```bash
wget -q URL
```

Option:

```text
-q = Quiet
```

Commonly used in scripts.

---

# Logging Output

```bash
wget URL -o download.log
```

Store logs:

```text
download.log
```

---

# Production Examples

## Download Software Package

```bash
wget https://releases.ubuntu.com/ubuntu.iso
```

---

## Download Backup

```bash
wget -c https://backup-server/daily.tar.gz
```

---

## Download Script

```bash
wget https://example.com/install.sh
```

Verify:

```bash
cat install.sh
```

before running.

---

## Download RPM Package

```bash
wget https://repo/package.rpm
```

Install:

```bash
sudo rpm -ivh package.rpm
```

---

## Download DEB Package

```bash
wget https://repo/package.deb
```

Install:

```bash
sudo dpkg -i package.deb
```

---

# Security Best Practices

## Verify File Integrity

Download checksum:

```bash
wget https://example.com/file.sha256
```

Verify:

```bash
sha256sum file.iso
```

Compare values.

---

## Never Run Unknown Scripts Directly

Avoid:

```bash
wget URL -O - | bash
```

Safer:

```bash
wget URL
cat script.sh
```

Review first.

---

# Common wget Options

| Option | Meaning |
|----------|----------|
| -O | Save with custom name |
| -P | Save to directory |
| -c | Resume download |
| -b | Background mode |
| -i | Read URLs from file |
| -q | Quiet mode |
| -r | Recursive download |
| -m | Mirror website |
| -N | Download newer files only |
| --spider | Test URL |
| --limit-rate | Limit bandwidth |

---

# wget vs curl

| Feature | wget | curl |
|----------|------|------|
| Download Files | Excellent | Good |
| Resume Downloads | Yes | Yes |
| Recursive Download | Yes | No |
| Website Mirroring | Yes | No |
| API Testing | Limited | Excellent |
| HTTP Requests | Basic | Advanced |
| Scripts & Automation | Excellent | Excellent |

---

# Real Production Use Cases

## Download Nightly Backups

```bash
wget -c https://backupserver/nightly.tar.gz
```

---

## Mirror Repository

```bash
wget -m https://repo.company.com
```

---

## Download Monitoring Agent

```bash
wget https://repo/agent.rpm
```

---

## Script Automation

```bash
wget -q URL
```

Used in cron jobs and deployment scripts.

---

# Interview Questions

### What is wget?

A command-line utility used to download files from HTTP, HTTPS, and FTP servers.

---

### What does wget stand for?

```text
World Wide Web Get
```

---

### Which option resumes a download?

```bash
-c
```

---

### Download file with different name?

```bash
wget -O newname.zip URL
```

---

### Download in background?

```bash
wget -b URL
```

---

### Download multiple URLs from a file?

```bash
wget -i urls.txt
```

---

### Mirror a website?

```bash
wget -m URL
```

---

### Check URL without downloading?

```bash
wget --spider URL
```

---

# Most Important Commands for Administrators

```bash
wget URL
```
Download file

```bash
wget -c URL
```
Resume download

```bash
wget -O file.zip URL
```
Save with custom name

```bash
wget -P /backup URL
```
Save to specific directory

```bash
wget -i urls.txt
```
Download multiple files

```bash
wget --spider URL
```
Check file existence

```bash
wget -m URL
```
Mirror website

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `wget URL` | Download file |
| `wget -c URL` | Resume download |
| `wget -O name URL` | Rename output |
| `wget -P dir URL` | Save to directory |
| `wget -i file` | Multiple downloads |
| `wget -q URL` | Quiet mode |
| `wget -b URL` | Background download |
| `wget --spider URL` | Test URL |
| `wget -m URL` | Mirror website |

# Exam Tip

The most commonly used production commands are:

```bash
wget -c URL
```

Resume interrupted downloads.

```bash
wget -O filename URL
```

Save with a custom name.

```bash
wget -q URL
```

Silent downloads in scripts and cron jobs.

`wget` is one of the most important Linux utilities for downloading software, backups, packages, and automation resources from remote servers.