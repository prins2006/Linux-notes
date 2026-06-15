# What are Processes in Linux?

## Introduction

A **process** in Linux is an instance of a running program. Whenever you execute a command or start an application, Linux creates a process to perform the requested task.

For example:

* Running `ls` creates a process.
* Opening a web browser creates one or more processes.
* Starting a web server like Nginx creates a background process.

In simple terms:

> **Program + Execution = Process**

---

# Process Lifecycle

A process goes through several stages during its lifetime:

```
New
 |
 v
Ready
 |
 v
Running
 |
 +----> Waiting
 |         |
 |         v
 +------ Running
 |
 v
Terminated
```

## Process States

| State      | Description                              |
| ---------- | ---------------------------------------- |
| New        | Process is being created.                |
| Ready      | Waiting for CPU time.                    |
| Running    | Currently executing.                     |
| Waiting    | Waiting for an event or resource.        |
| Stopped    | Temporarily paused.                      |
| Zombie     | Process finished but entry still exists. |
| Terminated | Process has ended.                       |

---

# Process ID (PID)

Every process has a unique number called the **Process ID (PID)**.

Example:

```bash
ps
```

Output:

```
 PID TTY          TIME CMD
1234 pts/0    00:00:00 bash
5678 pts/0    00:00:00 ps
```

Here:

* `1234` is the PID of the bash shell.
* `5678` is the PID of the ps command.

---

# Parent Process (PPID)

Every process is started by another process except the first system process.

Example:

```bash
ps -ef
```

Output:

```
UID   PID  PPID CMD
root    1     0 /sbin/init
root 1200     1 nginx
```

| Field | Meaning           |
| ----- | ----------------- |
| PID   | Process ID        |
| PPID  | Parent Process ID |

---

# Process Types

## 1. Foreground Process

Runs in the terminal and interacts with the user.

Example:

```bash
cat
```

The terminal waits until the command finishes.

---

## 2. Background Process

Runs without blocking the terminal.

Example:

```bash
sleep 100 &
```

Output:

```
[1] 2345
```

The process runs in the background.

---

# Viewing Processes

## ps Command

Displays active processes.

```bash
ps
```

### Show all processes

```bash
ps -e
```

### Full details

```bash
ps -ef
```

---

# top Command

Displays real-time process information.

```bash
top
```

Information shown:

* CPU usage
* Memory usage
* Running processes
* Load average

Exit:

```
q
```

---

# htop Command

Interactive process viewer.

```bash
htop
```

Features:

* Color interface
* Process search
* Easy process management

Install:

Ubuntu:

```bash
sudo apt install htop
```

RHEL:

```bash
sudo yum install htop
```

---

# Process Priority

Linux assigns priorities to processes.

## nice Value

Range:

```
-20 to 19
```

| Value | Priority |
| ----- | -------- |
| -20   | Highest  |
| 0     | Default  |
| 19    | Lowest   |

Example:

```bash
nice -n 10 sleep 100
```

---

# Kill a Process

## kill Command

Syntax:

```bash
kill PID
```

Example:

```bash
kill 1234
```

---

## Force Kill

```bash
kill -9 PID
```

Example:

```bash
kill -9 1234
```

---

## Kill by Process Name

```bash
pkill nginx
```

or

```bash
killall nginx
```

---

# Jobs Command

Shows background jobs.

```bash
jobs
```

Example:

```
[1]+ Running sleep 100 &
```

---

# Bring Background Job to Foreground

```bash
fg
```

Specific job:

```bash
fg %1
```

---

# Send Foreground Job to Background

Press:

```
Ctrl + Z
```

Then:

```bash
bg
```

---

# Zombie Process

A zombie process is a completed process whose parent has not collected its exit status.

Check:

```bash
ps aux | grep Z
```

---

# Daemon Process

A daemon is a background service process.

Examples:

* sshd
* nginx
* cron
* systemd

Check:

```bash
ps -ef
```

---

# Production Use Cases

## Web Server Down

Check process:

```bash
ps -ef | grep nginx
```

Restart service:

```bash
sudo systemctl restart nginx
```

---

## High CPU Usage

Check:

```bash
top
```

Find PID and terminate:

```bash
kill PID
```

---

## High Memory Usage

Sort by memory:

```bash
ps aux --sort=-%mem
```

---

# Important Process Commands

| Command | Purpose                     |
| ------- | --------------------------- |
| ps      | Display processes           |
| ps -ef  | Full process list           |
| top     | Real-time monitoring        |
| htop    | Interactive monitoring      |
| jobs    | Show background jobs        |
| bg      | Resume in background        |
| fg      | Bring to foreground         |
| kill    | Terminate process           |
| kill -9 | Force terminate             |
| pkill   | Kill by name                |
| killall | Kill all matching processes |
| nice    | Start with priority         |
| renice  | Change priority             |

---

# Production Troubleshooting Example

Suppose a web application is slow.

### Step 1

Check CPU usage:

```bash
top
```

### Step 2

Find application process:

```bash
ps -ef | grep app
```

### Step 3

Check memory usage:

```bash
ps aux --sort=-%mem
```

### Step 4

Restart service if necessary:

```bash
sudo systemctl restart app
```

### Step 5

Verify:

```bash
systemctl status app
```

---

# Interview Questions

## What is a process?

A process is a running instance of a program.

---

## What is PID?

PID stands for Process ID, a unique number assigned to every process.

---

## What is PPID?

PPID is the Parent Process ID.

---

## Difference between foreground and background processes?

| Foreground       | Background            |
| ---------------- | --------------------- |
| Uses terminal    | Runs independently    |
| User interaction | No direct interaction |

---

## Which command displays running processes?

```bash
ps
```

---

## Which command shows real-time process information?

```bash
top
```

---

## How do you terminate a process?

```bash
kill PID
```

Force termination:

```bash
kill -9 PID
```

---

# Daily Practice Commands

```bash
ps
ps -e
ps -ef

top

jobs

sleep 100 &
jobs

fg
bg

kill PID
kill -9 PID

pkill nginx

killall nginx

ps aux --sort=-%cpu
ps aux --sort=-%mem

nice -n 10 sleep 100

renice 5 PID
```

---

# Summary

A Linux process is a running program managed by the kernel. Understanding processes is essential for Linux system administration and production support.

Key concepts:

* Process
* PID and PPID
* Foreground and background processes
* Process states
* Process priority
* Process monitoring
* Process termination
* Production troubleshooting
* Daemon and zombie processes

These concepts are used daily by Linux administrators, DevOps engineers, and SRE teams.
