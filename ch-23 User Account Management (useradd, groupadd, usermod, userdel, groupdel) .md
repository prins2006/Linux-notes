# User Account Management in Linux

## Introduction

Linux is a multi-user operating system. System administrators use user and group management commands to:
- Create users
- Create groups
- Modify users
- Delete users
- Manage permissions
- Control access to files and directories

---

# Important Files

| File | Purpose |
|-------|----------|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted passwords |
| `/etc/group` | Group information |
| `/etc/gshadow` | Group passwords |

View user information:

```bash
cat /etc/passwd
```

View groups:

```bash
cat /etc/group
```

---

# 1. useradd

## Purpose

Creates a new user account.

## Syntax

```bash
sudo useradd [options] username
```

---

## Basic Example

Create user:

```bash
sudo useradd john
```

Check:

```bash
id john
```

Output:

```
uid=1001(john) gid=1001(john)
```

---

## Create Home Directory

```bash
sudo useradd -m john
```

Creates:

```
/home/john
```

---

## Create with Custom Home Directory

```bash
sudo useradd -m -d /data/john john
```

---

## Create with Specific UID

```bash
sudo useradd -u 2001 john
```

---

## Create with Specific Shell

```bash
sudo useradd -s /bin/bash john
```

---

## Create with Group

```bash
sudo useradd -g developers john
```

---

## Set Password

```bash
sudo passwd john
```

Example:

```
New password:
Retype password:
password updated successfully
```

---

# Production Example

Create deployment user:

```bash
sudo useradd -m -s /bin/bash deploy
sudo passwd deploy
```

---

# 2. groupadd

## Purpose

Creates a new group.

## Syntax

```bash
sudo groupadd groupname
```

---

## Example

Create developers group:

```bash
sudo groupadd developers
```

Verify:

```bash
grep developers /etc/group
```

Output:

```
developers:x:1002:
```

---

## Create with GID

```bash
sudo groupadd -g 3000 developers
```

---

# Production Example

Create DevOps team group:

```bash
sudo groupadd devops
```

---

# 3. usermod

## Purpose

Modify existing user accounts.

---

## Change Username

```bash
sudo usermod -l mike john
```

---

## Change Home Directory

```bash
sudo usermod -d /home/mike -m mike
```

---

## Change Login Shell

```bash
sudo usermod -s /bin/bash mike
```

---

## Add User to Secondary Group

Create group:

```bash
sudo groupadd docker
```

Add user:

```bash
sudo usermod -aG docker mike
```

Check:

```bash
groups mike
```

Output:

```
mike docker
```

---

## Lock User Account

```bash
sudo usermod -L mike
```

Unlock:

```bash
sudo usermod -U mike
```

---

# Production Example

Allow deploy user to use Docker:

```bash
sudo usermod -aG docker deploy
```

---

# 4. userdel

## Purpose

Delete user accounts.

---

## Delete User

```bash
sudo userdel john
```

Home directory remains.

---

## Delete User and Home Directory

```bash
sudo userdel -r john
```

Deletes:

```
User account
Home directory
Mail spool
```

---

# Production Example

Remove resigned employee:

```bash
sudo userdel -r employee1
```

---

# 5. groupdel

## Purpose

Delete groups.

---

## Syntax

```bash
sudo groupdel groupname
```

---

## Example

```bash
sudo groupdel developers
```

---

# Check User Information

## id

```bash
id john
```

Example:

```
uid=1001(john)
gid=1001(john)
groups=1001(john),1002(developers)
```

---

## groups

```bash
groups john
```

Output:

```
john developers docker
```

---

## whoami

```bash
whoami
```

Output:

```
john
```

---

# Production Scenario 1

## New Developer Joining

Create group:

```bash
sudo groupadd developers
```

Create user:

```bash
sudo useradd -m -g developers alice
```

Set password:

```bash
sudo passwd alice
```

Add Docker access:

```bash
sudo usermod -aG docker alice
```

Verify:

```bash
id alice
```

---

# Production Scenario 2

## DevOps Engineer Setup

Create group:

```bash
sudo groupadd devops
```

Create user:

```bash
sudo useradd -m -g devops deploy
```

Set password:

```bash
sudo passwd deploy
```

Grant Docker access:

```bash
sudo usermod -aG docker deploy
```

Grant sudo access:

```bash
sudo usermod -aG sudo deploy
```

---

# Production Scenario 3

## Employee Leaves Company

Lock account:

```bash
sudo usermod -L alice
```

Backup data.

Delete account:

```bash
sudo userdel -r alice
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| useradd | Create user |
| groupadd | Create group |
| usermod | Modify user |
| userdel | Delete user |
| groupdel | Delete group |
| passwd | Set password |
| id | Display user information |
| groups | Display user groups |
| whoami | Show current user |

---

# Common Options

| Option | Purpose |
|----------|----------|
| -m | Create home directory |
| -d | Custom home directory |
| -s | Login shell |
| -u | User ID |
| -g | Primary group |
| -G | Secondary groups |
| -aG | Append user to group |
| -L | Lock account |
| -U | Unlock account |
| -r | Remove home directory |

---

# Interview Questions

## Q1. What is the difference between useradd and adduser?

| useradd | adduser |
|----------|----------|
| Low-level command | User-friendly script |
| Requires more options | Interactive |
| Available on most Linux systems | Common on Debian/Ubuntu |

---

## Q2. What does `usermod -aG docker john` do?

- `-a` = Append
- `-G` = Secondary group
- Adds user `john` to the `docker` group without removing existing groups.

---

## Q3. How do you delete a user and its home directory?

```bash
sudo userdel -r john
```

---

## Q4. How do you lock a user account?

```bash
sudo usermod -L john
```

---

## Q5. Which files store user and group information?

```
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
```

---

# Real Production Use Cases

Linux administrators and DevOps engineers use these commands for:

- Creating application users
- Managing developer accounts
- Granting Docker and Kubernetes access
- CI/CD service accounts
- SSH user management
- Onboarding new employees
- Offboarding resigned employees
- Managing server access
- Configuring deployment users
- Maintaining security and permissions

---

# Summary

- `useradd` creates users.
- `groupadd` creates groups.
- `usermod` modifies users and group memberships.
- `userdel` removes users.
- `groupdel` removes groups.
- `passwd` sets passwords.
- `id` and `groups` verify user details.
- These commands are essential for Linux System Administration, DevOps, Cloud, and production server management.