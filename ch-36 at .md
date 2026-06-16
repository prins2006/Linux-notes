# Linux `at` Command

## What is the `at` Command?

The `at` command in Linux is used to schedule a command or script to run **once at a specific date and time**.

Unlike `cron`, which runs jobs repeatedly, `at` executes the task only one time and then removes it from the queue.

---

# Syntax

```bash
at [OPTION] TIME
```

Basic syntax:

```bash
at 10:30
```

After pressing Enter, type the commands you want to execute.

Finish by pressing:

```text
Ctrl + D
```

---

# Check if `at` is Installed

Ubuntu/Debian:

```bash
which at
```

or

```bash
at -V
```

If not installed:

```bash
sudo apt update
sudo apt install at
```

Start the service:

```bash
sudo systemctl enable atd
sudo systemctl start atd
```

Check status:

```bash
systemctl status atd
```

---

# Time Formats

## Run at a Specific Time

```bash
at 3:00 PM
```

---

## Run at Midnight

```bash
at midnight
```

---

## Run at Noon

```bash
at noon
```

---

## Run After 10 Minutes

```bash
at now + 10 minutes
```

---

## Run After 1 Hour

```bash
at now + 1 hour
```

---

## Run Tomorrow

```bash
at tomorrow
```

---

## Run Tomorrow at 9 AM

```bash
at 9:00 AM tomorrow
```

---

## Run Next Week

```bash
at now + 1 week
```

---

# Example 1: Create a File

Schedule:

```bash
at now + 2 minutes
```

Type:

```bash
touch /tmp/testfile
```

Exit:

```text
Ctrl + D
```

After two minutes:

```bash
ls /tmp
```

Output:

```text
testfile
```

---

# Example 2: Shutdown System

```bash
at 11:00 PM
```

Command:

```bash
shutdown -h now
```

Finish:

```text
Ctrl + D
```

---

# Example 3: Run a Script

Create script:

```bash
nano backup.sh
```

```bash
#!/bin/bash
tar -czf backup.tar.gz /home/user
```

Make executable:

```bash
chmod +x backup.sh
```

Schedule:

```bash
at 8:00 PM
```

Execute:

```bash
/home/user/backup.sh
```

---

# View Scheduled Jobs

```bash
atq
```

Example:

```text
1 Tue Jun 16 20:00:00 2026 a user
2 Tue Jun 16 21:00:00 2026 a user
```

---

# Alternative Command

```bash
at -l
```

Shows the same job queue.

---

# Remove Scheduled Job

Check jobs:

```bash
atq
```

Suppose:

```text
5 Tue Jun 16 20:00:00
```

Remove:

```bash
atrm 5
```

or

```bash
at -r 5
```

Check again:

```bash
atq
```

Job is removed.

---

# View Job Details

List jobs:

```bash
atq
```

Suppose Job ID:

```text
3
```

Display commands:

```bash
at -c 3
```

---

# Read Commands from a File

Create file:

```bash
nano commands.txt
```

Contents:

```bash
date
uptime
who
```

Run:

```bash
at -f commands.txt now + 5 minutes
```

---

# Production Use Cases

## 1. Schedule a Backup

```bash
at 11:30 PM
```

```bash
tar -czf backup.tar.gz /data
```

---

## 2. Restart a Service

```bash
at 2:00 AM
```

```bash
systemctl restart nginx
```

---

## 3. Delete Temporary Files

```bash
at now + 30 minutes
```

```bash
rm -rf /tmp/test/*
```

---

## 4. Run Maintenance Script

```bash
at 1:00 AM
```

```bash
/opt/scripts/maintenance.sh
```

---

# Important Files

| File             | Purpose                   |
| ---------------- | ------------------------- |
| `/etc/at.allow`  | Users allowed to use `at` |
| `/etc/at.deny`   | Users denied access       |
| `/var/spool/at/` | Stores scheduled jobs     |

---

# Difference Between `at` and `cron`

| at                          | cron               |
| --------------------------- | ------------------ |
| One-time execution          | Repeated execution |
| Specific date and time      | Recurring schedule |
| Simple scheduling           | Complex scheduling |
| Job removed after execution | Runs until removed |

Example:

**at**

```bash
at 5:00 PM
```

Runs once.

**cron**

```text
0 17 * * *
```

Runs every day at 5 PM.

---

# Common `at` Commands Cheat Sheet

| Command               | Purpose                       |
| --------------------- | ----------------------------- |
| `at 5:00 PM`          | Schedule a job                |
| `at now + 10 minutes` | Run after 10 minutes          |
| `atq`                 | List jobs                     |
| `at -l`               | List jobs                     |
| `atrm JOBID`          | Remove job                    |
| `at -r JOBID`         | Remove job                    |
| `at -c JOBID`         | View job details              |
| `at -f file time`     | Schedule commands from a file |

---

# Interview Questions

## Q1. What is the `at` command?

A Linux utility used to schedule a command or script for one-time execution at a specified time.

---

## Q2. Which service is required for `at`?

`atd`

Check:

```bash
systemctl status atd
```

---

## Q3. How do you list scheduled jobs?

```bash
atq
```

---

## Q4. How do you remove a scheduled job?

```bash
atrm JOB_ID
```

---

## Q5. What is the difference between `at` and `cron`?

* `at` executes a task once.
* `cron` executes tasks repeatedly based on a schedule.

---

# Production Best Practices

* Verify the `atd` service is running.
* Use absolute file paths in commands and scripts.
* Redirect output to log files for troubleshooting.
* Test scripts manually before scheduling.
* Use `cron` for recurring tasks and `at` for one-time tasks.

---

# Summary

* `at` schedules one-time tasks in Linux.
* Requires the `atd` service.
* `atq` lists scheduled jobs.
* `atrm` removes jobs.
* `at -c` displays job details.
* Commonly used for backups, maintenance, service restarts, and delayed command execution.
* For repeated tasks, use `cron`; for one-time tasks, use `at`.



# Difference Between `cron` and `at` Commands in Linux

Both `cron` and `at` are Linux job scheduling utilities, but they are used for different purposes.

| Feature              | `at` Command                          | `cron` Command                        |
| -------------------- | ------------------------------------- | ------------------------------------- |
| Purpose              | Schedule a job to run **once**        | Schedule a job to run **repeatedly**  |
| Execution            | One-time execution                    | Recurring execution                   |
| Schedule Type        | Specific date and time                | Minute, hour, day, month, and weekday |
| Job Removal          | Automatically removed after execution | Runs until removed from crontab       |
| Configuration        | Simple                                | More flexible                         |
| Service Required     | `atd`                                 | `cron` or `crond`                     |
| Command to View Jobs | `atq`                                 | `crontab -l`                          |
| Remove Job           | `atrm JOB_ID`                         | `crontab -e` or `crontab -r`          |
| Best Use Case        | One-time tasks                        | Periodic tasks                        |

---

# `at` Command

## Purpose

Execute a command or script only once at a specified time.

### Example

Run a backup at 8:00 PM today:

```bash
at 8:00 PM
```

Enter the command:

```bash
tar -czf /backup/home.tar.gz /home
```

Press:

```text
Ctrl + D
```

After execution, the job is automatically deleted.

### Use Cases

* One-time backup.
* Schedule a server reboot.
* Run a maintenance script once.
* Delete temporary files after a delay.

---

# `cron` Command

## Purpose

Execute commands repeatedly according to a schedule.

### Example

Open crontab:

```bash
crontab -e
```

Run a backup every day at 2:00 AM:

```text
0 2 * * * tar -czf /backup/home.tar.gz /home
```

This job runs daily until removed.

### Use Cases

* Daily backups.
* Weekly log cleanup.
* Monthly report generation.
* Regular database maintenance.
* Automated monitoring scripts.

---

# Scheduling Examples

## Run Once

Using `at`:

```bash
at 10:00 PM
```

Runs only one time.

---

## Run Every Day

Using `cron`:

```text
0 22 * * *
```

Runs every day at 10:00 PM.

---

# Service Comparison

## `at`

Check service:

```bash
systemctl status atd
```

Start service:

```bash
sudo systemctl start atd
```

Enable at boot:

```bash
sudo systemctl enable atd
```

---

## `cron`

Check service:

```bash
systemctl status cron
```

Start service:

```bash
sudo systemctl start cron
```

Enable at boot:

```bash
sudo systemctl enable cron
```

---

# Job Management

## `at`

List jobs:

```bash
atq
```

Remove a job:

```bash
atrm JOB_ID
```

View job details:

```bash
at -c JOB_ID
```

---

## `cron`

View jobs:

```bash
crontab -l
```

Edit jobs:

```bash
crontab -e
```

Remove all jobs:

```bash
crontab -r
```

---

# Production Examples

| Task                        | Recommended Tool |
| --------------------------- | ---------------- |
| Restart server once tonight | `at`             |
| Daily backup                | `cron`           |
| Weekly log cleanup          | `cron`           |
| One-time maintenance        | `at`             |
| Monthly report              | `cron`           |
| Delayed file deletion       | `at`             |

---

# Interview Questions

## Q1. What is the main difference between `at` and `cron`?

**Answer:**

* `at` executes a task once.
* `cron` executes tasks repeatedly based on a schedule.

---

## Q2. Which service does `at` use?

```text
atd
```

---

## Q3. Which service does `cron` use?

```text
cron (or crond)
```

---

## Q4. How do you list scheduled `at` jobs?

```bash
atq
```

---

## Q5. How do you list cron jobs?

```bash
crontab -l
```

---

# Quick Revision Table

| Command      | Purpose               |
| ------------ | --------------------- |
| `at 5:00 PM` | Schedule one-time job |
| `atq`        | List `at` jobs        |
| `atrm 1`     | Remove `at` job       |
| `crontab -e` | Edit cron jobs        |
| `crontab -l` | List cron jobs        |
| `crontab -r` | Remove cron jobs      |

---

# Summary

* **`at`** → One-time task scheduling.
* **`cron`** → Recurring task scheduling.
* **`atd`** service manages `at` jobs.
* **`cron`** service manages cron jobs.
* Use **`at`** for temporary or one-time operations.
* Use **`cron`** for automated, repetitive production tasks like backups, monitoring, and maintenance.
