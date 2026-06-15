# System Utility Commands in Linux

## Overview

System utility commands are basic Linux commands used to display system information, monitor system status, perform calculations, and locate executable programs. These commands are frequently used by Linux administrators and DevOps engineers.

---

# 1. `date` Command

## Purpose

Displays the current system date and time.

## Syntax

```bash
date
```

## Common Examples

### Display current date and time

```bash
date
```

Example Output:

```
Mon Jun 15 10:30:45 IST 2026
```

### Display date in custom format

```bash
date +"%d-%m-%Y"
```

Output:

```
15-06-2026
```

### Display time only

```bash
date +"%H:%M:%S"
```

Output:

```
10:30:45
```

### Display day name

```bash
date +"%A"
```

Output:

```
Monday
```

## Production Use Cases

* Check server time.
* Verify cron jobs.
* Generate log timestamps.
* Backup scheduling.

---

# 2. `uptime` Command

## Purpose

Shows how long the system has been running.

## Syntax

```bash
uptime
```

Example Output:

```
10:45:00 up 5 days, 4:20, 2 users, load average: 0.15, 0.20, 0.25
```

## Understanding the Output

| Field        | Description                          |
| ------------ | ------------------------------------ |
| Current Time | Current system time                  |
| Up Time      | How long the server has been running |
| Users        | Number of logged-in users            |
| Load Average | CPU workload                         |

### Human-readable format

```bash
uptime -p
```

Example:

```
up 5 days, 4 hours, 20 minutes
```

## Production Use Cases

* Check server health.
* Verify unexpected reboots.
* Monitor system performance.

---

# 3. `hostname` Command

## Purpose

Displays the system hostname.

## Syntax

```bash
hostname
```

Example:

```
SF-CPU-769
```

### Display IP Address

```bash
hostname -I
```

Example:

```
192.168.1.100
```

### Display Fully Qualified Domain Name

```bash
hostname -f
```

Example:

```
server.company.com
```

## Production Use Cases

* Identify servers.
* Verify SSH connections.
* Network troubleshooting.

---

# 4. `uname` Command

## Purpose

Displays Linux kernel and system information.

## Syntax

```bash
uname [OPTION]
```

### Display Kernel Name

```bash
uname
```

Output:

```
Linux
```

### Display Kernel Version

```bash
uname -r
```

### Display Machine Architecture

```bash
uname -m
```

Output:

```
x86_64
```

### Display All Information

```bash
uname -a
```

Example:

```
Linux server 6.8.0-60-generic x86_64 GNU/Linux
```

## Production Use Cases

* Check OS details.
* Verify kernel version.
* Software compatibility checks.

---

# 5. `which` Command

## Purpose

Finds the location of an executable command.

## Syntax

```bash
which command_name
```

## Examples

```bash
which ls
```

Output:

```
/usr/bin/ls
```

```bash
which git
```

Output:

```
/usr/bin/git
```

```bash
which python3
```

Output:

```
/usr/bin/python3
```

## Production Use Cases

* Verify installed software.
* Debug PATH issues.
* Locate executable files.

---

# 6. `cal` Command

## Purpose

Displays a calendar.

## Syntax

```bash
cal
```

Example:

```
     June 2026
Su Mo Tu We Th Fr Sa
   1  2  3  4  5  6
7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30
```

### Display Full Year

```bash
cal 2026
```

### Display Specific Month

```bash
cal 12 2026
```

## Production Use Cases

* Scheduling.
* Maintenance planning.
* Administrative tasks.

---

# 7. `bc` Command

## Purpose

`bc` stands for Basic Calculator.

It is a command-line calculator for arithmetic operations.

## Syntax

```bash
bc
```

### Addition

```bash
echo "10+20" | bc
```

Output:

```
30
```

### Subtraction

```bash
echo "50-15" | bc
```

Output:

```
35
```

### Multiplication

```bash
echo "10*5" | bc
```

Output:

```
50
```

### Division

```bash
echo "20/3" | bc
```

Output:

```
6
```

### Decimal Division

```bash
echo "scale=2;20/3" | bc
```

Output:

```
6.66
```

### Power

```bash
echo "5^3" | bc
```

Output:

```
125
```

### Square Root

```bash
echo "sqrt(100)" | bc -l
```

Output:

```
10
```

## Production Use Cases

* Shell scripting.
* Financial calculations.
* Storage calculations.
* Automation scripts.

---

# Quick Summary

| Command    | Purpose                           |
| ---------- | --------------------------------- |
| `date`     | Display current date and time     |
| `uptime`   | Show system uptime                |
| `hostname` | Display system name               |
| `uname`    | Display kernel and OS information |
| `which`    | Locate executable commands        |
| `cal`      | Display calendar                  |
| `bc`       | Command-line calculator           |

---

# Real Production Example

When troubleshooting a Linux server, an administrator may run:

```bash
date
uptime
hostname
hostname -I
uname -a
which python3
cal
echo "500*1024" | bc
```

These commands help to:

* Check server time.
* Verify uptime.
* Identify the server.
* Check network IP.
* Display kernel information.
* Locate installed software.
* View the calendar.
* Perform quick calculations.

---

# Interview Questions

### Q1. Which command displays the current date and time?

```bash
date
```

### Q2. Which command checks server uptime?

```bash
uptime
```

### Q3. How do you display complete kernel information?

```bash
uname -a
```

### Q4. Which command locates an executable file?

```bash
which
```

### Q5. How do you display the system IP address?

```bash
hostname -I
```

### Q6. Which Linux command acts as a calculator?

```bash
bc
```

---

# Practice Commands

```bash
date
date +"%d-%m-%Y"
date +"%H:%M:%S"

uptime
uptime -p

hostname
hostname -I
hostname -f

uname
uname -a
uname -r
uname -m

which ls
which git
which python3

cal
cal 2026
cal 12 2026

echo "10+20" | bc
echo "50-15" | bc
echo "10*5" | bc
echo "scale=2;20/3" | bc
echo "5^3" | bc
echo "sqrt(100)" | bc -l
```
