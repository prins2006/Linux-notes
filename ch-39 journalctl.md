# Linux `journalctl` for Troubleshooting and Log Analysis

# What is `journalctl`?

`journalctl` is a Linux command used to view and manage logs collected by **systemd-journald**.

Unlike traditional log files stored in `/var/log`, `journalctl` collects logs from:

* Kernel messages
* System services
* Applications
* Boot process
* User processes
* Authentication events

It provides a centralized logging system for troubleshooting Linux systems.

---

# What is systemd-journald?

`systemd-journald` is a logging service that collects and stores system logs.

Check its status:

```bash
systemctl status systemd-journald
```

Output:

```text
active (running)
```

---

# How journalctl Works

```
Applications
      |
Services
      |
Kernel
      |
Systemd
      |
systemd-journald
      |
journalctl
```

All logs are collected into a centralized journal database.

---

# Basic Syntax

```bash
journalctl [OPTIONS]
```

Display all logs:

```bash
journalctl
```

---

# View Latest Logs

```bash
journalctl -n 20
```

Shows the last 20 log entries.

Example:

```text
Jun 16 sshd: Accepted password
Jun 16 systemd: Started nginx
```

---

# Follow Logs in Real Time

Similar to `tail -f`.

```bash
journalctl -f
```

New logs appear automatically.

Stop:

```text
Ctrl + C
```

---

# View Current Boot Logs

```bash
journalctl -b
```

Displays logs since the last boot.

Example:

```text
Kernel initialized
Started Network Manager
Started SSH Server
```

---

# View Previous Boot Logs

List boots:

```bash
journalctl --list-boots
```

Example:

```text
-1 previous boot
0 current boot
```

Previous boot:

```bash
journalctl -b -1
```

Two boots ago:

```bash
journalctl -b -2
```

---

# View Kernel Messages

```bash
journalctl -k
```

Latest kernel messages:

```bash
journalctl -k -n 20
```

Equivalent to:

```bash
dmesg
```

---

# View Service Logs

## SSH

```bash
journalctl -u ssh
```

or

```bash
journalctl -u sshd
```

---

## Nginx

```bash
journalctl -u nginx
```

---

## Apache

```bash
journalctl -u apache2
```

---

## Docker

```bash
journalctl -u docker
```

---

## MySQL

```bash
journalctl -u mysql
```

---

# Live Service Monitoring

Example:

```bash
journalctl -u nginx -f
```

Watch nginx logs in real time.

---

# View Logs by Priority

Priority Levels:

| Number | Meaning     |
| ------ | ----------- |
| 0      | Emergency   |
| 1      | Alert       |
| 2      | Critical    |
| 3      | Error       |
| 4      | Warning     |
| 5      | Notice      |
| 6      | Information |
| 7      | Debug       |

Errors only:

```bash
journalctl -p err
```

Warnings and above:

```bash
journalctl -p warning
```

Critical:

```bash
journalctl -p crit
```

---

# View Logs Since Specific Time

Last hour:

```bash
journalctl --since "1 hour ago"
```

Today:

```bash
journalctl --since today
```

Yesterday:

```bash
journalctl --since yesterday
```

Specific date:

```bash
journalctl --since "2026-06-15"
```

---

# View Logs Until Time

```bash
journalctl --until today
```

Specific time:

```bash
journalctl --until "2026-06-16 10:00:00"
```

---

# Between Two Dates

```bash
journalctl --since "2026-06-15" --until "2026-06-16"
```

---

# View User Logs

Current user:

```bash
journalctl --user
```

Current session:

```bash
journalctl _UID=1000
```

---

# Search for Specific Process

Find PID:

```bash
ps -ef
```

View logs:

```bash
journalctl _PID=1234
```

---

# Search by Executable

```bash
journalctl /usr/sbin/sshd
```

---

# View Disk Usage

Check journal size:

```bash
journalctl --disk-usage
```

Example:

```text
Archived and active journals take up 500M
```

---

# Clean Old Logs

Keep only 100MB:

```bash
sudo journalctl --vacuum-size=100M
```

Keep 7 days:

```bash
sudo journalctl --vacuum-time=7d
```

---

# Save Logs to File

```bash
journalctl > logs.txt
```

Specific service:

```bash
journalctl -u nginx > nginx.log
```

---

# Common Troubleshooting Examples

## Server Failed to Boot

```bash
journalctl -b -1
```

Check previous boot errors.

---

## SSH Not Working

```bash
journalctl -u ssh
```

Live monitoring:

```bash
journalctl -u ssh -f
```

---

## Nginx Not Starting

Check service:

```bash
systemctl status nginx
```

Check logs:

```bash
journalctl -u nginx
```

---

## Docker Problems

```bash
journalctl -u docker
```

---

## High Kernel Errors

```bash
journalctl -k
```

---

## View Recent Errors

```bash
journalctl -p err -n 50
```

---

# Difference Between `journalctl` and Traditional Logs

| journalctl           | Traditional Logs     |
| -------------------- | -------------------- |
| Centralized          | Multiple files       |
| Binary journal       | Plain text           |
| Search by service    | Search file manually |
| Boot filtering       | Separate files       |
| Priority filtering   | Manual grep          |
| Real-time monitoring | tail -f              |

---

# Difference Between `journalctl -f` and `tail -f`

| journalctl -f      | tail -f     |
| ------------------ | ----------- |
| All systemd logs   | Single file |
| Service filtering  | File only   |
| Boot logs          | No          |
| Priority filtering | No          |
| Centralized        | No          |

---

# Production Scenario

Suppose:

User reports:

```
Website is down.
```

## Step 1

Check service:

```bash
systemctl status nginx
```

---

## Step 2

View logs:

```bash
journalctl -u nginx
```

---

## Step 3

Monitor live:

```bash
journalctl -u nginx -f
```

---

## Step 4

Check recent errors:

```bash
journalctl -p err
```

---

## Step 5

Check kernel:

```bash
journalctl -k
```

---

## Step 6

Check previous boot:

```bash
journalctl -b -1
```

---

# Most Important `journalctl` Commands

| Command                         | Purpose       |
| ------------------------------- | ------------- |
| `journalctl`                    | All logs      |
| `journalctl -n 20`              | Last 20 logs  |
| `journalctl -f`                 | Live logs     |
| `journalctl -b`                 | Current boot  |
| `journalctl -b -1`              | Previous boot |
| `journalctl -k`                 | Kernel logs   |
| `journalctl -u ssh`             | SSH service   |
| `journalctl -u nginx`           | Nginx logs    |
| `journalctl -p err`             | Error logs    |
| `journalctl --since today`      | Today's logs  |
| `journalctl --disk-usage`       | Journal size  |
| `journalctl --vacuum-size=100M` | Clean logs    |

---

# Interview Questions

## Q1. What is `journalctl`?

A command used to view logs collected by `systemd-journald`.

---

## Q2. Which service manages journal logs?

```
systemd-journald
```

---

## Q3. How do you monitor logs in real time?

```bash
journalctl -f
```

---

## Q4. How do you view logs for the previous boot?

```bash
journalctl -b -1
```

---

## Q5. How do you view logs for a specific service?

```bash
journalctl -u service_name
```

Example:

```bash
journalctl -u nginx
```

---

## Q6. How do you display only error messages?

```bash
journalctl -p err
```

---

# Production Best Practices

* Use `journalctl -u service` for service troubleshooting.
* Use `journalctl -b` after system boot issues.
* Monitor live logs with `journalctl -f`.
* Check kernel logs using `journalctl -k`.
* Regularly monitor journal size:

```bash
journalctl --disk-usage
```

* Clean old journals to prevent excessive disk usage.

---

# Quick Revision

| Command                       | Purpose                       |
| ----------------------------- | ----------------------------- |
| `journalctl`                  | View all logs                 |
| `journalctl -f`               | Live logs                     |
| `journalctl -b`               | Current boot                  |
| `journalctl -b -1`            | Previous boot                 |
| `journalctl -k`               | Kernel logs                   |
| `journalctl -u ssh`           | SSH logs                      |
| `journalctl -u nginx -f`      | Live Nginx logs               |
| `journalctl -p err`           | Error logs                    |
| `journalctl --since today`    | Today's logs                  |
| `journalctl --disk-usage`     | Journal size                  |
| `journalctl --vacuum-time=7d` | Delete logs older than 7 days |

---

# Summary

* `journalctl` is the primary log analysis tool for systemd-based Linux systems.
* It centralizes logs from the kernel, services, applications, and boot process.
* Key troubleshooting options include `-b` (boot), `-u` (service), `-k` (kernel), `-f` (live), and `-p` (priority).
* It simplifies production troubleshooting by allowing filtering by service, boot session, time range, and severity level.
* `journalctl` is one of the most important commands for Linux system administration and production support.




# Difference Between Log Monitoring (`/var/log`) and `journalctl`

## Introduction

Linux systems store logs in two main ways:

1. **Traditional Log Files (`/var/log`)**
2. **Systemd Journal (`journalctl`)**

Both are used for troubleshooting and monitoring but work differently.

---

# Architecture

## Traditional Logging

```
Applications
     |
Kernel
     |
Syslog Daemon (rsyslog/syslog-ng)
     |
/var/log/
     |
auth.log
syslog
kern.log
messages
```

---

## Systemd Logging

```
Applications
     |
Kernel
     |
Systemd Services
     |
systemd-journald
     |
journalctl
```

---

# 1. `/var/log`

## What is it?

A directory containing plain text log files.

Location:

```bash
ls /var/log
```

Example:

```
auth.log
syslog
kern.log
boot.log
```

---

## Common Commands

View file:

```bash
cat /var/log/syslog
```

Last 10 lines:

```bash
tail /var/log/syslog
```

Live monitoring:

```bash
tail -f /var/log/syslog
```

Search:

```bash
grep ssh /var/log/auth.log
```

---

## Common Log Files

| File | Purpose |
|-------|----------|
| syslog | General logs |
| auth.log | Authentication |
| kern.log | Kernel |
| boot.log | Boot |
| messages | General logs (RHEL) |
| secure | Authentication (RHEL) |

---

# 2. `journalctl`

## What is it?

A command that reads logs from the Systemd Journal database.

View all logs:

```bash
journalctl
```

Last 20 logs:

```bash
journalctl -n 20
```

Live monitoring:

```bash
journalctl -f
```

SSH logs:

```bash
journalctl -u ssh
```

Current boot:

```bash
journalctl -b
```

Previous boot:

```bash
journalctl -b -1
```

Kernel logs:

```bash
journalctl -k
```

---

# Main Difference

| Feature | /var/log | journalctl |
|----------|----------|------------|
| Storage | Text files | Binary journal |
| Managed by | rsyslog/syslog-ng | systemd-journald |
| Search by service | No | Yes |
| Boot filtering | No | Yes |
| Real-time monitoring | Yes | Yes |
| Priority filtering | No | Yes |
| Centralized logs | No | Yes |

---

# Log Storage

## `/var/log`

Files stored separately.

```
/var/log/

auth.log
syslog
kern.log
boot.log
```

Need to open individual files.

Example:

```bash
tail -f /var/log/auth.log
```

---

## `journalctl`

Everything stored centrally.

Example:

```bash
journalctl
```

Shows:

- Kernel
- SSH
- Docker
- Nginx
- Apache
- User services

---

# Service Log Monitoring

## `/var/log`

Find SSH logs:

```bash
grep ssh /var/log/auth.log
```

Find Nginx logs:

```bash
tail -f /var/log/nginx/access.log
```

---

## `journalctl`

SSH:

```bash
journalctl -u ssh
```

Nginx:

```bash
journalctl -u nginx
```

Docker:

```bash
journalctl -u docker
```

Apache:

```bash
journalctl -u apache2
```

---

# Boot Troubleshooting

## `/var/log`

```bash
cat /var/log/boot.log
```

Limited information.

---

## `journalctl`

Current boot:

```bash
journalctl -b
```

Previous boot:

```bash
journalctl -b -1
```

Very useful for boot failures.

---

# Kernel Messages

## `/var/log`

```bash
cat /var/log/kern.log
```

---

## `journalctl`

```bash
journalctl -k
```

Equivalent to:

```bash
dmesg
```

---

# Real-Time Monitoring

## `/var/log`

```bash
tail -f /var/log/syslog
```

Single file only.

---

## `journalctl`

```bash
journalctl -f
```

Entire system.

Specific service:

```bash
journalctl -u nginx -f
```

---

# Error Monitoring

## `/var/log`

Need grep:

```bash
grep ERROR /var/log/syslog
```

---

## `journalctl`

Errors:

```bash
journalctl -p err
```

Warnings:

```bash
journalctl -p warning
```

Critical:

```bash
journalctl -p crit
```

---

# Time Filtering

## `/var/log`

Need grep or manual search.

Example:

```bash
grep "Jun 16" /var/log/syslog
```

---

## `journalctl`

Today:

```bash
journalctl --since today
```

Last hour:

```bash
journalctl --since "1 hour ago"
```

Between dates:

```bash
journalctl --since "2026-06-15" --until "2026-06-16"
```

---

# Production Troubleshooting

## SSH Login Failure

### Traditional

```bash
tail -f /var/log/auth.log
```

### Systemd

```bash
journalctl -u ssh -f
```

---

## Server Boot Failure

Traditional:

```bash
cat /var/log/boot.log
```

Systemd:

```bash
journalctl -b -1
```

---

## Nginx Not Starting

Traditional:

```bash
tail -f /var/log/nginx/error.log
```

Systemd:

```bash
journalctl -u nginx
```

---

## Hardware Issues

Traditional:

```bash
cat /var/log/kern.log
```

Systemd:

```bash
journalctl -k
```

---

# Advantages

## `/var/log`

### Pros

- Simple text files.
- Easy to use with cat, grep, tail.
- Supported on almost all Linux distributions.

### Cons

- Logs spread across many files.
- Difficult to correlate events.
- No built-in service filtering.

---

## `journalctl`

### Pros

- Centralized logging.
- Filter by service.
- Filter by boot.
- Filter by priority.
- Filter by time.
- Real-time monitoring.

### Cons

- Binary storage.
- Requires systemd.
- Needs journalctl command to read.

---

# Interview Questions

## Q1. What is `/var/log`?

A directory that stores traditional Linux log files.

---

## Q2. What is `journalctl`?

A command to view logs collected by `systemd-journald`.

---

## Q3. Which service manages `journalctl` logs?

```
systemd-journald
```

---

## Q4. Which command shows previous boot logs?

```bash
journalctl -b -1
```

---

## Q5. Which command monitors SSH logs?

Traditional:

```bash
tail -f /var/log/auth.log
```

Systemd:

```bash
journalctl -u ssh -f
```

---

# Quick Revision Table

| Task | `/var/log` | `journalctl` |
|-------|------------|--------------|
| General logs | `/var/log/syslog` | `journalctl` |
| Authentication | `/var/log/auth.log` | `journalctl -u ssh` |
| Kernel logs | `/var/log/kern.log` | `journalctl -k` |
| Boot logs | `/var/log/boot.log` | `journalctl -b` |
| Live monitoring | `tail -f` | `journalctl -f` |
| Previous boot | Not available | `journalctl -b -1` |
| Error logs | `grep ERROR` | `journalctl -p err` |
| Service logs | Individual files | `journalctl -u service` |

---

# Which One Should You Use?

| Situation | Recommended |
|-----------|------------|
| Read a specific log file | `/var/log` |
| Monitor application logs | `/var/log` |
| Troubleshoot systemd services | `journalctl` |
| Investigate boot failures | `journalctl` |
| Check kernel events | `journalctl -k` |
| Live system monitoring | `journalctl -f` |
| Production troubleshooting | `journalctl` |

---

# Summary

- **`/var/log`** is the traditional Linux logging system that stores logs as plain text files.
- **`journalctl`** is the modern Systemd logging utility that stores centralized binary logs.
- `/var/log` is useful for direct file-based log monitoring with commands like `cat`, `grep`, and `tail`.
- `journalctl` provides advanced filtering by service, boot session, priority, and time.
- In modern Linux production environments, administrators commonly use **both**: `/var/log` for application-specific logs and `journalctl` for system and service troubleshooting.CH-40