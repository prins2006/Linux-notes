# SSH and Telnet in Linux

# Introduction

SSH (Secure Shell) and Telnet are network protocols used to connect and manage remote computers over a network.

A system administrator can log in to another Linux server remotely and execute commands as if sitting in front of that machine.

Example:
```
Laptop -----------------> Linux Server
       SSH/Telnet
```

SSH is the modern and secure method, while Telnet is an older and insecure protocol.

---

# 1. SSH (Secure Shell)

## Meaning

SSH stands for:

```
Secure Shell
```

SSH is a cryptographic network protocol used for secure remote login and command execution.

It encrypts all communication between the client and server.

Default Port:

```
22/TCP
```

---

# Why SSH is Used?

SSH provides:

- Secure remote login
- Secure file transfer
- Remote command execution
- Server administration
- Automation scripts
- Secure tunneling
- Port forwarding

---

# SSH Architecture

```
+------------+           +------------+
| SSH Client | --------> | SSH Server |
+------------+           +------------+
      Port 22 TCP
```

Client:
```
ssh
```

Server:
```
sshd
```

---

# SSH Package

RHEL:

```bash
rpm -qa | grep openssh
```

Ubuntu:

```bash
dpkg -l | grep openssh
```

Install:

Ubuntu:

```bash
sudo apt install openssh-server
```

RHEL:

```bash
sudo dnf install openssh-server
```

---

# SSH Service

Check status:

```bash
systemctl status ssh
```

RHEL:

```bash
systemctl status sshd
```

Start:

```bash
sudo systemctl start sshd
```

Enable:

```bash
sudo systemctl enable sshd
```

Restart:

```bash
sudo systemctl restart sshd
```

Reload:

```bash
sudo systemctl reload sshd
```

---

# Check SSH Port

```bash
ss -tulnp | grep ssh
```

Example:

```
tcp LISTEN 0 128 *:22 *:* users:(("sshd"))
```

---

# SSH Client Command

Basic syntax:

```bash
ssh username@hostname
```

Example:

```bash
ssh root@192.168.1.100
```

or

```bash
ssh prins@server1
```

---

# Specify Port

```bash
ssh -p 2222 user@server
```

---

# Run Remote Command

```bash
ssh user@server "hostname"
```

Output:

```
linux-server
```

---

# Copy Files Using SCP

Copy to server:

```bash
scp file.txt user@server:/home/user/
```

Copy from server:

```bash
scp user@server:/etc/passwd .
```

Copy directory:

```bash
scp -r test user@server:/tmp/
```

---

# SFTP

Secure File Transfer Protocol

Connect:

```bash
sftp user@server
```

Commands:

```
ls
pwd
get
put
mkdir
exit
```

---

# SSH Key Authentication

Generate key:

```bash
ssh-keygen
```

Files created:

```
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

Copy public key:

```bash
ssh-copy-id user@server
```

Login:

```bash
ssh user@server
```

No password required.

---

# SSH Configuration File

Server configuration:

```
/etc/ssh/sshd_config
```

Common settings:

```
Port 22

PermitRootLogin no

PasswordAuthentication yes

PubkeyAuthentication yes

MaxAuthTries 3
```

Restart after changes:

```bash
systemctl restart sshd
```

---

# SSH Client Configuration

File:

```
~/.ssh/config
```

Example:

```
Host webserver

HostName 192.168.1.100

User prins

Port 22
```

Connect:

```bash
ssh webserver
```

---

# Important SSH Options

Verbose mode:

```bash
ssh -v user@server
```

More debugging:

```bash
ssh -vv
```

Maximum debugging:

```bash
ssh -vvv
```

Compression:

```bash
ssh -C user@server
```

Identity file:

```bash
ssh -i mykey.pem user@server
```

---

# Production SSH Security

Disable root login:

```
PermitRootLogin no
```

Disable empty passwords:

```
PermitEmptyPasswords no
```

Use SSH keys:

```
PubkeyAuthentication yes
```

Limit users:

```
AllowUsers admin devops
```

Change port:

```
Port 2222
```

Restart:

```bash
systemctl restart sshd
```

---

# SSH Use Cases

## System Administration

```bash
ssh admin@server
```

---

## Application Deployment

Deploy application remotely.

---

## Backup Scripts

Automate backups.

---

## DevOps

Jenkins, Ansible, Puppet use SSH.

---

## Cloud Servers

AWS EC2

Azure VM

Google Cloud VM

DigitalOcean

---

# SSH Troubleshooting

Check service:

```bash
systemctl status sshd
```

Check port:

```bash
ss -tulnp | grep 22
```

Check firewall:

```bash
firewall-cmd --list-all
```

Allow SSH:

```bash
firewall-cmd --permanent --add-service=ssh

firewall-cmd --reload
```

Check logs:

```bash
journalctl -u sshd
```

Test connection:

```bash
ssh -v user@server
```

---

# 2. Telnet

# Meaning

Telnet stands for:

```
Telecommunication Network
```

Telnet is an old protocol used for remote terminal access.

Default Port:

```
23/TCP
```

---

# Telnet Architecture

```
Client --------> Server

Port 23
```

---

# Problem with Telnet

Telnet sends:

- Username
- Password
- Commands

in plain text.

Anyone monitoring the network can read the data.

Example:

```
User: root

Password: admin123
```

Not encrypted.

---

# Install Telnet

Ubuntu:

```bash
sudo apt install telnet
```

RHEL:

```bash
sudo dnf install telnet
```

Server:

```bash
sudo dnf install telnet-server
```

---

# Start Service

```bash
systemctl start telnet.socket
```

Enable:

```bash
systemctl enable telnet.socket
```

---

# Connect

```bash
telnet 192.168.1.100
```

---

# Telnet Use Cases Today

Telnet is rarely used for remote login.

Mostly used for testing TCP ports.

Example:

Check SMTP:

```bash
telnet mail.server.com 25
```

Check HTTP:

```bash
telnet google.com 80
```

Check custom application:

```bash
telnet server 8080
```

---

# SSH vs Telnet

| Feature | SSH | Telnet |
|----------|------|--------|
| Full Form | Secure Shell | Telecommunication Network |
| Port | 22 | 23 |
| Encryption | Yes | No |
| Authentication | Secure | Plain Text |
| Secure File Transfer | Yes | No |
| Production Use | Yes | No |
| Remote Administration | Yes | Not Recommended |
| Automation | Yes | Limited |
| Security | High | Very Low |

---

# Common Interview Questions

## What is SSH?

SSH is a secure protocol for remote login and administration using encrypted communication.

---

## What is sshd?

The SSH server daemon that accepts incoming SSH connections.

---

## What port does SSH use?

```
22/TCP
```

---

## What port does Telnet use?

```
23/TCP
```

---

## Why is SSH preferred over Telnet?

Because SSH encrypts all communication, while Telnet sends data in plain text.

---

## What file configures the SSH server?

```
/etc/ssh/sshd_config
```

---

## How do you generate SSH keys?

```bash
ssh-keygen
```

---

## How do you copy an SSH public key to a server?

```bash
ssh-copy-id user@server
```

---

# Production-Level Best Practices

✅ Use SSH instead of Telnet.

✅ Disable root SSH login.

✅ Use SSH key authentication.

✅ Keep OpenSSH updated.

✅ Restrict SSH access to required users.

✅ Monitor SSH logs with:

```bash
journalctl -u sshd
```

✅ Allow only necessary firewall access:

```bash
firewall-cmd --add-service=ssh
```

✅ Disable the Telnet service completely on production servers.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `ssh user@host` | Connect to remote server |
| `ssh -p 2222 user@host` | Connect using custom port |
| `ssh-keygen` | Generate SSH keys |
| `ssh-copy-id user@host` | Copy public key |
| `scp file user@host:/path` | Secure file copy |
| `sftp user@host` | Secure file transfer |
| `systemctl status sshd` | Check SSH service |
| `journalctl -u sshd` | SSH logs |
| `telnet host port` | Test TCP connectivity |
| `ss -tulnp` | Check listening ports |

## Exam Tip

**SSH = Secure, encrypted, production-ready remote administration (Port 22).**

**Telnet = Unencrypted, legacy protocol mainly used today for simple TCP connectivity testing (Port 23).**