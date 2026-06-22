# SSH and Telnet - Configure and Secure SSH

# Objective

Learn how to:

- Configure SSH Server
- Secure SSH Access
- Use SSH Keys
- Restrict User Access
- Change Default SSH Settings
- Troubleshoot SSH Problems
- Understand why SSH is preferred over Telnet

---

# What is SSH?

SSH (Secure Shell) is a secure network protocol used to remotely connect to Linux servers.

Default Port:

```bash
22/TCP
```

Service:

```bash
sshd
```

Configuration File:

```bash
/etc/ssh/sshd_config
```

---

# What is Telnet?

Telnet is an old remote login protocol.

Default Port:

```bash
23/TCP
```

Major Problem:

- No encryption
- Passwords sent in plain text
- Easy to intercept

Example:

```text
Client ---> Username: root
Client ---> Password: admin123
```

Anyone capturing packets can see credentials.

Therefore:

❌ Telnet is not recommended.

✅ SSH is the industry standard.

---

# SSH Architecture

```text
+-------------+          +-------------+
| SSH Client  | -------> | SSH Server  |
+-------------+          +-------------+

      TCP Port 22
```

Common SSH Client:

```bash
ssh
```

SSH Server Process:

```bash
sshd
```

---

# Installing SSH Server

## Ubuntu/Debian

```bash
sudo apt update
sudo apt install openssh-server
```

---

## RHEL/Rocky/AlmaLinux

```bash
sudo dnf install openssh-server -y
```

---

# Managing SSH Service

Check status:

```bash
systemctl status sshd
```

Start service:

```bash
sudo systemctl start sshd
```

Enable at boot:

```bash
sudo systemctl enable sshd
```

Restart:

```bash
sudo systemctl restart sshd
```

Reload configuration:

```bash
sudo systemctl reload sshd
```

---

# Verify SSH is Listening

Using ss:

```bash
ss -tulnp | grep ssh
```

Example:

```text
tcp LISTEN 0 128 *:22 *:* users:(("sshd"))
```

Using netstat:

```bash
netstat -tulnp | grep ssh
```

---

# Basic SSH Connection

Connect to remote server:

```bash
ssh username@server-ip
```

Example:

```bash
ssh admin@192.168.1.100
```

---

# SSH Configuration File

Main file:

```bash
/etc/ssh/sshd_config
```

View:

```bash
sudo vi /etc/ssh/sshd_config
```

Validate syntax before restart:

```bash
sshd -t
```

No output = configuration is valid.

---

# Important SSH Configuration Parameters

## Change SSH Port

Default:

```text
Port 22
```

Change:

```text
Port 2222
```

Restart SSH:

```bash
systemctl restart sshd
```

Connect:

```bash
ssh -p 2222 user@server
```

### Why Change Port?

Reduces automated bot attacks scanning port 22.

---

## Disable Root Login

Default may be:

```text
PermitRootLogin yes
```

Secure setting:

```text
PermitRootLogin no
```

Benefits:

- Prevent direct root access
- Forces use of sudo
- Improves audit tracking

---

## Disable Empty Passwords

```text
PermitEmptyPasswords no
```

---

## Limit Login Attempts

```text
MaxAuthTries 3
```

After 3 failed attempts connection is terminated.

---

## Set Login Timeout

```text
LoginGraceTime 30
```

User must authenticate within 30 seconds.

---

## Disable DNS Lookups

```text
UseDNS no
```

Improves login speed.

---

## Restrict Specific Users

Allow only selected users:

```text
AllowUsers admin devops backup
```

Only these users can SSH.

---

## Restrict Groups

```text
AllowGroups sshusers
```

Create group:

```bash
groupadd sshusers
```

Add user:

```bash
usermod -aG sshusers admin
```

---

# Configure SSH Key Authentication

Password authentication is less secure.

Production servers often use SSH keys.

---

## Generate Key Pair

On client machine:

```bash
ssh-keygen
```

Files created:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

Modern systems may create:

```text
id_ed25519
id_ed25519.pub
```

---

## Copy Public Key to Server

```bash
ssh-copy-id user@server
```

Manual method:

```bash
cat ~/.ssh/id_rsa.pub
```

Paste into:

```bash
~/.ssh/authorized_keys
```

---

## Login Without Password

```bash
ssh user@server
```

Authentication uses private key automatically.

---

# Disable Password Authentication

After verifying key-based login works:

Edit:

```bash
/etc/ssh/sshd_config
```

Set:

```text
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart:

```bash
systemctl restart sshd
```

Benefits:

- Blocks password attacks
- Protects against brute-force attempts

---

# Configure Firewall for SSH

## firewalld

Allow SSH:

```bash
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload
```

Custom port:

```bash
firewall-cmd --permanent --add-port=2222/tcp
firewall-cmd --reload
```

---

## UFW (Ubuntu)

Allow SSH:

```bash
ufw allow ssh
```

Custom port:

```bash
ufw allow 2222/tcp
```

---

# SELinux and Custom SSH Port

If changing port on RHEL:

Check existing SSH ports:

```bash
semanage port -l | grep ssh
```

Add new port:

```bash
semanage port -a -t ssh_port_t -p tcp 2222
```

Verify:

```bash
semanage port -l | grep ssh
```

---

# SSH Client Configuration

File:

```bash
~/.ssh/config
```

Example:

```text
Host webserver

HostName 192.168.1.100

User admin

Port 2222
```

Connect simply:

```bash
ssh webserver
```

---

# SSH File Permissions

Correct permissions are critical.

User home:

```bash
chmod 700 ~/.ssh
```

Authorized keys:

```bash
chmod 600 ~/.ssh/authorized_keys
```

Private key:

```bash
chmod 600 ~/.ssh/id_rsa
```

Public key:

```bash
chmod 644 ~/.ssh/id_rsa.pub
```

---

# SSH Troubleshooting

## Check Service

```bash
systemctl status sshd
```

---

## Check Listening Port

```bash
ss -tulnp | grep ssh
```

---

## Test Configuration

```bash
sshd -t
```

---

## Check Logs

RHEL:

```bash
journalctl -u sshd
```

Ubuntu:

```bash
journalctl -u ssh
```

Live monitoring:

```bash
journalctl -fu sshd
```

---

## Debug Client Connection

```bash
ssh -v user@server
```

More details:

```bash
ssh -vv user@server
```

Maximum debugging:

```bash
ssh -vvv user@server
```

---

# Production Security Best Practices

## Recommended SSH Configuration

```text
Port 2222

PermitRootLogin no

PasswordAuthentication no

PubkeyAuthentication yes

PermitEmptyPasswords no

MaxAuthTries 3

LoginGraceTime 30

AllowUsers admin devops

UseDNS no
```

---

# Real Production Scenario

Company Linux Server:

Requirements:

- No root login
- SSH key authentication only
- Custom port 2222
- Only admin and devops users allowed

Configuration:

```text
Port 2222

PermitRootLogin no

PasswordAuthentication no

PubkeyAuthentication yes

AllowUsers admin devops
```

Apply:

```bash
sshd -t
systemctl restart sshd
```

Open firewall:

```bash
firewall-cmd --add-port=2222/tcp --permanent
firewall-cmd --reload
```

---

# SSH vs Telnet

| Feature | SSH | Telnet |
|----------|------|---------|
| Port | 22 | 23 |
| Encryption | Yes | No |
| Password Protection | Encrypted | Plain Text |
| Secure File Transfer | Yes | No |
| Public Key Authentication | Yes | No |
| Production Usage | Yes | No |
| Compliance Friendly | Yes | No |
| Security | High | Very Low |

---

# Interview Questions

### What is SSH?

Secure Shell protocol used for secure remote administration.

### What process handles SSH connections?

```bash
sshd
```

### Which file configures SSH server?

```bash
/etc/ssh/sshd_config
```

### How do you test SSH configuration before restarting?

```bash
sshd -t
```

### How do you disable root login?

```text
PermitRootLogin no
```

### How do you disable password authentication?

```text
PasswordAuthentication no
```

### How do you generate SSH keys?

```bash
ssh-keygen
```

### Why is SSH more secure than Telnet?

SSH encrypts all traffic; Telnet sends data in plain text.

---

# Quick Revision

```bash
systemctl status sshd
```
Check SSH service

```bash
ssh user@server
```
Connect to server

```bash
ssh-keygen
```
Generate SSH key pair

```bash
ssh-copy-id user@server
```
Copy public key

```bash
sshd -t
```
Validate SSH configuration

```bash
journalctl -u sshd
```
View SSH logs

```bash
ssh -vvv user@server
```
Debug SSH connection

```bash
firewall-cmd --add-service=ssh
```
Allow SSH through firewall

```bash
chmod 600 ~/.ssh/authorized_keys
```
Secure SSH key permissions

## Exam Tip

For production Linux servers:

✅ SSH Keys  
✅ Disable Root Login  
✅ Disable Password Authentication  
✅ Restrict Users  
✅ Monitor Logs  
✅ Use Firewall Rules  

These are the most common SSH hardening practices expected from Linux Administrators and DevOps Engineers.