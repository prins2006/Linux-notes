# `crontab` Command in Linux

## What is crontab?

The `crontab` (Cron Table) command is used to **schedule tasks to run automatically** at specified times and dates in Linux.

The **cron daemon (`crond`)** runs in the background and executes scheduled jobs.

### Simple Definition

> **Crontab is a Linux utility used to schedule repetitive tasks automatically.**

Examples:

* Daily backups
* Log cleanup
* Database maintenance
* Sending emails
* Running shell scripts
* System monitoring

---

# What is Cron?

Cron is a background service that checks the crontab file every minute and executes scheduled jobs.

Check cron service:

Ubuntu/Debian:

```bash
systemctl status cron
```

RHEL/CentOS:

```bash
systemctl status crond
```

Start cron:

Ubuntu:

```bash
sudo systemctl start cron
```

RHEL:

```bash
sudo systemctl start crond
```

Enable at boot:

Ubuntu:

```bash
sudo systemctl enable cron
```

RHEL:

```bash
sudo systemctl enable crond
```

---

# Crontab Syntax

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of week (0-7)
│ │ │ └──── Month (1-12)
│ │ └────── Day of month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
```

---

# Crontab Fields

| Field        | Range |
| ------------ | ----- |
| Minute       | 0-59  |
| Hour         | 0-23  |
| Day of Month | 1-31  |
| Month        | 1-12  |
| Day of Week  | 0-7   |

Note:

```text
0 or 7 = Sunday
1 = Monday
2 = Tuesday
...
6 = Saturday
```

---

# Manage Crontab

## Display Current Jobs

```bash
crontab -l
```

---

## Edit Crontab

```bash
crontab -e
```

Example:

```text
0 2 * * * /home/user/backup.sh
```

Runs every day at 2:00 AM.

---

## Remove Crontab

```bash
crontab -r
```

Delete all scheduled jobs.

---

## Prompt Before Removal

```bash
crontab -i -r
```

---

# Common Examples

## Every Minute

```text
* * * * * command
```

Example:

```text
* * * * * date >> /tmp/time.log
```

---

## Every Hour

```text
0 * * * * command
```

Example:

```text
0 * * * * backup.sh
```

---

## Daily at Midnight

```text
0 0 * * * command
```

---

## Daily at 2 AM

```text
0 2 * * * backup.sh
```

---

## Every Sunday at 3 AM

```text
0 3 * * 0 backup.sh
```

---

## Every Monday at 8 AM

```text
0 8 * * 1 report.sh
```

---

## Every 5 Minutes

```text
*/5 * * * * command
```

Example:

```text
*/5 * * * * health_check.sh
```

---

## Every 10 Minutes

```text
*/10 * * * * command
```

---

## Every 30 Minutes

```text
*/30 * * * * command
```

---

## Every 2 Hours

```text
0 */2 * * * command
```

---

## Every Month

```text
0 0 1 * * command
```

Runs on the first day of every month.

---

# Special Strings

| String   | Meaning        |
| -------- | -------------- |
| @reboot  | Run at startup |
| @yearly  | Once per year  |
| @monthly | Once per month |
| @weekly  | Once per week  |
| @daily   | Once per day   |
| @hourly  | Once per hour  |

Example:

```text
@daily /home/user/backup.sh
```

---

# Production Use Cases

## Daily Backup

```text
0 2 * * * /opt/scripts/backup.sh
```

Runs daily at 2 AM.

---

## Log Cleanup

```text
0 1 * * 0 find /var/log -name "*.log" -mtime +30 -delete
```

Deletes logs older than 30 days every Sunday.

---

## Disk Monitoring

```text
*/5 * * * * df -h > /tmp/disk.txt
```

Checks disk usage every 5 minutes.

---

## Database Backup

```text
30 1 * * * mysqldump database > backup.sql
```

Runs daily at 1:30 AM.

---

# System Crontab

Location:

```text
/etc/crontab
```

View:

```bash
cat /etc/crontab
```

Example:

```text
17 * * * * root cd / && run-parts /etc/cron.hourly
```

---

# Cron Directories

| Directory         | Frequency |
| ----------------- | --------- |
| /etc/cron.hourly  | Hourly    |
| /etc/cron.daily   | Daily     |
| /etc/cron.weekly  | Weekly    |
| /etc/cron.monthly | Monthly   |

---

# Check Cron Logs

Ubuntu:

```bash
grep CRON /var/log/syslog
```

RHEL:

```bash
grep CRON /var/log/cron
```

Using journalctl:

```bash
journalctl -u cron
```

or

```bash
journalctl -u crond
```

---

# Real Production Scenario

Suppose a company needs database backups every day at 2 AM.

Create script:

```bash
backup.sh
```

Edit crontab:

```bash
crontab -e
```

Add:

```text
0 2 * * * /home/admin/backup.sh
```

Verify:

```bash
crontab -l
```

Check logs:

```bash
journalctl -u cron
```

---

# Important crontab Commands

Display jobs:

```bash
crontab -l
```

Edit jobs:

```bash
crontab -e
```

Remove jobs:

```bash
crontab -r
```

Check cron service:

```bash
systemctl status cron
```

Ubuntu:

```bash
systemctl restart cron
```

RHEL:

```bash
systemctl restart crond
```

---

# Interview Questions

## What is crontab?

A Linux utility used to schedule recurring tasks.

---

## What service executes cron jobs?

```text
cron
```

or

```text
crond
```

---

## Which command edits cron jobs?

```bash
crontab -e
```

---

## Which command displays cron jobs?

```bash
crontab -l
```

---

## Which command removes cron jobs?

```bash
crontab -r
```

---

## How do you run a job every 5 minutes?

```text
*/5 * * * *
```

---

## How do you run a job at system startup?

```text
@reboot
```

---

# Quick Revision Table

| Command               | Purpose            |
| --------------------- | ------------------ |
| crontab -l            | List cron jobs     |
| crontab -e            | Edit cron jobs     |
| crontab -r            | Remove cron jobs   |
| systemctl status cron | Check cron service |
| journalctl -u cron    | View cron logs     |

---

# Daily Practice

```bash
crontab -l

crontab -e

crontab -r

systemctl status cron

journalctl -u cron
```

Practice schedules:

```text
* * * * *
*/5 * * * *
0 * * * *
0 2 * * *
0 0 * * 0
@daily
@weekly
@monthly
@reboot
```

---

# Summary

The `crontab` command is one of the most important Linux automation tools.

## Production Uses

* Automated backups
* Log rotation
* Database maintenance
* Health checks
* Monitoring scripts
* Report generation
* Cleanup tasks

## Production Best Practice

1. Write and test the script.
2. Add the cron entry with `crontab -e`.
3. Verify using `crontab -l`.
4. Check cron service status.
5. Monitor logs to confirm successful execution.

Linux administrators and DevOps engineers use `crontab` extensively to automate routine system tasks.
