# `systemd-analyze` Command (Production-Level Guide)

**Level:** Beginner → Advanced → Production
**Operating Systems:** RHEL 7/8/9, Rocky Linux 8/9, AlmaLinux 8/9, Ubuntu 16.04+, Debian, Fedora

---

# What is `systemd-analyze`?

`systemd-analyze` is a Linux command used to **measure, analyze, and troubleshoot the system boot process** on systems that use **systemd**.

It provides information such as:

* Total boot time
* Kernel boot time
* Userspace boot time
* Time taken by each service
* Critical boot path
* Slow services
* Boot performance statistics
* Systemd configuration verification

It is one of the most important commands for **Linux System Administrators**, **DevOps Engineers**, and **Production Support Engineers** when diagnosing slow boot issues.

---

# Syntax

```bash
systemd-analyze [OPTIONS] [COMMAND]
```

---

# How Boot Time is Calculated

The total boot time is divided into three major parts:

```text
Power ON
      │
      ▼
Firmware (BIOS / UEFI)
      │
      ▼
Bootloader (GRUB2)
      │
      ▼
Linux Kernel
      │
      ▼
systemd Userspace
      │
      ▼
Login Prompt
```

Formula:

```text
Total Boot Time

=

Firmware Time

+

Bootloader Time

+

Kernel Time

+

Userspace Time
```

---

# 1. Display Overall Boot Time

Command:

```bash
systemd-analyze
```

Example Output:

```text
Startup finished in 2.345s (firmware)
+ 0.550s (loader)
+ 1.842s (kernel)
+ 4.713s (userspace)

= 9.450s
```

Explanation:

| Stage     | Meaning                                  |
| --------- | ---------------------------------------- |
| Firmware  | Time spent in BIOS or UEFI               |
| Loader    | Time spent in GRUB2                      |
| Kernel    | Linux kernel initialization              |
| Userspace | Time spent starting systemd and services |
| Total     | Total boot time                          |

---

# Real Production Example

```text
Firmware : 3.1 seconds

GRUB      : 0.6 seconds

Kernel    : 1.4 seconds

Userspace : 7.2 seconds

Total     : 12.3 seconds
```

The server takes **12.3 seconds** to become operational.

---

# 2. Display Boot Performance Graph

Command:

```bash
systemd-analyze blame
```

Example Output:

```text
5.234s NetworkManager.service

3.950s firewalld.service

2.800s httpd.service

2.100s mariadb.service

1.450s sshd.service
```

Meaning:

The services are sorted by startup time.

The slowest service appears first.

---

# Production Example

Suppose booting takes **45 seconds**.

Running:

```bash
systemd-analyze blame
```

Shows:

```text
28.560s docker.service

7.100s mariadb.service

4.900s httpd.service

2.400s firewalld.service
```

Immediately you know that **Docker** is the biggest contributor to the slow boot.

---

# 3. Display Critical Boot Chain

Command:

```bash
systemd-analyze critical-chain
```

Example Output:

```text
graphical.target

└─multi-user.target

 └─httpd.service

  └─network-online.target

   └─NetworkManager.service
```

Meaning:

This command shows the **dependency chain** that determines the boot completion time.

If one service in the chain is delayed, every dependent service waits.

---

# Production Example

```text
NetworkManager

↓

network-online.target

↓

mariadb.service

↓

httpd.service

↓

Application Ready
```

If **NetworkManager** takes 10 seconds, the database and web server also start later.

---

# 4. Display SVG Boot Graph

Command:

```bash
systemd-analyze plot > boot.svg
```

This creates a graphical timeline of the boot process.

Example:

```bash
systemd-analyze plot > boot.svg
```

Open:

```text
boot.svg
```

The graph displays:

* Service start times
* Parallel startup
* Waiting periods
* CPU usage timeline

Useful for production performance analysis.

---

# 5. Verify systemd Unit Files

Command:

```bash
systemd-analyze verify /etc/systemd/system/httpd.service
```

Purpose:

Checks a unit file for:

* Syntax errors
* Missing directives
* Dependency problems
* Invalid configuration

Example:

```text
No issues found.
```

Or:

```text
Unknown directive

Missing dependency

Invalid option
```

---

# 6. Show Unit Dependencies

Command:

```bash
systemctl list-dependencies multi-user.target
```

Although not part of `systemd-analyze`, it is commonly used with it to understand service relationships.

---

# 7. Show Security Analysis

Command:

```bash
systemd-analyze security sshd.service
```

Example Output:

```text
Overall exposure level: 6.5 MEDIUM
```

It evaluates service security based on sandboxing features such as:

* PrivateTmp
* ProtectSystem
* ProtectHome
* CapabilityBoundingSet
* NoNewPrivileges

Useful for hardening production services.

---

# Common `systemd-analyze` Commands

| Command                               | Description                      |
| ------------------------------------- | -------------------------------- |
| `systemd-analyze`                     | Show total boot time             |
| `systemd-analyze blame`               | List services by startup time    |
| `systemd-analyze critical-chain`      | Show boot dependency chain       |
| `systemd-analyze plot > boot.svg`     | Generate graphical boot timeline |
| `systemd-analyze verify file.service` | Validate a unit file             |
| `systemd-analyze security service`    | Analyze service security         |

---

# Production Use Case

A Rocky Linux 9 web server reports slow boot times.

Administrator runs:

```bash
systemd-analyze
```

Output:

```text
Startup finished in 18.3s
```

Then:

```bash
systemd-analyze blame
```

Output:

```text
9.500s docker.service

3.100s mariadb.service

2.400s httpd.service
```

Next:

```bash
systemd-analyze critical-chain
```

The output reveals that `httpd.service` waits for `mariadb.service`, which waits for `docker.service`.

The administrator investigates Docker configuration, reducing its startup time and improving the total boot time from **18.3 seconds** to **9.8 seconds**.

---

# Best Practices

* Run `systemd-analyze` after installing new software or services.
* Use `systemd-analyze blame` to identify slow-starting services.
* Review `critical-chain` to understand startup dependencies.
* Validate custom unit files with `verify` before enabling them.
* Periodically check important services with `systemd-analyze security` to improve system hardening.
* Combine `systemd-analyze` with `journalctl -b` when troubleshooting boot problems.

---

# Interview Questions

### 1. What is `systemd-analyze`?

A command-line tool used to analyze boot performance, service startup times, dependencies, and systemd unit files.

### 2. Which command shows total boot time?

```bash
systemd-analyze
```

### 3. Which command shows the slowest services?

```bash
systemd-analyze blame
```

### 4. Which command displays boot dependencies?

```bash
systemd-analyze critical-chain
```

### 5. Which command creates a graphical boot timeline?

```bash
systemd-analyze plot > boot.svg
```

### 6. Which command validates a systemd unit file?

```bash
systemd-analyze verify /path/to/unit.service
```

### 7. Which command analyzes the security configuration of a service?

```bash
systemd-analyze security sshd.service
```
