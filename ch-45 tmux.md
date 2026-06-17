# Linux tmux Command

## What is tmux?

**tmux (Terminal Multiplexer)** is a command-line tool that allows you to create and manage multiple terminal sessions within a single window.

It lets you:
- Run multiple terminals in one SSH session.
- Detach and reattach sessions.
- Split the terminal into panes.
- Keep processes running after disconnecting.
- Share terminal sessions with other users.

tmux is commonly used by Linux System Administrators, DevOps Engineers, and Developers.

---

# Why Use tmux?

Suppose you are connected to a production server via SSH and need to:

- Monitor logs
- Deploy an application
- Run backups
- Check CPU usage

Instead of opening multiple SSH connections, you can use tmux.

Even if the SSH connection drops, tmux keeps your work running.

---

# Features

✅ Multiple sessions

✅ Multiple windows

✅ Split terminal panes

✅ Detach and reattach sessions

✅ Session persistence

✅ Keyboard shortcuts

✅ Shared sessions

---

# Installation

## Ubuntu/Debian

```bash
sudo apt update
sudo apt install tmux
```

---

## RHEL/CentOS/Rocky Linux

```bash
sudo yum install tmux
```

or

```bash
sudo dnf install tmux
```

---

## Check Version

```bash
tmux -V
```

Example:

```
tmux 3.4
```

---

# Start tmux

Simply type:

```bash
tmux
```

A new tmux session starts.

---

# Create Named Session

Syntax:

```bash
tmux new -s session_name
```

Example:

```bash
tmux new -s backup
```

Another example:

```bash
tmux new -s monitoring
```

---

# List Sessions

```bash
tmux ls
```

or

```bash
tmux list-sessions
```

Example:

```
backup: 1 windows
monitoring: 1 windows
```

---

# Detach Session

Keep the session running in the background.

Shortcut:

```
Ctrl+B then D
```

Output:

```
detached
```

---

# Reattach Session

```bash
tmux attach
```

or

```bash
tmux attach -t backup
```

---

# Kill Session

```bash
tmux kill-session -t backup
```

Kill all sessions:

```bash
tmux kill-server
```

---

# Create New Window

Shortcut:

```
Ctrl+B then C
```

New terminal window opens.

---

# Switch Windows

Next window:

```
Ctrl+B then N
```

Previous window:

```
Ctrl+B then P
```

List windows:

```
Ctrl+B then W
```

Go to window 0:

```
Ctrl+B then 0
```

Go to window 1:

```
Ctrl+B then 1
```

---

# Rename Window

Shortcut:

```
Ctrl+B then ,
```

Enter new window name.

---

# Split Terminal Horizontally

Shortcut:

```
Ctrl+B then "
```

Example:

```
--------------------
|                  |
--------------------
|                  |
--------------------
```

---

# Split Terminal Vertically

Shortcut:

```
Ctrl+B then %
```

Example:

```
----------|---------
          |
----------|---------
```

---

# Move Between Panes

Shortcut:

```
Ctrl+B then Arrow Keys
```

Example:

```
← ↑ ↓ →
```

---

# Resize Panes

Hold:

```
Ctrl+B
```

Then:

```
Alt + Arrow Keys
```

---

# Close Pane

Type:

```bash
exit
```

or

Press:

```
Ctrl+D
```

---

# Rename Session

```bash
tmux rename-session -t backup production
```

---

# Capture Pane Output

Enter copy mode:

```
Ctrl+B then [
```

Navigate with arrow keys.

Exit:

```
q
```

---

# Session Information

Display current session:

```bash
tmux display-message
```

---

# Configuration File

User configuration:

```
~/.tmux.conf
```

Global configuration:

```
/etc/tmux.conf
```

Example:

```
set -g mouse on
set -g history-limit 5000
```

---

# Production Examples

## Application Deployment

Create session:

```bash
tmux new -s deploy
```

Run:

```bash
git pull
systemctl restart app
```

Detach safely.

---

## Monitor Logs

```bash
tmux new -s logs
tail -f /var/log/syslog
```

Split another pane:

```
Ctrl+B %
```

Run:

```bash
top
```

Monitor logs and CPU simultaneously.

---

## Database Backup

```bash
tmux new -s backup
```

Run:

```bash
mysqldump database > backup.sql
```

Detach.

Reconnect later.

---

## Kubernetes

Pane 1:

```bash
kubectl get pods -w
```

Pane 2:

```bash
kubectl logs -f pod-name
```

Pane 3:

```bash
top
```

---

## File Transfer

```bash
tmux new -s transfer
rsync -av source destination
```

Disconnect SSH safely.

---

# Useful tmux Commands

| Command | Purpose |
|----------|----------|
| tmux | Start session |
| tmux new -s backup | Create named session |
| tmux ls | List sessions |
| tmux attach | Attach session |
| tmux attach -t backup | Attach named session |
| tmux kill-session -t backup | Kill session |
| tmux kill-server | Kill all sessions |
| tmux rename-session | Rename session |

---

# Important Shortcuts

| Shortcut | Purpose |
|-----------|----------|
| Ctrl+B D | Detach session |
| Ctrl+B C | New window |
| Ctrl+B N | Next window |
| Ctrl+B P | Previous window |
| Ctrl+B W | List windows |
| Ctrl+B % | Vertical split |
| Ctrl+B " | Horizontal split |
| Ctrl+B Arrow | Move panes |
| Ctrl+B [ | Copy mode |
| Ctrl+B , | Rename window |

---

# tmux vs Screen

| Feature | tmux | screen |
|----------|----------|----------|
| Multiple panes | Yes | Limited |
| Session management | Excellent | Good |
| Window management | Advanced | Basic |
| Mouse support | Yes | Limited |
| Configuration | Flexible | Simple |
| Modern development | Preferred | Older |

---

# tmux vs nohup

| Feature | tmux | nohup |
|----------|----------|----------|
| Interactive | Yes | No |
| Reconnect session | Yes | No |
| Split windows | Yes | No |
| Background jobs | Yes | Yes |
| Session management | Yes | No |

---

# Real Production Use Cases

## DevOps Monitoring

Pane 1:

```bash
top
```

Pane 2:

```bash
journalctl -f
```

Pane 3:

```bash
kubectl get pods -w
```

---

## CI/CD Deployment

```bash
tmux new -s deployment
```

Run deployment pipeline.

---

## Server Maintenance

Run updates:

```bash
sudo apt update
sudo apt upgrade
```

Detach and reconnect later.

---

## Long-running Scripts

```bash
python3 backup.py
```

Keep running after SSH disconnect.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| tmux | Start session |
| tmux new -s name | Create session |
| tmux ls | List sessions |
| tmux attach | Attach session |
| tmux attach -t name | Attach named session |
| tmux kill-session -t name | Kill session |
| tmux kill-server | Kill all sessions |

---

# Interview Questions

## Q1. What is tmux?

tmux is a terminal multiplexer that allows multiple terminal sessions, windows, and panes within a single terminal.

---

## Q2. Why is tmux used in production?

It allows long-running tasks to continue after SSH disconnects and enables efficient management of multiple terminal sessions.

---

## Q3. How do you detach a tmux session?

Press:

```
Ctrl+B
D
```

---

## Q4. How do you list active tmux sessions?

```bash
tmux ls
```

---

## Q5. How do you reconnect to a tmux session?

```bash
tmux attach -t session_name
```

Example:

```bash
tmux attach -t backup
```

---

## Q6. What is the default tmux command prefix?

```
Ctrl+B
```

Most tmux keyboard shortcuts begin with this prefix.

---

## Q7. What are the main advantages of tmux over screen?

- Better pane management.
- More flexible configuration.
- Improved window and session handling.
- Mouse support.
- Widely used in modern DevOps and cloud environments.

---