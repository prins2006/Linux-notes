# FTP (File Transfer Protocol)

# Introduction

FTP (File Transfer Protocol) is a standard network protocol used to transfer files between computers over a TCP/IP network.

FTP allows users to:

- Upload files to a server
- Download files from a server
- Create directories
- Delete files
- Rename files
- Manage remote file systems

FTP is one of the oldest Internet protocols and is still used in some environments for file sharing and application distribution.

---

# What Does FTP Mean?

```text
FTP = File Transfer Protocol
```

It is an Application Layer protocol in the TCP/IP model.

---

# Why FTP is Used?

FTP is used for:

- Website file uploads
- Software distribution
- Backup transfers
- File sharing
- Data exchange between systems

Example:

```text
Local Computer
       |
       | FTP
       v
Remote Server
```

---

# FTP Architecture

FTP follows a Client-Server model.

```text
+------------+       +------------+
| FTP Client | ----> | FTP Server |
+------------+       +------------+
```

Examples of FTP Clients:

- ftp command
- lftp
- FileZilla
- WinSCP

Examples of FTP Servers:

- vsftpd
- ProFTPD
- Pure-FTPd

---

# FTP Ports

FTP uses two TCP ports:

| Port | Purpose |
|--------|---------|
| 21 | Control Connection |
| 20 | Data Transfer (Active Mode) |

---

# FTP Connections

FTP uses two separate connections:

## Control Connection

Used for:

- Authentication
- Commands
- Responses

Port:

```text
TCP 21
```

---

## Data Connection

Used for:

- Uploading files
- Downloading files
- Directory listings

Port:

```text
TCP 20
```

(Active Mode)

---

# FTP Modes

## 1. Active Mode

```text
Client ------> Server Port 21
Client <------ Server Port 20
```

Server initiates the data connection.

Problems:

- Firewall issues
- NAT issues

---

## 2. Passive Mode

```text
Client ------> Server Port 21
Client ------> Server Random Port
```

Client initiates both connections.

Advantages:

- Firewall friendly
- Most commonly used today

---

# Installing FTP Server (vsftpd)

## What is vsftpd?

```text
Very Secure FTP Daemon
```

Popular FTP server for Linux.

---

## Install on RHEL/Rocky/AlmaLinux

```bash
sudo dnf install vsftpd -y
```

---

## Install on Ubuntu

```bash
sudo apt install vsftpd -y
```

---

# Managing FTP Service

Start service:

```bash
sudo systemctl start vsftpd
```

Enable at boot:

```bash
sudo systemctl enable vsftpd
```

Check status:

```bash
systemctl status vsftpd
```

Restart:

```bash
sudo systemctl restart vsftpd
```

---

# Verify FTP Port

```bash
ss -tulnp | grep :21
```

Example:

```text
tcp LISTEN 0 32 *:21
```

---

# FTP Configuration File

Main configuration:

```bash
/etc/vsftpd/vsftpd.conf
```

View:

```bash
vi /etc/vsftpd/vsftpd.conf
```

---

# Important FTP Configuration Parameters

## Anonymous Login

Enable:

```text
anonymous_enable=YES
```

Disable:

```text
anonymous_enable=NO
```

Production recommendation:

```text
NO
```

---

## Local User Login

```text
local_enable=YES
```

Allows Linux users to login.

---

## Enable Uploads

```text
write_enable=YES
```

Allows file uploads.

---

## User Home Directory Access

```text
chroot_local_user=YES
```

Locks users inside their home directory.

Security best practice.

---

# Restart After Configuration Changes

```bash
systemctl restart vsftpd
```

---

# Configure Firewall

Open FTP service:

```bash
firewall-cmd --permanent --add-service=ftp
firewall-cmd --reload
```

---

# SELinux Configuration

Allow FTP home directories:

```bash
setsebool -P ftp_home_dir on
```

Check:

```bash
getsebool ftp_home_dir
```

---

# FTP Client Command

Connect:

```bash
ftp server-ip
```

Example:

```bash
ftp 192.168.1.100
```

Login:

```text
Name:
Password:
```

---

# Basic FTP Commands

## Show Current Directory

```ftp
pwd
```

---

## List Files

```ftp
ls
```

---

## Change Directory

```ftp
cd directory
```

---

## Upload File

```ftp
put file.txt
```

---

## Download File

```ftp
get file.txt
```

---

## Upload Multiple Files

```ftp
mput *.txt
```

---

## Download Multiple Files

```ftp
mget *.txt
```

---

## Create Directory

```ftp
mkdir backups
```

---

## Delete File

```ftp
delete file.txt
```

---

## Rename File

```ftp
rename old.txt new.txt
```

---

## Exit FTP

```ftp
bye
```

or

```ftp
quit
```

---

# FTP User Access Control

Allow only specific users:

File:

```bash
/etc/vsftpd/user_list
```

Example:

```text
admin
backupuser
```

Configuration:

```text
userlist_enable=YES
```

---

# FTP Logs

Log file:

```bash
/var/log/vsftpd.log
```

View:

```bash
tail -f /var/log/vsftpd.log
```

System logs:

```bash
journalctl -u vsftpd
```

---

# Testing FTP Connectivity

Check port:

```bash
telnet server-ip 21
```

or

```bash
nc -zv server-ip 21
```

Output:

```text
Connected
```

---

# Security Problems with FTP

FTP transfers:

- Usernames
- Passwords
- Data

in plain text.

Example:

```text
Username: admin
Password: admin123
```

Can be captured using packet sniffers.

---

# Secure Alternatives

## FTPS

FTP over SSL/TLS

Encrypted FTP communication.

---

## SFTP

SSH File Transfer Protocol

Uses:

```text
TCP Port 22
```

Part of SSH.

Most recommended.

---

# FTP vs SFTP

| Feature | FTP | SFTP |
|----------|---------|---------|
| Port | 21 | 22 |
| Encryption | No | Yes |
| Authentication | Plain Text | Secure |
| Uses SSH | No | Yes |
| Security | Low | High |
| Production Usage | Limited | Very Common |

---

# FTP vs FTPS

| Feature | FTP | FTPS |
|----------|---------|---------|
| Encryption | No | SSL/TLS |
| Port | 21 | 21 + TLS |
| Security | Low | Better |
| Certificates | No | Yes |

---

# Real Production Scenario

Legacy Application Server:

Requirements:

- Internal file uploads
- Authenticated users
- Restricted access

Configuration:

```text
anonymous_enable=NO

local_enable=YES

write_enable=YES

chroot_local_user=YES
```

Restart:

```bash
systemctl restart vsftpd
```

---

# Interview Questions

### What is FTP?

File Transfer Protocol used to transfer files over a network.

---

### Which ports does FTP use?

```text
21/TCP (Control)
20/TCP (Data)
```

---

### What is vsftpd?

Very Secure FTP Daemon, a popular Linux FTP server.

---

### Which file configures vsftpd?

```bash
/etc/vsftpd/vsftpd.conf
```

---

### How do you start the FTP service?

```bash
systemctl start vsftpd
```

---

### What command uploads a file?

```ftp
put file.txt
```

---

### What command downloads a file?

```ftp
get file.txt
```

---

### Why is FTP insecure?

Because usernames, passwords, and data are transmitted in plain text.

---

### Which protocol is preferred instead of FTP?

```text
SFTP
```

---

# Common Administrator Commands

Check FTP service:

```bash
systemctl status vsftpd
```

Check FTP port:

```bash
ss -tulnp | grep :21
```

Restart FTP:

```bash
systemctl restart vsftpd
```

View logs:

```bash
journalctl -u vsftpd
```

Open firewall:

```bash
firewall-cmd --add-service=ftp
```

Test port:

```bash
nc -zv server-ip 21
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `systemctl start vsftpd` | Start FTP service |
| `systemctl enable vsftpd` | Enable at boot |
| `systemctl status vsftpd` | Check status |
| `ftp server-ip` | Connect to FTP server |
| `put file.txt` | Upload file |
| `get file.txt` | Download file |
| `ls` | List files |
| `pwd` | Show directory |
| `quit` | Exit FTP |
| `journalctl -u vsftpd` | View logs |

# Exam Tip

For modern Linux production environments:

❌ FTP is generally avoided because it is not encrypted.

✅ SFTP (SSH File Transfer Protocol) is the preferred and secure method for file transfers.

Most cloud servers and enterprise Linux systems use **SSH/SFTP** instead of traditional FTP.