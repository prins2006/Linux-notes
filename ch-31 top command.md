# `top` Command in Linux

## Introduction

The `top` command is a real-time system monitoring tool in Linux. It displays information about:

* Running processes
* CPU usage
* Memory usage
* System uptime
* Load average
* Process IDs (PID)
* User information

`top` is one of the most commonly used commands by Linux Administrators, DevOps Engineers, and SRE teams for production troubleshooting.

---

# Syntax

```bash
top
```

Simply type:

```bash
top
```

Example Output:

```text
top - 10:30:15 up 5 days, 4:20, 2 users, load average: 0.15, 0.20, 0.25
Tasks: 250 total,   1 running, 249 sleeping
%Cpu(s): 5.0 us, 2.0 sy, 93.0 id
MiB Mem : 7980 total, 2500 free, 3000 used, 2480 buff/cache
MiB Swap: 2048 total, 2048 free

PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
1 root 20 0 169000 12000 8000 S 0.0 0.1 0:05 systemd
1500 root 20 0 450000 30000 15000 S 5.0 1.5 0:20 nginx
```

---

# Understanding the Output

## 1. System Time and Uptime

```text
top - 10:30:15 up 5 days
```

| Field     | Meaning             |
| --------- | ------------------- |
| 10:30:15  | Current system time |
| up 5 days | Server uptime       |

---

## 2. Load Average

```text
load average: 0.15, 0.20, 0.25
```

Represents CPU workload for:

| Value  | Time       |
| ------ | ---------- |
| First  | 1 minute   |
| Second | 5 minutes  |
| Third  | 15 minutes |

### Production Rule

* Load < CPU cores → Healthy
* Load > CPU cores → Heavy load

---

## 3. Tasks

```text
Tasks: 250 total
```

| Field    | Meaning                   |
| -------- | ------------------------- |
| Total    | Total processes           |
| Running  | Active processes          |
| Sleeping | Waiting processes         |
| Stopped  | Paused                    |
| Zombie   | Completed but not cleaned |

---

## 4. CPU Usage

```text
%Cpu(s)
```

| Field | Meaning             |
| ----- | ------------------- |
| us    | User processes      |
| sy    | System processes    |
| ni    | Nice priority       |
| id    | Idle CPU            |
| wa    | Waiting for I/O     |
| hi    | Hardware interrupts |
| si    | Software interrupts |

Example:

```text
93% idle
```

Means CPU is mostly free.

---

## 5. Memory Usage

```text
MiB Mem
```

| Field      | Meaning       |
| ---------- | ------------- |
| Total      | Total RAM     |
| Free       | Available RAM |
| Used       | Used RAM      |
| Buff/Cache | Cached memory |

---

## 6. Swap Memory

```text
MiB Swap
```

Swap is disk space used when RAM becomes full.

---

# Process Section

| Column  | Meaning        |
| ------- | -------------- |
| PID     | Process ID     |
| USER    | Process owner  |
| PR      | Priority       |
| NI      | Nice value     |
| VIRT    | Virtual memory |
| RES     | Physical RAM   |
| SHR     | Shared memory  |
| S       | Process state  |
| %CPU    | CPU usage      |
| %MEM    | Memory usage   |
| TIME+   | CPU time       |
| COMMAND | Process name   |

---

# Common Interactive Commands

## Sort by CPU

Press:

```text
P
```

Highest CPU usage first.

---

## Sort by Memory

Press:

```text
M
```

Highest memory usage first.

---

## Kill a Process

Press:

```text
k
```

Enter PID:

```text
1500
```

Signal:

```text
15
```

---

## Change Refresh Time

Press:

```text
d
```

Enter:

```text
5
```

Refresh every 5 seconds.

---

## Show CPU Cores

Press:

```text
1
```

Displays usage for each CPU core.

---

## Exit

Press:

```text
q
```

---

# Useful Options

## Update Every 2 Seconds

```bash
top -d 2
```

---

## Show Specific User Processes

```bash
top -u root
```

---

## Batch Mode

```bash
top -b
```

Useful for scripting.

---

## Save Output

```bash
top -b -n 1 > top_report.txt
```

---

# Production Use Cases

## Check High CPU Usage

```bash
top
```

Press:

```text
P
```

Identify CPU-consuming process.

---

## Check High Memory Usage

```bash
top
```

Press:

```text
M
```

Find memory-intensive applications.

---

## Monitor Application

Example:

```bash
top -u nginx
```

Monitor Nginx processes.

---

## Troubleshoot Slow Server

Check:

* CPU
* RAM
* Swap
* Load average

---

# Real Production Scenario

A website is slow.

## Step 1

Run:

```bash
top
```

## Step 2

Check load average.

## Step 3

Sort by CPU:

```text
P
```

## Step 4

Find problematic process.

Example:

```text
java
```

## Step 5

Check PID:

```text
3456
```

## Step 6

Terminate if necessary:

```bash
kill 3456
```

Verify performance improvement.

---

# Difference Between ps and top

| ps              | top                  |
| --------------- | -------------------- |
| Static snapshot | Real-time monitoring |
| Single output   | Continuous updates   |
| Lightweight     | Interactive          |

---

# top vs htop

| top                | htop               |
| ------------------ | ------------------ |
| Default installed  | Extra package      |
| Basic interface    | Colorful interface |
| Keyboard shortcuts | Mouse support      |

Install htop:

Ubuntu:

```bash
sudo apt install htop
```

RHEL:

```bash
sudo yum install htop
```

---

# Important top Commands

```bash
top

top -d 2

top -u root

top -b

top -b -n 1

top -b -n 1 > report.txt
```

Interactive Keys:

| Key | Function       |
| --- | -------------- |
| P   | Sort CPU       |
| M   | Sort Memory    |
| k   | Kill Process   |
| d   | Change delay   |
| 1   | Show CPU cores |
| q   | Quit           |

---

# Interview Questions

## What is the top command?

A real-time Linux system monitoring tool.

---

## What information does top display?

* CPU usage
* Memory usage
* Running processes
* Load average
* PID
* User information

---

## Which key sorts by CPU usage?

```text
P
```

---

## Which key sorts by memory usage?

```text
M
```

---

## Which key kills a process?

```text
k
```

---

## Which key exits top?

```text
q
```

---

## How do you monitor processes for a specific user?

```bash
top -u username
```

Example:

```bash
top -u root
```

---

# Daily Practice Commands

```bash
top

top -d 2

top -u root

top -b

top -b -n 1

top -b -n 1 > report.txt
```

Practice Interactive Keys:

```text
P
M
k
d
1
q
```

---

# Summary

The `top` command is an essential Linux monitoring tool used in production environments.

## Key Features

* Real-time process monitoring.
* Displays CPU and memory usage.
* Shows load average and uptime.
* Helps identify high-resource processes.
* Supports process management.
* Useful for server troubleshooting and performance analysis.

## Most Common Production Workflow

```bash
top
```

Then:

1. Check load average.
2. Press `P` to sort by CPU.
3. Press `M` to sort by memory.
4. Identify problematic process.
5. Kill or restart the process if required.
6. Verify system performance.
