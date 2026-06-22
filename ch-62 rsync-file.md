# rsync - Remote Synchronization in Linux

# Introduction

**rsync (Remote Synchronization)** is one of the most important Linux utilities used for:

- File synchronization
- Directory synchronization
- Incremental backups
- Remote backups
- Data migration
- Disaster recovery

It efficiently copies only the changed parts of files instead of copying everything again.

This makes rsync:

✅ Fast

✅ Bandwidth Efficient

✅ Reliable

✅ Widely Used in Production Environments

---

# What Does rsync Mean?

```text
rsync = Remote Sync
```

It synchronizes files and directories between:

- Local → Local
- Local → Remote
- Remote → Local
- Remote → Remote

---

# Why Use rsync?

Suppose you have:

```text
100 GB Backup Folder
```

You modify only:

```text
1 MB File
```

Normal copy:

```bash
cp -r backup/ destination/
```

Copies entire 100 GB again.

rsync:

```bash
rsync backup/ destination/
```

Copies only the changed data.

This is called:

```text
Incremental Synchronization
```

---

# How rsync Works

```text
Source
   |
   | Compare Changes
   v
Destination
```

rsync checks:

- File size
- Timestamp
- Checksums (optional)

Only changed files are transferred.

---

# Install rsync

## RHEL / Rocky / AlmaLinux

```bash
sudo dnf install rsync -y
```

---

## Ubuntu / Debian

```bash
sudo apt install rsync -y
```

---

# Check Version

```bash
rsync --version
```

Example:

```text
rsync version 3.2.7
```

---

# Basic Syntax

```bash
rsync [options] source destination
```

Example:

```bash
rsync file1.txt /backup/
```

---

# Local File Synchronization

Copy file:

```bash
rsync file.txt /backup/
```

---

# Copy Directory

```bash
rsync -r project/ /backup/
```

Option:

```text
-r = Recursive
```

---

# Most Important Option: Archive Mode

```bash
rsync -a source/ destination/
```

Archive mode preserves:

- Permissions
- Ownership
- Groups
- Timestamps
- Symbolic links

Equivalent to:

```text
-rlptgoD
```

Production environments typically use:

```bash
rsync -a
```

---

# Verbose Output

```bash
rsync -av source/ destination/
```

Options:

```text
-a = Archive
-v = Verbose
```

Example Output:

```text
sending incremental file list

file1.txt
file2.txt
```

---

# Show Progress

```bash
rsync -av --progress source/ destination/
```

Example:

```text
50%  100MB/s
```

Useful for large transfers.

---

# Dry Run (Simulation)

Test before copying:

```bash
rsync -av --dry-run source/ destination/
```

Nothing is copied.

Shows what would happen.

Production best practice before deleting files.

---

# Delete Extra Files

```bash
rsync -av --delete source/ destination/
```

Example:

```text
Source:
file1
file2

Destination:
file1
file2
file3
```

After sync:

```text
file3 removed
```

Destination becomes identical to source.

⚠ Be careful with --delete.

---

# Synchronizing Over SSH

Most common production use case.

Syntax:

```bash
rsync -av source/ user@server:/backup/
```

Example:

```bash
rsync -av project/ admin@192.168.1.100:/backup/
```

---

# Remote to Local

```bash
rsync -av user@server:/backup/ .
```

Example:

```bash
rsync -av admin@192.168.1.100:/backup/ /restore/
```

---

# Using Custom SSH Port

Default SSH:

```text
22
```

Custom SSH Port:

```bash
rsync -av -e "ssh -p 2222" source/ user@server:/backup/
```

---

# Compress During Transfer

```bash
rsync -avz source/ destination/
```

Option:

```text
-z = Compression
```

Useful for:

- Slow WAN links
- Remote sites
- Cloud transfers

---

# Preserve Permissions

```bash
rsync -a
```

Preserves:

```text
Permissions
Owner
Group
Timestamp
```

Very important for Linux backups.

---

# Synchronize Hidden Files

Hidden files:

```text
.bashrc
.profile
.ssh
```

rsync copies them automatically.

Example:

```bash
rsync -av /home/user/ /backup/user/
```

---

# Exclude Files

Exclude log files:

```bash
rsync -av --exclude="*.log" source/ destination/
```

---

# Exclude Multiple Patterns

```bash
rsync -av \
--exclude="*.log" \
--exclude="*.tmp" \
source/ destination/
```

---

# Backup Home Directory

```bash
rsync -av /home/prins/ /backup/prins/
```

Production example.

---

# Mirror Server Data

```bash
rsync -av --delete /data/ backup-server:/data/
```

Maintains exact copy.

---

# Create Daily Backup

```bash
rsync -av /var/www/ /backup/web/
```

Add to cron.

---

# rsync with SSH Keys

Generate key:

```bash
ssh-keygen
```

Copy key:

```bash
ssh-copy-id admin@server
```

Run backup:

```bash
rsync -av /data admin@server:/backup
```

No password required.

Perfect for automation.

---

# Using rsync in Cron Jobs

Edit cron:

```bash
crontab -e
```

Daily backup at 1 AM:

```bash
0 1 * * * rsync -av /data /backup
```

---

# Common rsync Options

| Option | Meaning |
|----------|----------|
| -a | Archive mode |
| -v | Verbose |
| -r | Recursive |
| -z | Compress |
| -h | Human readable |
| --progress | Show progress |
| --delete | Delete extra files |
| --dry-run | Test run |
| -e ssh | Use SSH |

---

# Understanding Trailing Slash

## Example 1

```bash
rsync -av source destination
```

Result:

```text
destination/source
```

---

## Example 2

```bash
rsync -av source/ destination
```

Result:

```text
Contents copied directly
```

Important interview question.

---

# Real Production Examples

## Website Backup

```bash
rsync -avz /var/www/ backup:/webbackup/
```

---

## Database Backup Transfer

```bash
rsync -av db.sql backup:/db/
```

---

## User Home Backup

```bash
rsync -av /home/ backup:/home-backup/
```

---

## Log Archive

```bash
rsync -av /var/log/ backup:/logs/
```

---

# Troubleshooting rsync

## Permission Denied

Check:

```bash
ls -ld source
```

Check remote user permissions.

---

## SSH Error

Test SSH:

```bash
ssh user@server
```

---

## Dry Run

```bash
rsync -av --dry-run source destination
```

Verify changes before syncing.

---

## Verbose Mode

```bash
rsync -avv source destination
```

More details.

---

# rsync vs cp

| Feature | rsync | cp |
|----------|--------|------|
| Incremental Copy | Yes | No |
| Network Transfer | Yes | No |
| Compression | Yes | No |
| SSH Support | Yes | No |
| Synchronization | Yes | No |
| Backup Friendly | Excellent | Basic |

---

# rsync vs scp

| Feature | rsync | scp |
|----------|---------|--------|
| Incremental Transfer | Yes | No |
| Resume Support | Better | Limited |
| Compression | Yes | Yes |
| Synchronization | Yes | No |
| Bandwidth Efficient | Yes | No |

---

# Interview Questions

### What is rsync?

A utility used for fast file and directory synchronization locally or remotely.

---

### Why is rsync faster than cp?

Because rsync transfers only changed data.

---

### Which protocol does rsync commonly use remotely?

```text
SSH
```

---

### How do you test rsync without copying files?

```bash
rsync --dry-run
```

---

### Which option preserves permissions and ownership?

```bash
-a
```

---

### How do you show transfer progress?

```bash
--progress
```

---

### Which option deletes files at destination that do not exist in source?

```bash
--delete
```

---

# Most Important Commands for Administrators

```bash
rsync -av source/ destination/
```

Basic synchronization

```bash
rsync -avz source/ server:/backup/
```

Remote synchronization

```bash
rsync -av --delete source/ destination/
```

Mirror directories

```bash
rsync -av --dry-run source/ destination/
```

Test synchronization

```bash
rsync -av --progress source/ destination/
```

Show progress

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `rsync -av` | Archive synchronization |
| `rsync -avz` | Compressed synchronization |
| `rsync --delete` | Mirror source exactly |
| `rsync --dry-run` | Test run |
| `rsync --progress` | Show transfer progress |
| `rsync -e ssh` | Use SSH |
| `rsync -av source/ server:/backup/` | Remote backup |

# Exam Tip

The most common production backup command is:

```bash
rsync -avz --delete /data/ backup-server:/backup/
```

Because it:

✅ Preserves permissions

✅ Uses compression

✅ Transfers only changes

✅ Synchronizes remote systems efficiently

✅ Works securely over SSH