# `kill` Command in Linux

## Introduction

The `kill` command is used to **terminate (stop) a running process** in Linux.

Every running process has a unique **Process ID (PID)**. The `kill` command sends a signal to that process to perform an action, such as:

* Terminate the process.
* Forcefully stop the process.
* Pause or resume a process.
* Reload configuration.

> **Note:** Despite its name, `kill` does not always "kill" a process. It sends a signal to the process.

---

# Syntax

```bash
kill [SIGNAL] PID
```

Example:

```bash
kill 1234
```

Sends the default termination signal to process 1234.

---

# Find the Process PID

Before using `kill`, find the Process ID.

## Using ps

```bash
ps -ef
```

Find a specific process:

```bash
ps -ef | grep nginx
```

Example Output:

```text
root    1500     1  nginx
```

PID = 1500

---

# Default kill Command

```bash
kill PID
```

Example:

```bash
kill 1500
```

Default Signal:

```text
SIGTERM (15)
```

The process is requested to terminate gracefully.

---

# Linux Signals

Each signal has a number and a purpose.

## Display Available Signals

```bash
kill -l
```

Example Output:

```text
1) SIGHUP
2) SIGINT
9) SIGKILL
15) SIGTERM
18) SIGCONT
19) SIGSTOP
```

---

# Common Signals

| Signal  | Number | Purpose              |
| ------- | ------ | -------------------- |
| SIGHUP  | 1      | Reload configuration |
| SIGINT  | 2      | Interrupt process    |
| SIGKILL | 9      | Force kill           |
| SIGTERM | 15     | Graceful termination |
| SIGSTOP | 19     | Pause process        |
| SIGCONT | 18     | Resume process       |

---

# SIGTERM (15)

## Graceful Termination

```bash
kill -15 PID
```

or

```bash
kill PID
```

Example:

```bash
kill 1500
```

### Production Use

Preferred method for stopping applications.

Allows:

* Save data.
* Close files.
* Release resources.
* Shut down cleanly.

---

# SIGKILL (9)

## Force Kill

```bash
kill -9 PID
```

Example:

```bash
kill -9 1500
```

The process cannot ignore this signal.

### Production Use

Use only when:

* Process hangs.
* Application freezes.
* SIGTERM fails.

---

# SIGHUP (1)

Reload configuration.

Example:

```bash
kill -1 PID
```

Production Example:

Reload a web server after configuration updates.

---

# SIGSTOP (19)

Pause a process.

```bash
kill -19 PID
```

Example:

```bash
kill -19 1500
```

---

# SIGCONT (18)

Resume a paused process.

```bash
kill -18 PID
```

Example:

```bash
kill -18 1500
```

---

# Kill Multiple Processes

```bash
kill PID1 PID2 PID3
```

Example:

```bash
kill 1200 1300 1400
```

---

# Kill by Process Name

## pkill

```bash
pkill nginx
```

Kills all nginx processes.

---

## killall

```bash
killall nginx
```

Kills all matching processes.

---

# Check Whether Process is Running

```bash
ps -ef | grep nginx
```

If no output:

The process has stopped.

---

# Production Workflow

## Step 1

Find process:

```bash
ps -ef | grep java
```

Example:

```text
java 3456
```

---

## Step 2

Terminate gracefully:

```bash
kill 3456
```

---

## Step 3

Verify:

```bash
ps -ef | grep java
```

---

## Step 4

If still running:

```bash
kill -9 3456
```

---

# Real Production Scenario

Suppose a Java application consumes 100% CPU.

Check:

```bash
top
```

Find:

```text
PID 3456
CPU 99%
```

Terminate:

```bash
kill 3456
```

If unsuccessful:

```bash
kill -9 3456
```

Restart service:

```bash
sudo systemctl restart app
```

Verify:

```bash
systemctl status app
```

---

# Difference Between kill, pkill, and killall

| Command | Uses                            |
| ------- | ------------------------------- |
| kill    | Kill by PID                     |
| pkill   | Kill by process name            |
| killall | Kill all matching process names |

Examples:

```bash
kill 1500

pkill nginx

killall nginx
```

---

# Important Commands

## Display Signals

```bash
kill -l
```

---

## Graceful Stop

```bash
kill PID
```

---

## Force Kill

```bash
kill -9 PID
```

---

## Pause Process

```bash
kill -19 PID
```

---

## Resume Process

```bash
kill -18 PID
```

---

## Kill by Name

```bash
pkill nginx
```

---

## Kill All Matching Processes

```bash
killall nginx
```

---

# Interview Questions

## What is the kill command?

It sends signals to running processes.

---

## What is the default signal?

```text
SIGTERM (15)
```

---

## Which signal forcefully terminates a process?

```text
SIGKILL (9)
```

---

## Which signal should be used first in production?

```text
SIGTERM (15)
```

Because it allows a graceful shutdown.

---

## How do you display available signals?

```bash
kill -l
```

---

## Difference between SIGTERM and SIGKILL?

| SIGTERM              | SIGKILL                 |
| -------------------- | ----------------------- |
| Graceful shutdown    | Immediate termination   |
| Process can clean up | Process cannot clean up |
| Recommended first    | Last option             |

---

# Daily Practice Commands

```bash
ps -ef

ps -ef | grep nginx

kill 1500

kill -15 1500

kill -9 1500

kill -1 1500

kill -19 1500

kill -18 1500

kill -l

pkill nginx

killall nginx
```

---

# Summary

The `kill` command is an essential Linux process management tool.

## Key Points

* Sends signals to processes.
* Default signal is `SIGTERM (15)`.
* `SIGKILL (9)` forcefully stops a process.
* Use `SIGTERM` before `SIGKILL` in production.
* `pkill` and `killall` work with process names.
* Commonly used with `ps`, `top`, and `systemctl`.

## Production Best Practice

```text
1. Find PID.
2. Use kill (SIGTERM).
3. Verify process stopped.
4. If necessary, use kill -9.
5. Restart the service if required.
```
