# Linux Screen Command

## What is Screen?

`screen` is a terminal multiplexer that allows you to create, manage, and reconnect to multiple terminal sessions from a single SSH connection.

It is widely used by Linux administrators to keep long-running processes active even after disconnecting from the server.

---

# Why Use Screen?

Suppose you are connected to a remote server via SSH and running:

- Software installation
- Database backup
- Log monitoring
- File transfer
- Script execution

If your SSH connection drops, the process may stop.

Using `screen`, the process continues running in the background, and you can reconnect later.

---

# Features

✅ Multiple terminal sessions

✅ Detached and reattached sessions

✅ Long-running task management

✅ Multiple windows inside one session

✅ Shared terminal sessions

✅ Background execution

---

# Installation

## Ubuntu/Debian

```bash
sudo apt update
sudo apt install screen
```

---

## RHEL/CentOS/Rocky/AlmaLinux

```bash
sudo yum install screen
```

or

```bash
sudo dnf install screen
```

---

## Verify Installation

```bash
screen --version
```

Example:

```
Screen version 4.09.00
```

---

# Start Screen

```bash
screen
```

A new screen session starts.

Run commands normally.

Example:

```bash
top
```

or

```bash
ping google.com
```

---

# Start Named Session

Syntax:

```bash
screen -S session_name
```

Example:

```bash
screen -S backup
```

Create another:

```bash
screen -S monitoring
```

Named sessions are easier to identify.

---

# List Screen Sessions

```bash
screen -ls
```

Example:

```
There are screens on:

12345.backup
12350.monitoring

2 Sockets in /run/screen/S-user.
```

---

# Detach Screen Session

Keep session running in background.

Shortcut:

```
Ctrl + A then D
```

Output:

```
Detached from 12345.backup
```

Session continues running.

---

# Reattach Session

```bash
screen -r
```

If multiple sessions exist:

```bash
screen -r 12345
```

or

```bash
screen -r backup
```

---

# Create Multiple Windows

Inside screen:

```
Ctrl + A then C
```

Creates a new window.

---

# Switch Windows

Next window:

```
Ctrl + A then N
```

Previous window:

```
Ctrl + A then P
```

List windows:

```
Ctrl + A then "
```

---

# Rename Window

```
Ctrl + A then A
```

Enter new window name.

---

# Kill Current Window

Type:

```bash
exit
```

or

Press:

```
Ctrl + D
```

---

# Kill Screen Session

Inside session:

```bash
exit
```

or

```bash
logout
```

Session closes.

---

# Kill Detached Session

List sessions:

```bash
screen -ls
```

Kill:

```bash
screen -X -S backup quit
```

Or:

```bash
screen -S backup -X quit
```

---

# Force Reattach

If session is attached elsewhere:

```bash
screen -d -r
```

Detach old connection and reconnect.

---

# Start Command Directly

Example:

```bash
screen -S pingtest ping google.com
```

Starts command inside screen.

---

# Log Screen Output

Start logging:

```
Ctrl + A then H
```

Creates:

```
screenlog.0
```

Stop logging:

```
Ctrl + A then H
```

---

# Split Screen

Horizontal split:

```
Ctrl + A then S
```

Switch region:

```
Ctrl + A then Tab
```

Close split:

```
Ctrl + A then X
```

---

# Copy Mode

Enter copy mode:

```
Ctrl + A then Esc
```

Navigate using arrow keys.

Exit:

```
Esc
```

---

# Screen Configuration File

User configuration:

```
~/.screenrc
```

Global configuration:

```
/etc/screenrc
```

Example:

```
startup_message off
defscrollback 5000
hardstatus alwayslastline
```

---

# Production Examples

## Long Software Installation

Instead of:

```bash
sudo apt upgrade
```

Use:

```bash
screen -S update
sudo apt upgrade
```

Detach:

```
Ctrl+A D
```

Reconnect later:

```bash
screen -r update
```

---

## Database Backup

Start:

```bash
screen -S backup
```

Run:

```bash
mysqldump database > backup.sql
```

Detach safely.

---

## File Download

```bash
screen -S download
wget largefile.iso
```

Disconnect SSH safely.

---

## Log Monitoring

```bash
screen -S logs
tail -f /var/log/syslog
```

Detach and reconnect later.

---

## Server Monitoring

```bash
screen -S monitor
top
```

---

# Common Screen Commands

| Command | Purpose |
|----------|----------|
| screen | Start session |
| screen -S name | Named session |
| screen -ls | List sessions |
| screen -r | Reattach |
| screen -r id | Reattach specific session |
| screen -d -r | Force reattach |
| screen -S name -X quit | Kill session |
| exit | Close session |

---

# Keyboard Shortcuts

| Shortcut | Purpose |
|-----------|----------|
| Ctrl+A D | Detach session |
| Ctrl+A C | New window |
| Ctrl+A N | Next window |
| Ctrl+A P | Previous window |
| Ctrl+A " | Window list |
| Ctrl+A A | Rename window |
| Ctrl+A H | Start/stop logging |
| Ctrl+A S | Horizontal split |
| Ctrl+A Tab | Switch split |
| Ctrl+A X | Close split |
| Ctrl+A Esc | Copy mode |

---

# Difference Between Screen and nohup

| Feature | screen | nohup |
|----------|----------|----------|
| Interactive session | Yes | No |
| Reconnect later | Yes | No |
| Multiple windows | Yes | No |
| Background execution | Yes | Yes |
| Session management | Yes | No |
| Long-running jobs | Excellent | Basic |

---

# Difference Between Screen and tmux

| Feature | screen | tmux |
|----------|----------|----------|
| Older tool | Yes | No |
| Multiple panes | Limited | Advanced |
| Session management | Good | Better |
| Configuration | Simple | Flexible |
| Default on many servers | Yes | Growing popularity |

---

# Real Production Use Cases

## Application Deployment

```
screen -S deploy
```

Run deployment scripts safely.

---

## Kubernetes Logs

```
screen -S kube
kubectl logs -f pod-name
```

---

## Database Migration

```
screen -S migration
```

Run migration scripts without interruption.

---

## Backup Jobs

```
screen -S backup
tar -czf backup.tar.gz /data
```

---

## Large File Transfers

```
screen -S rsync
rsync -av source destination
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| screen | Start session |
| screen -S backup | Create named session |
| screen -ls | List sessions |
| screen -r | Reattach session |
| screen -r backup | Reattach named session |
| screen -d -r | Force reattach |
| screen -S backup -X quit | Kill session |
| exit | Close session |

---

# Interview Questions

## Q1. What is the Screen command?

Screen is a terminal multiplexer that allows users to create persistent terminal sessions that continue running after disconnection.

---

## Q2. Why is Screen used in production?

It keeps long-running tasks active during SSH disconnects and allows administrators to reconnect later.

---

## Q3. How do you detach a Screen session?

Press:

```
Ctrl + A
D
```

---

## Q4. How do you reconnect to a detached Screen session?

```bash
screen -r
```

or

```bash
screen -r session_name
```

---

## Q5. How do you list active Screen sessions?

```bash
screen -ls
```

---

## Q6. What is the difference between Screen and nohup?

- `screen` provides interactive, reconnectable terminal sessions.
- `nohup` simply keeps a command running after logout without interactive session management.

---