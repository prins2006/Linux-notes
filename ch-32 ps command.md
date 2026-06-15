# `ps` Command in Linux

## Introduction

The `ps` (**Process Status**) command is used to display information about currently running processes in Linux.

A **process** is a running instance of a program. The `ps` command helps Linux administrators monitor, troubleshoot, and manage these processes.

---

# Syntax

```bash
ps [OPTIONS]
```

---

# Basic ps Command

Display processes running in the current terminal.

```bash
ps
```

Example Output:

```text
PID TTY          TIME CMD
2345 pts/0   00:00:00 bash
3456 pts/0   00:00:00 ps
```

## Output Explanation

| Field | Meaning                              |
| ----- | ------------------------------------ |
| PID   | Process ID                           |
| TTY   | Terminal associated with the process |
| TIME  | CPU time consumed                    |
| CMD   | Command name                         |

---

# Common ps Options

## 1. Display All Processes

```bash
ps -A
```

or

```bash
ps -e
```

Shows every running process on the system.

### Production Use

View all active processes on a server.

---

## 2. Full Format Listing

```bash
ps -f
```

Example:

```text
UID   PID  PPID  C STIME TTY   TIME CMD
user 1234  1200  0 10:00 pts/0 00:00 bash
```

### Columns

| Field | Meaning           |
| ----- | ----------------- |
| UID   | User ID           |
| PID   | Process ID        |
| PPID  | Parent Process ID |
| C     | CPU Usage         |
| STIME | Start Time        |
| CMD   | Command           |

---

## 3. Full System Process List

```bash
ps -ef
```

Example:

```text
UID   PID  PPID CMD
root    1     0 /sbin/init
root 1200     1 sshd
root 1500     1 nginx
```

### Production Use

Most commonly used command for troubleshooting.

---

## 4. BSD Style Output

```bash
ps aux
```

Example:

```text
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
root  1  0.0  0.1 ...
```

### Column Meaning

| Column  | Description      |
| ------- | ---------------- |
| USER    | Process owner    |
| PID     | Process ID       |
| %CPU    | CPU usage        |
| %MEM    | Memory usage     |
| VSZ     | Virtual memory   |
| RSS     | Physical memory  |
| STAT    | Process state    |
| COMMAND | Executed command |

---

# Search for a Specific Process

## Using grep

```bash
ps -ef | grep nginx
```

Example:

```text
root 1500 1 nginx
```

Check SSH service:

```bash
ps -ef | grep ssh
```

Check Docker:

```bash
ps -ef | grep docker
```

### Production Use

Verify whether an application is running.

---

# Display Process Tree

```bash
ps -ejH
```

or

```bash
ps axjf
```

Shows parent-child relationships.

Example:

```text
systemd
 ├── sshd
 ├── nginx
 └── mysql
```

---

# Sort Processes by CPU Usage

```bash
ps aux --sort=-%cpu
```

Highest CPU-consuming processes appear first.

---

# Sort Processes by Memory Usage

```bash
ps aux --sort=-%mem
```

Highest memory-consuming processes appear first.

---

# Display Specific Columns

```bash
ps -eo pid,user,%cpu,%mem,cmd
```

Example:

```text
PID USER %CPU %MEM CMD
1 root 0.0 0.1 systemd
```

---

# Display Process by PID

```bash
ps -p 1234
```

Multiple PIDs:

```bash
ps -p 1234,5678
```

---

# Display Process for a User

```bash
ps -u root
```

Example:

```bash
ps -u ubuntu
```

---

# Process States

| State | Meaning  |
| ----- | -------- |
| R     | Running  |
| S     | Sleeping |
| D     | Waiting  |
| T     | Stopped  |
| Z     | Zombie   |

Example:

```bash
ps aux
```

STAT column:

```text
R
S
Z
```

---

# Production Use Cases

## 1. Check if Nginx is Running

```bash
ps -ef | grep nginx
```

---

## 2. Check SSH Service

```bash
ps -ef | grep ssh
```

---

## 3. Find High CPU Processes

```bash
ps aux --sort=-%cpu
```

---

## 4. Find High Memory Processes

```bash
ps aux --sort=-%mem
```

---

## 5. Verify Java Application

```bash
ps -ef | grep java
```

---

## 6. Check Docker Process

```bash
ps -ef | grep docker
```

---

# Real Production Scenario

Suppose a website is slow.

### Step 1

Check running web server:

```bash
ps -ef | grep nginx
```

### Step 2

Check CPU usage:

```bash
ps aux --sort=-%cpu
```

### Step 3

Check memory usage:

```bash
ps aux --sort=-%mem
```

### Step 4

Find application PID:

```bash
ps -ef | grep app
```

### Step 5

Kill problematic process if necessary:

```bash
kill PID
```

---

# Difference Between ps and top

| ps                    | top              |
| --------------------- | ---------------- |
| Static output         | Real-time output |
| Snapshot of processes | Live monitoring  |
| Lightweight           | Interactive      |

---

# Important ps Commands

```bash
ps

ps -A

ps -e

ps -f

ps -ef

ps aux

ps -ef | grep nginx

ps -ef | grep ssh

ps aux --sort=-%cpu

ps aux --sort=-%mem

ps -eo pid,user,%cpu,%mem,cmd

ps -u root

ps -p PID

ps -ejH

ps axjf
```

---

# Interview Questions

## What does ps stand for?

Process Status.

---

## What is the purpose of ps?

To display information about running processes.

---

## Which command displays all processes?

```bash
ps -e
```

or

```bash
ps -A
```

---

## Which command is commonly used in production?

```bash
ps -ef
```

---

## How do you search for a process?

```bash
ps -ef | grep process_name
```

Example:

```bash
ps -ef | grep nginx
```

---

## How do you display processes sorted by CPU usage?

```bash
ps aux --sort=-%cpu
```

---

## How do you display processes sorted by memory usage?

```bash
ps aux --sort=-%mem
```

---

# Daily Practice Commands

```bash
ps

ps -e

ps -A

ps -f

ps -ef

ps aux

ps -ef | grep ssh

ps -ef | grep nginx

ps aux --sort=-%cpu

ps aux --sort=-%mem

ps -eo pid,user,%cpu,%mem,cmd

ps -u root

ps -p 1

ps -ejH

ps axjf
```

---

# Summary

The `ps` command is one of the most important Linux process management tools.

## Key Points

* Displays running processes.
* Shows PID and PPID.
* Monitors CPU and memory usage.
* Finds specific application processes.
* Helps troubleshoot production issues.
* Commonly combined with `grep`.
* Frequently used by Linux Administrators, DevOps Engineers, and SRE teams.

**Most commonly used production commands:**

```bash
ps -ef
ps aux
ps -ef | grep nginx
ps aux --sort=-%cpu
ps aux --sort=-%mem
```
