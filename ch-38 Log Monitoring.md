# Linux System Log Monitoring (`/var/log`)

# What is `/var/log`?

`/var/log` is a standard Linux directory that stores **system and application log files**.

Logs record important events such as:

* System boot.
* User logins.
* Authentication attempts.
* Service start and stop events.
* Application errors.
* Kernel messages.
* Network activities.

System administrators use these logs for:

* Troubleshooting.
* Security auditing.
* Performance monitoring.
* Incident investigation.

---

# What is a Log File?

A log file is a text file that records system events.

Example:

```text
Jun 16 10:30:15 server sshd[1234]: Accepted password for user
```

This indicates:

* Date and time
* Hostname
* Service
* Event

---

# `/var/log` Directory Structure

View log files:

```bash
ls -l /var/log
```

Example output:

```text
auth.log
syslog
kern.log
boot.log
dmesg
messages
lastlog
wtmp
btmp
dpkg.log
apt
journal
```

---

# Important Log Files

| File                       | Purpose                       |
| -------------------------- | ----------------------------- |
| `/var/log/syslog`          | General system logs           |
| `/var/log/messages`        | System messages (RHEL/CentOS) |
| `/var/log/auth.log`        | Authentication logs           |
| `/var/log/secure`          | Authentication logs (RHEL)    |
| `/var/log/kern.log`        | Kernel messages               |
| `/var/log/dmesg`           | Boot kernel messages          |
| `/var/log/boot.log`        | Boot process                  |
| `/var/log/lastlog`         | Last login information        |
| `/var/log/wtmp`            | Login/logout history          |
| `/var/log/btmp`            | Failed login attempts         |
| `/var/log/dpkg.log`        | Package installation          |
| `/var/log/apt/history.log` | APT package history           |

---

# 1. syslog

## Purpose

General system activity.

Ubuntu/Debian:

```bash
cat /var/log/syslog
```

View latest entries:

```bash
tail /var/log/syslog
```

Live monitoring:

```bash
tail -f /var/log/syslog
```

Example:

```text
Jun 16 systemd: Started Apache Server.
```

---

# 2. messages

RHEL/CentOS:

```bash
cat /var/log/messages
```

Live monitoring:

```bash
tail -f /var/log/messages
```

Contains:

* System messages
* Hardware events
* Services

---

# 3. auth.log

Ubuntu authentication log.

```bash
cat /var/log/auth.log
```

Monitor:

```bash
tail -f /var/log/auth.log
```

Example:

```text
Accepted password for user
```

Shows:

* SSH login
* sudo usage
* Authentication failures

---

# 4. secure

RHEL authentication log.

```bash
tail -f /var/log/secure
```

---

# 5. kern.log

Kernel messages.

```bash
cat /var/log/kern.log
```

Monitor:

```bash
tail -f /var/log/kern.log
```

Contains:

* Driver messages
* Hardware detection
* Kernel warnings

---

# 6. dmesg

Boot kernel messages.

View:

```bash
dmesg
```

Human-readable:

```bash
dmesg -T
```

Latest:

```bash
dmesg | tail
```

---

# 7. boot.log

System boot process.

```bash
cat /var/log/boot.log
```

Shows:

* Services started
* Boot errors

---

# 8. lastlog

Displays last login for every user.

Command:

```bash
lastlog
```

Example:

```text
Username Port From Latest
root pts/0 192.168.1.10
```

---

# 9. wtmp

Stores login/logout history.

View:

```bash
last
```

Example:

```text
user pts/0 192.168.1.10
```

---

# 10. btmp

Stores failed login attempts.

View:

```bash
lastb
```

Example:

```text
root ssh:notty 192.168.1.20
```

Requires root:

```bash
sudo lastb
```

---

# 11. dpkg.log

Package installation history.

```bash
cat /var/log/dpkg.log
```

Example:

```text
install nginx
upgrade apache2
```

---

# 12. apt Logs

APT history:

```bash
cat /var/log/apt/history.log
```

APT terminal output:

```bash
cat /var/log/apt/term.log
```

---

# Useful Log Monitoring Commands

## View Entire File

```bash
cat logfile
```

---

## Last 10 Lines

```bash
tail logfile
```

---

## Real-time Monitoring

```bash
tail -f logfile
```

---

## Search Errors

```bash
grep ERROR logfile
```

---

## Search SSH Logins

```bash
grep ssh /var/log/auth.log
```

---

## Search Failed Logins

```bash
grep Failed /var/log/auth.log
```

---

## Count Failed Logins

```bash
grep Failed /var/log/auth.log | wc -l
```

---

# journalctl

Modern Linux distributions use Systemd.

View logs:

```bash
journalctl
```

Current boot:

```bash
journalctl -b
```

SSH service:

```bash
journalctl -u ssh
```

Latest logs:

```bash
journalctl -n 20
```

Live logs:

```bash
journalctl -f
```

---

# Log Rotation

## What is Log Rotation?

Large log files are automatically compressed and archived.

Managed by:

```text
logrotate
```

Configuration:

```text
/etc/logrotate.conf
```

Directory:

```text
/etc/logrotate.d/
```

Example:

```
syslog

syslog.1

syslog.2.gz
```

---

# Production Troubleshooting

## Server Not Booting

Check:

```bash
cat /var/log/boot.log
```

---

## SSH Login Problems

Check:

```bash
tail -f /var/log/auth.log
```

---

## Failed Login Attempts

```bash
sudo lastb
```

---

## Hardware Detection

```bash
dmesg
```

---

## Service Failure

```bash
journalctl -u nginx
```

---

## Package Installation History

```bash
cat /var/log/dpkg.log
```

---

# Difference Between Important Log Files

| Log File        | Purpose               |
| --------------- | --------------------- |
| syslog          | General system logs   |
| messages        | General logs (RHEL)   |
| auth.log        | Authentication        |
| secure          | Authentication (RHEL) |
| kern.log        | Kernel messages       |
| dmesg           | Boot kernel logs      |
| boot.log        | Boot process          |
| lastlog         | Last user login       |
| wtmp            | Login history         |
| btmp            | Failed logins         |
| dpkg.log        | Package history       |
| apt/history.log | APT operations        |

---

# Interview Questions

## Q1. What is `/var/log`?

A directory containing Linux system and application log files.

---

## Q2. Which log stores SSH login information?

Ubuntu:

```text
/var/log/auth.log
```

RHEL:

```text
/var/log/secure
```

---

## Q3. Which command displays failed logins?

```bash
lastb
```

---

## Q4. Which command displays login history?

```bash
last
```

---

## Q5. Which command monitors logs in real time?

```bash
tail -f logfile
```

or

```bash
journalctl -f
```

---

## Q6. What is `logrotate`?

A Linux utility that automatically rotates, compresses, and manages old log files.

---

# Production Best Practices

* Regularly monitor `auth.log` for unauthorized access.
* Use `tail -f` or `journalctl -f` for live troubleshooting.
* Check `dmesg` after hardware changes.
* Monitor disk usage because log files can grow large:

```bash
df -h
```

* Configure `logrotate` properly.
* Restrict access to sensitive log files.

---

# Quick Revision

| Command                     | Purpose                  |
| --------------------------- | ------------------------ |
| `ls /var/log`               | List logs                |
| `tail -f /var/log/syslog`   | Live system logs         |
| `tail -f /var/log/auth.log` | Live authentication logs |
| `dmesg -T`                  | Kernel messages          |
| `last`                      | Login history            |
| `lastb`                     | Failed logins            |
| `lastlog`                   | Last login per user      |
| `journalctl -f`             | Live systemd logs        |
| `journalctl -u ssh`         | SSH service logs         |

---

# Summary

* `/var/log` stores Linux system and application logs.
* `syslog` and `messages` contain general system events.
* `auth.log` and `secure` track authentication activities.
* `kern.log` and `dmesg` contain kernel and hardware information.
* `last`, `lastb`, and `lastlog` provide user login history.
* `journalctl` is the modern log viewer for systemd-based systems.
* `logrotate` manages and archives old log files.
* Log monitoring is a critical skill for Linux administration, security auditing, and production troubleshooting.
