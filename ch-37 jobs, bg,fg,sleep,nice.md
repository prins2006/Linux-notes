# Linux Process Management (`bg`, `fg`, `nice`, `renice`, `jobs`, `nohup`)

## 1. Introduction

In Linux, every running program is called a **process**. Process management allows users to:

* Start processes.
* Pause processes.
* Run processes in the background.
* Bring them back to the foreground.
* Change CPU priority.
* Keep processes running after logout.

This is especially useful for Linux administrators and DevOps engineers when managing long-running tasks like backups, log analysis, software compilation, and script execution.

---

# Understanding Foreground and Background Processes

## Foreground Process

A foreground process occupies the terminal. You cannot use the terminal for another command until it finishes.

Example:

```bash
sleep 100
```

The terminal waits for 100 seconds.

Press:

```text
Ctrl + C
```

to terminate.

---

## Background Process

A background process runs without occupying the terminal.

Example:

```bash
sleep 100 &
```

Output:

```text
[1] 12345
```

Where:

* `[1]` = Job Number
* `12345` = Process ID (PID)

You can continue using the terminal.

---

# 2. `jobs` Command

## Purpose

Displays background and suspended jobs started from the current shell.

## Syntax

```bash
jobs
```

Example:

```bash
sleep 300 &
sleep 500 &
jobs
```

Output:

```text
[1]- Running sleep 300 &
[2]+ Running sleep 500 &
```

### Symbols

| Symbol | Meaning      |
| ------ | ------------ |
| +      | Current job  |
| -      | Previous job |

---

# 3. Suspending a Process

Suppose:

```bash
ping google.com
```

Output:

```text
64 bytes from ...
64 bytes from ...
```

Press:

```text
Ctrl + Z
```

Output:

```text
[1]+ Stopped ping google.com
```

The process is paused but not terminated.

Check:

```bash
jobs
```

Output:

```text
[1]+ Stopped ping google.com
```

---

# 4. `bg` Command

## Purpose

Runs a suspended job in the background.

## Syntax

```bash
bg
```

or

```bash
bg %1
```

Example:

Suspend:

```bash
ping google.com
```

Press:

```text
Ctrl + Z
```

Run:

```bash
bg
```

Output:

```text
[1]+ ping google.com &
```

Check:

```bash
jobs
```

Output:

```text
[1]+ Running ping google.com &
```

The process continues running while the terminal remains free.

---

# 5. `fg` Command

## Purpose

Brings a background process to the foreground.

## Syntax

```bash
fg
```

or

```bash
fg %1
```

Example:

Check:

```bash
jobs
```

Output:

```text
[1]+ Running ping google.com &
```

Run:

```bash
fg
```

Output:

```text
ping google.com
```

The process again occupies the terminal.

Stop:

```text
Ctrl + C
```

---

# Flow of `bg` and `fg`

```text
Start Process

      |

Foreground

      |

Ctrl + Z

      |

Suspended

      |

bg

      |

Background

      |

fg

      |

Foreground

      |

Ctrl + C

      |

Terminated
```

---

# 6. `nice` Command

## Purpose

Starts a process with a specific CPU priority.

Linux scheduler gives CPU time according to priority.

Lower nice value = Higher CPU priority.

Higher nice value = Lower CPU priority.

---

# Nice Value Table

| Nice Value | Priority |
| ---------- | -------- |
| -20        | Highest  |
| 0          | Default  |
| 19         | Lowest   |

---

# Syntax

```bash
nice -n value command
```

---

## Default Priority

```bash
nice ls
```

Default:

```text
0
```

---

## Low Priority Backup

```bash
nice -n 10 tar -czf backup.tar.gz /home
```

The backup uses CPU only when available.

---

## High Priority

Root user:

```bash
sudo nice -n -10 command
```

---

# Why Use `nice`?

Suppose:

* Database server running.
* Users accessing application.
* Large backup needed.

Without nice:

```bash
tar -czf backup.tar.gz /
```

CPU usage may increase significantly.

Better:

```bash
nice -n 15 tar -czf backup.tar.gz /
```

Application performance remains stable.

---

# 7. `renice`

## Purpose

Change priority of an existing process.

Syntax:

```bash
renice value PID
```

Example:

Find PID:

```bash
ps -ef | grep java
```

Output:

```text
1234 java
```

Change priority:

```bash
renice 10 1234
```

Output:

```text
1234: old priority 0, new priority 10
```

---

# Check Nice Value

```bash
ps -l
```

Example:

```text
PID NI COMMAND

1234 0 java

5678 10 tar
```

---

# 8. `nohup`

## Purpose

Keeps a process running after logout.

Without nohup:

```bash
python app.py
```

Logout:

Process stops.

With nohup:

```bash
nohup python app.py &
```

Output:

```text
appending output to nohup.out
```

Logout:

Process continues.

---

# Production Example

Start application:

```bash
nohup java -jar app.jar &
```

Check:

```bash
ps -ef | grep java
```

---

# Difference Between `bg`, `fg`, `nice`, `renice`, and `nohup`

| Command | Purpose                            |
| ------- | ---------------------------------- |
| bg      | Resume suspended job in background |
| fg      | Bring background job to foreground |
| nice    | Start process with custom priority |
| renice  | Change running process priority    |
| nohup   | Continue process after logout      |

---

# Real Production Scenario

Suppose a server administrator needs to:

## Backup Server

```bash
nice -n 15 tar -czf backup.tar.gz /data &
```

---

## Check Job

```bash
jobs
```

---

## Bring to Foreground

```bash
fg
```

---

## Suspend

```text
Ctrl + Z
```

---

## Continue in Background

```bash
bg
```

---

## Logout Without Stopping

```bash
nohup tar -czf backup.tar.gz /data &
```

---

# Interview Questions

## Q1. Difference between foreground and background process?

Foreground occupies the terminal.

Background runs independently.

---

## Q2. What does Ctrl+Z do?

Suspends the running process.

---

## Q3. What does bg do?

Runs a suspended process in the background.

---

## Q4. What does fg do?

Brings a background process to the foreground.

---

## Q5. What is the nice value range?

```text
-20 to 19
```

---

## Q6. Difference between nice and renice?

| nice                  | renice               |
| --------------------- | -------------------- |
| Before process starts | After process starts |
| No PID needed         | PID required         |

---

## Q7. Why use nohup?

To keep a process running after the user logs out.

---

# Production Best Practices

* Use `bg` for long-running interactive commands.
* Use `fg` when user interaction is needed.
* Use `nice` for CPU-intensive tasks like backups and compression.
* Use `renice` to adjust priorities of running applications.
* Use `nohup` for long-running services and scripts.
* Use `jobs`, `ps`, and `top` to monitor process status.

---

# Quick Revision

| Command  | Example              | Purpose                   |
| -------- | -------------------- | ------------------------- |
| `Ctrl+Z` | Running process      | Suspend                   |
| `bg`     | `bg %1`              | Background execution      |
| `fg`     | `fg %1`              | Foreground execution      |
| `jobs`   | `jobs`               | List jobs                 |
| `nice`   | `nice -n 10 command` | Start with lower priority |
| `renice` | `renice 10 PID`      | Change running priority   |
| `nohup`  | `nohup app.sh &`     | Continue after logout     |

---

# Summary

Process management in Linux allows efficient control of running applications. `bg` and `fg` manage job execution, `nice` and `renice` control CPU scheduling priority, `jobs` tracks shell jobs, and `nohup` ensures processes continue after logout. These commands are commonly used in production environments for backups, maintenance, application deployment, and server administration.
