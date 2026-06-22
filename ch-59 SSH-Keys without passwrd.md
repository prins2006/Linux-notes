# SSH Keys - Access Remote Server Without Password

# Introduction

SSH Keys provide a secure way to log in to remote Linux servers without typing a password every time.

Instead of using:

```text
Username + Password
```

SSH uses:

```text
Public Key + Private Key
```

This method is:

✅ More Secure

✅ Faster

✅ Used in Production Environments

✅ Required for DevOps Automation

✅ Resistant to Brute Force Attacks

---

# Why Use SSH Keys?

Traditional Login:

```text
Client ---> Username
Client ---> Password
```

Problems:

- Password can be guessed
- Password can be stolen
- Password attacks are common

SSH Key Authentication:

```text
Client ---> Private Key

Server ---> Public Key Verification
```

No password travels across the network.

---

# How SSH Key Authentication Works

```text
+------------------+
| Client Machine   |
| Private Key      |
+--------+---------+
         |
         |
         v
+------------------+
| Remote Server    |
| Public Key       |
+------------------+
```

Server verifies that the client owns the correct private key.

If verification succeeds:

```text
Login Allowed
```

---

# SSH Key Components

When generating keys:

```bash
ssh-keygen
```

Two files are created:

```text
id_rsa
id_rsa.pub
```

or on newer systems:

```text
id_ed25519
id_ed25519.pub
```

---

# Difference Between Public and Private Key

| Key | Purpose |
|-------|----------|
| Private Key | Kept secret on client |
| Public Key | Copied to server |

---

# Key Storage Location

Default directory:

```bash
~/.ssh/
```

Example:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

or

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

---

# Types of SSH Keys

## RSA

Older but still common.

Generate:

```bash
ssh-keygen -t rsa -b 4096
```

---

## ED25519

Modern and recommended.

Generate:

```bash
ssh-keygen -t ed25519
```

Advantages:

- Faster
- Smaller keys
- Better security
- Modern standard

Production recommendation:

```text
ED25519
```

---

# Generate SSH Key Pair

Create key:

```bash
ssh-keygen -t ed25519
```

Output:

```text
Generating public/private key pair.
```

Location:

```text
/home/prins/.ssh/id_ed25519
```

Press Enter.

Passphrase:

```text
Enter passphrase:
```

Optional but recommended.

Result:

```text
id_ed25519
id_ed25519.pub
```

---

# View Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Example:

```text
ssh-ed25519 AAAAC3Nza...
```

---

# Copy Public Key to Remote Server

Recommended method:

```bash
ssh-copy-id user@server
```

Example:

```bash
ssh-copy-id admin@192.168.1.100
```

Output:

```text
Number of key(s) added: 1
```

---

# Manual Key Copy

Display public key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy output.

---

# Login to Server

```bash
ssh user@server
```

Example:

```bash
ssh admin@192.168.1.100
```

---

# Create SSH Directory on Server

If needed:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

# Add Public Key

Edit:

```bash
vi ~/.ssh/authorized_keys
```

Paste public key.

Save file.

---

# Secure Permissions

SSH is strict about permissions.

Set:

```bash
chmod 700 ~/.ssh
```

```bash
chmod 600 ~/.ssh/authorized_keys
```

Verify:

```bash
ls -ld ~/.ssh
```

```bash
ls -l ~/.ssh
```

---

# Test Passwordless Login

Connect:

```bash
ssh user@server
```

Expected:

```text
Login successful without password.
```

---

# Disable Password Authentication

After verifying key login works.

Edit:

```bash
sudo vi /etc/ssh/sshd_config
```

Find:

```text
PasswordAuthentication yes
```

Change:

```text
PasswordAuthentication no
```

Ensure:

```text
PubkeyAuthentication yes
```

Restart SSH:

```bash
sudo systemctl restart sshd
```

---

# Verify SSH Authentication Method

Client:

```bash
ssh -v user@server
```

Look for:

```text
Offering public key
```

and

```text
Authentication succeeded
```

---

# Use Multiple SSH Keys

Generate another key:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/devops_key
```

Files:

```text
devops_key
devops_key.pub
```

Connect:

```bash
ssh -i ~/.ssh/devops_key user@server
```

---

# SSH Agent

SSH Agent stores keys in memory.

Start agent:

```bash
eval $(ssh-agent)
```

Add key:

```bash
ssh-add ~/.ssh/id_ed25519
```

List keys:

```bash
ssh-add -l
```

Benefits:

- Enter passphrase once
- Reuse key automatically

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

IdentityFile ~/.ssh/id_ed25519
```

Connect:

```bash
ssh webserver
```

---

# Troubleshooting SSH Keys

## Permission Denied

Error:

```text
Permission denied (publickey)
```

Check:

```bash
chmod 700 ~/.ssh
```

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## Verify Authorized Key

Server:

```bash
cat ~/.ssh/authorized_keys
```

Ensure correct public key exists.

---

## Check SSH Service

```bash
systemctl status sshd
```

---

## Check SSH Logs

RHEL:

```bash
journalctl -u sshd
```

Ubuntu:

```bash
journalctl -u ssh
```

---

## Debug Connection

```bash
ssh -vvv user@server
```

Shows detailed authentication process.

---

# Production-Level SSH Key Security

## Recommended Settings

File:

```bash
/etc/ssh/sshd_config
```

Configuration:

```text
PermitRootLogin no

PasswordAuthentication no

PubkeyAuthentication yes

PermitEmptyPasswords no

MaxAuthTries 3
```

Restart:

```bash
systemctl restart sshd
```

---

# Real Production Example

Developer Workstation:

```text
192.168.1.50
```

Linux Server:

```text
192.168.1.100
```

Generate key:

```bash
ssh-keygen -t ed25519
```

Copy key:

```bash
ssh-copy-id admin@192.168.1.100
```

Test:

```bash
ssh admin@192.168.1.100
```

Disable passwords:

```text
PasswordAuthentication no
```

Restart:

```bash
systemctl restart sshd
```

Result:

```text
Only SSH key authentication allowed.
```

---

# Interview Questions

## What is SSH Key Authentication?

A method of authentication using a public/private key pair instead of passwords.

---

## Where are SSH keys stored?

```bash
~/.ssh/
```

---

## Which file contains authorized public keys?

```bash
~/.ssh/authorized_keys
```

---

## How do you generate an SSH key?

```bash
ssh-keygen -t ed25519
```

---

## How do you copy a public key to a server?

```bash
ssh-copy-id user@server
```

---

## How do you disable password login?

In:

```bash
/etc/ssh/sshd_config
```

Set:

```text
PasswordAuthentication no
```

---

## What command debugs SSH authentication?

```bash
ssh -vvv user@server
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `ssh-keygen -t ed25519` | Generate SSH key |
| `ssh-copy-id user@server` | Copy public key |
| `ssh user@server` | Login using key |
| `ssh -v user@server` | Verify authentication |
| `ssh-add keyfile` | Add key to agent |
| `chmod 700 ~/.ssh` | Secure SSH directory |
| `chmod 600 authorized_keys` | Secure authorized keys |
| `systemctl restart sshd` | Restart SSH service |
| `journalctl -u sshd` | View SSH logs |
| `ssh -vvv user@server` | Full SSH debugging |

# Exam Tip

**Production servers should use SSH key authentication instead of passwords.**

Best practice:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

This is the standard SSH hardening configuration used in Linux administration, cloud environments, and DevOps infrastructure.