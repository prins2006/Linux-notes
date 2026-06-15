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


# CPU States in Linux (`top` Command)

## Introduction

When you run the `top` command, you may see output similar to:

```text
%Cpu(s): 15.2 us, 5.3 sy, 0.0 ni, 75.4 id, 3.1 wa, 0.5 hi, 0.5 si, 0.0 st
```

These values represent how the CPU spends its time. Understanding them is essential for Linux system administration and production troubleshooting.

---

# CPU States Overview

| Field | Full Form          | Meaning                                        |
| ----- | ------------------ | ---------------------------------------------- |
| us    | User Space         | Time spent running user applications           |
| sy    | System Space       | Time spent by the Linux kernel                 |
| ni    | Nice               | Time spent on low-priority processes           |
| id    | Idle               | CPU is not doing any work                      |
| wa    | I/O Wait           | CPU waiting for disk or network operations     |
| hi    | Hardware Interrupt | CPU servicing hardware devices                 |
| si    | Software Interrupt | CPU servicing software interrupts              |
| st    | Steal Time         | Time stolen by a hypervisor (virtual machines) |

---

# 1. us (User Space CPU Time)

## What is it?

CPU time spent executing user applications.

Examples:

* Chrome
* Firefox
* Python programs
* Java applications
* Nginx workers
* MySQL queries

Example:

```text
us = 30%
```

The CPU spends 30% of its time running user applications.

## Production Example

A Java application receives many requests.

```
Java Application
       |
       v
Linux CPU
       |
       v
us increases
```

High `us` often means:

* Heavy application workload.
* Many active users.
* CPU-intensive computations.

## Troubleshooting

Find high CPU processes:

```bash
top
```

Press:

```
P
```

Or:

```bash
ps aux --sort=-%cpu
```

---

# 2. sy (System CPU Time)

## What is it?

CPU time spent running Linux kernel operations.

Kernel tasks include:

* Process management.
* Memory management.
* File system operations.
* Network packet handling.
* Device drivers.

Example:

```text
sy = 15%
```

The kernel uses 15% CPU.

## Production Example

A web server receives many requests.

```
Client
  |
  v
Kernel
  |
  +--> Network
  +--> File System
  +--> Memory
```

The kernel workload increases.

High `sy` may indicate:

* Heavy disk activity.
* Network traffic.
* Frequent system calls.
* Driver problems.

---

# 3. ni (Nice CPU Time)

## What is it?

CPU time used by low-priority processes.

Linux process priority:

| Nice Value | Priority |
| ---------- | -------- |
| -20        | Highest  |
| 0          | Default  |
| 19         | Lowest   |

Example:

```bash
nice -n 10 python backup.py
```

The backup process runs with lower priority.

Example:

```text
ni = 5%
```

5% CPU is used by nice processes.

## Production Example

Background backup jobs.

```bash
nice -n 19 tar -czf backup.tar.gz /data
```

The backup does not affect production services.

---

# 4. id (Idle CPU Time)

## What is it?

CPU time when no tasks are running.

Example:

```text
id = 80%
```

CPU is idle 80% of the time.

## Healthy Server

```
CPU:
20% busy
80% idle
```

High idle means:

* Server has spare capacity.
* Normal workload.

Low idle means:

* CPU heavily utilized.

---

# 5. wa (I/O Wait)

## What is it?

CPU waits for Input/Output operations.

Examples:

* Reading files.
* Writing logs.
* Database access.
* Network storage.
* SSD or HDD operations.

Example:

```text
wa = 20%
```

CPU waits 20% of the time for I/O.

## Production Example

MySQL query:

```
Application
    |
    v
Database
    |
    v
Disk Read
    |
CPU Waiting
```

High `wa` indicates:

* Slow disks.
* Storage bottlenecks.
* Heavy database operations.
* NFS delays.

## Troubleshooting

Check:

```bash
top
```

Use:

```bash
iostat
```

or

```bash
iotop
```

---

# 6. hi (Hardware Interrupts)

## What is it?

CPU time spent handling hardware devices.

Examples:

* Keyboard.
* Mouse.
* Network cards.
* Disk controllers.
* USB devices.

Example:

```text
hi = 1%
```

Hardware interrupts consume 1% CPU.

## Production Example

High network traffic:

```
Network Card
      |
      v
Hardware Interrupt
      |
      v
CPU
```

High `hi` may indicate:

* Network floods.
* Hardware problems.
* Heavy disk activity.

---

# 7. si (Software Interrupts)

## What is it?

CPU time spent processing software interrupts.

Generated by:

* Networking stack.
* Kernel scheduling.
* Software drivers.

Example:

```text
si = 2%
```

Software interrupts consume 2% CPU.

## Production Example

Millions of network packets:

```
Packet
  |
  v
Kernel
  |
Software Interrupt
  |
CPU
```

High `si` may indicate:

* Heavy network traffic.
* Packet processing.
* Software driver activity.

---

# 8. st (Steal Time)

## Virtual Machines Only

Example:

```text
st = 10%
```

The hypervisor takes 10% CPU time away from your VM.

High steal time means:

* Host server overloaded.
* Too many virtual machines.

Common in:

* AWS
* Azure
* VMware
* KVM

---

# Production Interpretation

## Healthy Server

```text
us = 20%
sy = 5%
ni = 0%
id = 70%
wa = 3%
hi = 1%
si = 1%
st = 0%
```

Healthy workload.

---

## High CPU Application

```text
us = 85%
sy = 10%
id = 5%
```

Application uses most CPU.

Check:

```bash
top
```

---

## Disk Bottleneck

```text
wa = 40%
```

Likely storage issue.

Check:

```bash
iostat
```

---

## Network Flood

```text
hi = 10%
si = 20%
```

Heavy interrupt processing.

Check:

```bash
sar -n DEV
```

---

## Virtual Machine Issue

```text
st = 25%
```

Host machine is overloaded.

---

# Real Production Scenario

Suppose a website is slow.

Run:

```bash
top
```

Output:

```text
us 70%
sy 15%
id 5%
wa 8%
hi 1%
si 1%
```

Analysis:

| CPU State | Interpretation               |
| --------- | ---------------------------- |
| us 70%    | Application workload is high |
| sy 15%    | Kernel activity is moderate  |
| id 5%     | CPU almost fully utilized    |
| wa 8%     | Some disk waiting            |
| hi 1%     | Hardware normal              |
| si 1%     | Software interrupts normal   |

Action:

1. Identify CPU-heavy processes.
2. Optimize application.
3. Scale resources if needed.

---

# Interview Questions

## What does `us` mean?

CPU time spent running user applications.

---

## What does `sy` mean?

CPU time spent in the Linux kernel.

---

## What does `ni` mean?

CPU time consumed by low-priority (nice) processes.

---

## What does `id` mean?

Idle CPU time.

---

## What does `wa` indicate?

CPU waiting for disk or I/O operations.

---

## What are `hi` and `si`?

* `hi`: Hardware interrupts.
* `si`: Software interrupts.

---

## What is `st`?

CPU time stolen by the hypervisor in virtual machines.

---

# Quick Revision Table

| Field | Meaning             | High Value Indicates      |
| ----- | ------------------- | ------------------------- |
| us    | User applications   | Heavy application load    |
| sy    | Kernel operations   | System overhead           |
| ni    | Nice processes      | Background jobs           |
| id    | Idle CPU            | Available CPU             |
| wa    | I/O wait            | Slow storage              |
| hi    | Hardware interrupts | Device activity           |
| si    | Software interrupts | Network/software activity |
| st    | VM steal time       | Host overload             |

---

# Summary

When analyzing a Linux server with `top`:

* **High `us`** → Application load.
* **High `sy`** → Kernel workload.
* **High `ni`** → Background low-priority jobs.
* **High `id`** → Healthy spare CPU.
* **High `wa`** → Disk or storage bottleneck.
* **High `hi`** → Hardware interrupt activity.
* **High `si`** → Network/software interrupt processing.
* **High `st`** → Virtual machine host contention.

Understanding these CPU states helps Linux administrators quickly diagnose performance issues in production environments.


# Understanding `top` Command Process Columns

When you run the `top` command:

```bash
top
```

You will see a process table similar to:

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
```

Each column provides important information about a running process. Linux administrators use these columns daily for production monitoring and troubleshooting.

---

# Process Table

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
```

| Column  | Full Form       | Description                        |
| ------- | --------------- | ---------------------------------- |
| PID     | Process ID      | Unique process number              |
| USER    | User Name       | Owner of the process               |
| PR      | Priority        | Scheduling priority                |
| NI      | Nice Value      | User-defined priority              |
| VIRT    | Virtual Memory  | Total virtual memory used          |
| RES     | Resident Memory | Physical RAM used                  |
| SHR     | Shared Memory   | Shared memory with other processes |
| S       | State           | Current process state              |
| %CPU    | CPU Usage       | CPU consumed by process            |
| %MEM    | Memory Usage    | RAM consumed                       |
| TIME+   | CPU Time        | Total CPU time used                |
| COMMAND | Command Name    | Process or application name        |

---

# 1. PID (Process ID)

## What is PID?

PID is a unique number assigned to every running process.

Example:

```text
PID
1500
```

Process ID = 1500

## Production Use

Find process:

```bash
ps -p 1500
```

Kill process:

```bash
kill 1500
```

Example:

```text
PID COMMAND
1   systemd
234 sshd
1500 nginx
```

---

# 2. USER

## What is USER?

Shows who started the process.

Example:

```text
USER
root
nginx
mysql
ubuntu
```

## Production Example

```text
USER COMMAND
root systemd
mysql mysqld
nginx nginx
```

Useful for:

* Security auditing.
* User activity monitoring.
* Permission troubleshooting.

---

# 3. PR (Priority)

## What is PR?

Priority determines how important a process is to the CPU scheduler.

Example:

```text
PR
20
```

Common values:

| PR    | Meaning         |
| ----- | --------------- |
| 20    | Normal          |
| Lower | Higher priority |
| RT    | Real-time       |

Example:

```text
PR 20
```

Normal Linux process.

## Production Example

Critical applications may have higher priorities.

---

# 4. NI (Nice Value)

## What is NI?

User-controlled priority adjustment.

Range:

```text
-20 to 19
```

| NI  | Priority |
| --- | -------- |
| -20 | Highest  |
| 0   | Default  |
| 19  | Lowest   |

Example:

```bash
nice -n 10 backup.sh
```

Background jobs use lower priority.

---

# 5. VIRT (Virtual Memory)

## What is VIRT?

Total virtual memory allocated.

Includes:

* Physical RAM
* Shared libraries
* Memory-mapped files
* Swap
* Reserved memory

Example:

```text
VIRT
2G
```

Virtual memory = 2 GB.

## Production Example

Java applications often have large VIRT values.

---

# 6. RES (Resident Memory)

## What is RES?

Actual RAM currently used by the process.

Example:

```text
RES
500M
```

Uses 500 MB physical RAM.

## Production Example

MySQL:

```text
mysqld
RES 2G
```

Database occupies 2 GB RAM.

---

# 7. SHR (Shared Memory)

## What is SHR?

Memory shared with other processes.

Examples:

* Shared libraries
* Common files
* Shared cache

Example:

```text
SHR
20M
```

20 MB shared memory.

## Production Example

Multiple Nginx workers share libraries.

---

# 8. S (Process State)

## What is S?

Current status of the process.

| State | Meaning         |
| ----- | --------------- |
| R     | Running         |
| S     | Sleeping        |
| D     | Waiting for I/O |
| T     | Stopped         |
| Z     | Zombie          |

Example:

```text
S
R
```

Running process.

Example:

```text
S
S
```

Sleeping process.

---

# 9. %CPU

## What is %CPU?

Percentage of CPU currently used.

Example:

```text
%CPU
75.5
```

Process uses 75.5% CPU.

## Production Example

Find CPU-heavy process.

Sort by CPU:

Inside top:

```text
P
```

---

# 10. %MEM

## What is %MEM?

Percentage of physical RAM used.

Example:

```text
%MEM
15.2
```

Uses 15.2% RAM.

## Production Example

Sort by memory:

Inside top:

```text
M
```

---

# 11. TIME+

## What is TIME+?

Total CPU time consumed since the process started.

Example:

```text
TIME+
15:45.22
```

Process used 15 minutes and 45 seconds of CPU time.

## Production Example

Long-running services accumulate CPU time.

---

# 12. COMMAND

## What is COMMAND?

Shows the executable or application name.

Example:

```text
COMMAND
systemd
nginx
mysqld
java
python3
docker
```

## Production Example

Find Java application:

```text
COMMAND
java
```

---

# Example Process

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

1500 nginx 20 0 500M 120M 20M S 25.0 3.5 10:25 nginx
```

## Analysis

| Field   | Value           |
| ------- | --------------- |
| PID     | 1500            |
| USER    | nginx           |
| PR      | Normal priority |
| NI      | Default         |
| VIRT    | 500 MB          |
| RES     | 120 MB RAM      |
| SHR     | 20 MB shared    |
| S       | Sleeping        |
| %CPU    | 25%             |
| %MEM    | 3.5%            |
| TIME+   | 10 min 25 sec   |
| COMMAND | nginx           |

---

# Production Scenario

A website becomes slow.

Run:

```bash
top
```

Output:

```text
PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND

3456 java 20 0 8G 4G 50M R 95 45 120:30 java
```

Analysis:

| Field | Meaning        |
| ----- | -------------- |
| PID   | 3456           |
| USER  | java user      |
| VIRT  | 8 GB allocated |
| RES   | 4 GB RAM used  |
| CPU   | 95%            |
| MEM   | 45%            |
| State | Running        |

Action:

1. Investigate application.
2. Check logs.
3. Restart service if needed.

---

# Interview Questions

## What is PID?

Process ID.

---

## What is USER?

Owner of the process.

---

## Difference between PR and NI?

| PR                         | NI                    |
| -------------------------- | --------------------- |
| Kernel scheduling priority | User-defined priority |

---

## What is VIRT?

Total virtual memory allocated.

---

## What is RES?

Actual physical RAM used.

---

## What is SHR?

Shared memory used with other processes.

---

## What does S mean?

Current process state.

---

## What is %CPU?

CPU usage percentage.

---

## What is %MEM?

RAM usage percentage.

---

## What is TIME+?

Total CPU time consumed by the process.

---

## What is COMMAND?

Name of the running program.

---

# Quick Revision Table

| Column  | Meaning          |
| ------- | ---------------- |
| PID     | Process ID       |
| USER    | Process Owner    |
| PR      | Process Priority |
| NI      | Nice Value       |
| VIRT    | Virtual Memory   |
| RES     | Physical RAM     |
| SHR     | Shared Memory    |
| S       | Process State    |
| %CPU    | CPU Usage        |
| %MEM    | RAM Usage        |
| TIME+   | Total CPU Time   |
| COMMAND | Program Name     |

---

# Most Important Production Columns

Linux administrators usually focus on:

```text
PID
USER
RES
S
%CPU
%MEM
COMMAND
```

These columns help identify:

* High CPU usage
* High memory consumption
* Running applications
* Process owners
* Application health
* Performance bottlenecks
