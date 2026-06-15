# Talking to Users in Linux (`users`, `wall`, `write`)

## Introduction

Linux is a multi-user operating system where multiple users can log in and work simultaneously. System administrators can communicate with logged-in users using commands like:

* `users`
* `who`
* `w`
* `wall`
* `write`

These commands are useful for system maintenance, server announcements, and user communication.

---

# 1. `users` Command

## What is `users`?

The `users` command displays the usernames of users currently logged into the system.

## Syntax

```bash
users
```

## Example

```bash
$ users
john admin root
```

### Output Meaning

* `john` is logged in.
* `admin` is logged in.
* `root` is logged in.

If a user has multiple sessions, their name appears multiple times.

Example:

```bash
$ users
john john admin
```

John has two active sessions.

---

# Production Use Case

Before system maintenance, an administrator checks active users.

```bash
users
```

Output:

```
john alice dev1
```

The administrator knows which users are online.

---

# 2. `who` Command

Displays detailed login information.

## Syntax

```bash
who
```

Example:

```bash
$ who

john pts/0 2026-06-14 09:00
alice pts/1 2026-06-14 09:30
```

### Columns

| Field      | Description     |
| ---------- | --------------- |
| Username   | Logged in user  |
| Terminal   | Session         |
| Login Time | Login timestamp |

---

# 3. `w` Command

Shows active users and their activities.

## Syntax

```bash
w
```

Example:

```bash
$ w

USER   TTY  FROM        LOGIN@ IDLE JCPU PCPU WHAT
john   pts0 10.0.0.5    09:00 1:00 0.05 0.01 vim file.txt
alice  pts1 10.0.0.10   09:30 0.00 0.02 0.01 top
```

Useful for monitoring user activity.

---

# 4. `wall` Command

## What is wall?

`wall` means:

**Write All**

It sends a message to every logged-in user.

---

## Syntax

```bash
wall
```

Type the message:

```
System maintenance starts in 10 minutes.
```

Press:

```
Ctrl + D
```

All logged-in users receive the message.

---

## Send Message from File

Create a file:

```bash
echo "Server will reboot in 5 minutes." > message.txt
```

Send:

```bash
wall < message.txt
```

---

# Production Example

Before rebooting:

```bash
wall
```

Message:

```
Attention:
The server will restart in 10 minutes.
Please save your work.
```

Press:

```
Ctrl + D
```

Every logged-in user sees the message.

---

# 5. `write` Command

## What is write?

The `write` command sends a private message to a specific logged-in user.

---

## Syntax

```bash
write username
```

Example:

```bash
write john
```

Type:

```
Hello John.
Please log out after completing your work.
```

Press:

```
Ctrl + D
```

---

# Private Chat Example

Administrator:

```bash
write alice
```

Message:

```
Please restart your application.
```

Press:

```
Ctrl + D
```

Alice receives only this message.

---

# Find User Terminal

Sometimes you need the terminal name.

Use:

```bash
who
```

Example:

```
john pts/0
alice pts/1
```

Send message:

```bash
write john pts/0
```

---

# Difference Between wall and write

| Feature           | wall                 | write                  |
| ----------------- | -------------------- | ---------------------- |
| Send to all users | Yes                  | No                     |
| Send to one user  | No                   | Yes                    |
| Private           | No                   | Yes                    |
| Production use    | Server announcements | Personal communication |

---

# Production Scenario

## Step 1: Check Logged-in Users

```bash
users
```

Output:

```
john alice dev1
```

---

## Step 2: Get Detailed Information

```bash
who
```

---

## Step 3: Check User Activity

```bash
w
```

---

## Step 4: Broadcast Maintenance Message

```bash
wall
```

Message:

```
System maintenance starts in 15 minutes.
Please save your work.
```

Press:

```
Ctrl + D
```

---

## Step 5: Send Private Message

```bash
write john
```

Message:

```
Please stop your application deployment.
```

Press:

```
Ctrl + D
```

---

# Related Commands

## Current User

```bash
whoami
```

---

## Login Name

```bash
logname
```

---

## Terminal Name

```bash
tty
```

Example:

```bash
$ tty

/dev/pts/0
```

---

## Last Logged-in Users

```bash
last
```

---

## Current Sessions

```bash
who
```

---

# Security Notes

## wall

* Usually available to privileged users.
* Used for maintenance notifications.

## write

* User must be logged in.
* Some users can disable messages.

Disable messages:

```bash
mesg n
```

Enable messages:

```bash
mesg y
```

Check status:

```bash
mesg
```

---

# Interview Questions

## What does the `users` command do?

Displays currently logged-in users.

---

## What does `wall` stand for?

Write All.

---

## What does the `write` command do?

Sends a private message to a logged-in user.

---

## Which command shows detailed login information?

```bash
who
```

---

## Which command shows user activity?

```bash
w
```

---

## How do you disable incoming messages?

```bash
mesg n
```

---

# Quick Revision

| Task                 | Command          |
| -------------------- | ---------------- |
| Show logged-in users | `users`          |
| Show login details   | `who`            |
| Show user activity   | `w`              |
| Broadcast message    | `wall`           |
| Private message      | `write username` |
| Show current user    | `whoami`         |
| Show terminal        | `tty`            |
| Show login name      | `logname`        |
| Show login history   | `last`           |
| Disable messages     | `mesg n`         |
| Enable messages      | `mesg y`         |

---

# Real Production Example

A system administrator plans a server reboot.

### Check active users

```bash
users
```

### Check detailed sessions

```bash
who
```

### Check active work

```bash
w
```

### Notify everyone

```bash
wall
```

```
NOTICE:
Server maintenance will begin in 10 minutes.
Please save your work and log out safely.
```

Press:

```
Ctrl + D
```

### Send a private message to a developer

```bash
write john
```

```
Please stop the deployment process before maintenance.
```

Press:

```
Ctrl + D
```

---

# Summary

* `users` displays logged-in users.
* `who` provides detailed login information.
* `w` shows what users are currently doing.
* `wall` broadcasts messages to all logged-in users.
* `write` sends private messages to individual users.
* `mesg` controls whether a user can receive messages.
* These commands are commonly used by Linux administrators for communication and maintenance in production environments.
