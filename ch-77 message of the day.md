# Customize Message of the Day (MOTD)

**Level:** Beginner → Advanced → Production
**Applies To:** RHEL 7/8/9, Rocky Linux 8/9, AlmaLinux, Ubuntu, Debian, Fedora

---

# Table of Contents

1. Introduction
2. What is MOTD?
3. How MOTD Works
4. MOTD Files
5. Static MOTD
6. Dynamic MOTD
7. `/etc/motd` vs `/etc/issue`
8. `/etc/profile.d` and Login Messages
9. Production Example
10. Common Commands
11. Troubleshooting
12. Best Practices
13. Interview Questions

---

# 1. Introduction

**MOTD (Message of the Day)** is a text message displayed to users **after they successfully log in** to a Linux system.

System administrators use MOTD to display:

* Welcome messages
* Company information
* Security warnings
* Maintenance schedules
* Server information
* Login policies
* Support contact details

MOTD is commonly used on production Linux servers to inform users about the system.

---

# 2. What is MOTD?

When a user logs in using SSH or a terminal, Linux displays the MOTD before showing the shell prompt.

Example:

```text
Last login: Mon Jun 29 10:00:15 2026 from 192.168.1.25

*******************************************
 Welcome to Production Server
 Company : ABC Technologies
 Environment : Production
 Authorized Users Only
*******************************************

prins@server:~$
```

---

# 3. How MOTD Works

Login flow:

```text
User Login
      │
      ▼
Authentication
      │
      ▼
Display Last Login
      │
      ▼
Read /etc/motd
      │
      ▼
Display Message
      │
      ▼
Shell Prompt
```

---

# 4. MOTD Files

| File              | Purpose                                       |
| ----------------- | --------------------------------------------- |
| `/etc/motd`       | Static Message of the Day                     |
| `/etc/issue`      | Message shown before local login              |
| `/etc/issue.net`  | Message shown before SSH login (when enabled) |
| `/etc/profile`    | Runs commands for login shells                |
| `/etc/profile.d/` | Custom login scripts                          |

---

# 5. Static MOTD

Edit the MOTD file:

```bash
sudo vi /etc/motd
```

Example:

```text
*************************************************

 Welcome to Rocky Linux 9

 Production Server

 Hostname : web01

 Environment : Production

 Managed by Linux Team

 Unauthorized access is prohibited.

*************************************************
```

Save the file.

Every user logging in will see this message.

---

# 6. Dynamic MOTD

Instead of a fixed message, Linux can display real-time system information.

Example script:

```bash
sudo vi /etc/profile.d/server-info.sh
```

Contents:

```bash
#!/bin/bash

echo "=========================================="
echo " Welcome to $(hostname)"
echo " Date       : $(date)"
echo " User       : $(whoami)"
echo " Uptime     : $(uptime -p)"
echo " IP Address : $(hostname -I)"
echo " Kernel     : $(uname -r)"
echo "=========================================="
```

Make it executable:

```bash
sudo chmod +x /etc/profile.d/server-info.sh
```

Next login output:

```text
==========================================
Welcome to web01
Date       : Mon Jun 29 16:45:21 IST 2026
User       : prins
Uptime     : up 12 hours
IP Address : 192.168.1.100
Kernel     : 5.14.0-570.el9.x86_64
==========================================
```

---

# 7. `/etc/motd` vs `/etc/issue`

| Feature        | `/etc/motd`            | `/etc/issue`        |
| -------------- | ---------------------- | ------------------- |
| Display Time   | After successful login | Before login prompt |
| Requires Login | Yes                    | No                  |
| SSH Users      | Yes                    | Usually No          |
| Local Console  | Yes                    | Yes                 |
| Purpose        | Welcome information    | Login banner        |

---

# 8. `/etc/profile.d` Login Messages

Scripts stored in:

```text
/etc/profile.d/
```

run automatically for login shells.

Example:

```bash
sudo vi /etc/profile.d/welcome.sh
```

```bash
#!/bin/bash

echo "Welcome to Production Server"
echo "Today's Date : $(date)"
```

Make executable:

```bash
chmod +x /etc/profile.d/welcome.sh
```

---

# 9. Production Example

A company has a production web server.

Every administrator connecting through SSH sees:

```text
*****************************************************

Company : ABC Technologies

Server : WEB-01

Environment : Production

Monitoring : Enabled

Backup : Daily 02:00 AM

Support : linux-admin@example.com

WARNING:
Unauthorized access is prohibited.
All activities are monitored and logged.

*****************************************************
```

This helps administrators immediately identify the server and important operational details.

---

# 10. Common Commands

Create MOTD:

```bash
sudo vi /etc/motd
```

Display MOTD:

```bash
cat /etc/motd
```

Clear MOTD:

```bash
sudo truncate -s 0 /etc/motd
```

Create login script:

```bash
sudo vi /etc/profile.d/info.sh
```

Make executable:

```bash
chmod +x /etc/profile.d/info.sh
```

List profile scripts:

```bash
ls -l /etc/profile.d/
```

---

# 11. Troubleshooting

### MOTD Not Displayed

Check:

```bash
cat /etc/motd
```

Verify permissions:

```bash
ls -l /etc/motd
```

Expected:

```text
-rw-r--r-- root root
```

Check if profile scripts execute:

```bash
ls /etc/profile.d/
```

Reconnect with SSH to test.

---

# 12. Best Practices

* Display only useful information.
* Avoid exposing sensitive data such as passwords or internal secrets.
* Include company name and environment (Development, Testing, Production).
* Add an authorized-use warning for security and compliance.
* Use dynamic scripts only when necessary to avoid slowing login.
* Keep the message concise and easy to read.

---

# 13. Interview Questions

### 1. What is MOTD?

MOTD (Message of the Day) is a message displayed after a user successfully logs into a Linux system.

### 2. Which file stores the static MOTD?

```text
/etc/motd
```

### 3. Which directory is commonly used for dynamic login messages?

```text
/etc/profile.d/
```

### 4. What is the difference between `/etc/motd` and `/etc/issue`?

* `/etc/motd` is displayed **after** a successful login.
* `/etc/issue` is displayed **before** the login prompt.

### 5. How do you display the current MOTD?

```bash
cat /etc/motd
```

### 6. How do you make a login script executable?

```bash
chmod +x /etc/profile.d/script.sh
```

### 7. Why is MOTD useful in production?

It provides administrators with important information such as server identity, environment, maintenance notices, and security warnings immediately after login.
