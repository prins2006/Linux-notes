# Switch Users and sudo Access in Linux (`su` and `sudo`)

## Introduction

Linux is a multi-user operating system. System administrators can switch between user accounts and execute commands with elevated (administrator) privileges using the `su` and `sudo` commands.

**Production Use Cases:**

* System administration
* Application deployment
* Package installation
* User management
* Service management
* Security and auditing

---

# 1. `su` Command

## What is `su`?

`su` stands for **Substitute User** or **Switch User**. It allows a user to switch to another user account.

## Syntax

```bash
su [options] username
```

If no username is specified, `su` switches to the root user.

Example:

```bash
su
```

You will be prompted for the root password.

---

## Switch to Root User

```bash
su -
```

or

```bash
su - root
```

Example:

```bash
$ su -
Password:
#
```

Verify:

```bash
whoami
```

Output:

```
root
```

---

## Switch to Another User

Example:

```bash
su john
```

Verify:

```bash
whoami
```

Output:

```
john
```

---

## Difference Between `su` and `su -`

### `su`

```bash
su username
```

* Switches user.
* Keeps current environment variables.
* Does not load the target user's login profile.

Example:

```bash
su john
```

---

### `su -`

```bash
su - username
```

* Switches user.
* Loads the target user's environment.
* Simulates a fresh login.

Example:

```bash
su - john
```

Production environments generally prefer:

```bash
su -
```

---

# 2. `sudo` Command

## What is `sudo`?

`sudo` stands for:

**Super User DO**

It allows authorized users to execute commands as another user, usually root.

---

## Syntax

```bash
sudo command
```

Example:

```bash
sudo ls /root
```

---

## Install a Package

```bash
sudo apt update
sudo apt install nginx
```

RHEL/CentOS:

```bash
sudo yum install httpd
```

---

## Edit System Files

```bash
sudo vi /etc/hosts
```

---

## Restart Services

```bash
sudo systemctl restart sshd
```

---

# Check sudo Access

```bash
sudo -l
```

Example:

```bash
$ sudo -l
```

Shows allowed commands for the current user.

---

# Become Root Using sudo

## Method 1

```bash
sudo -i
```

Check:

```bash
whoami
```

Output:

```
root
```

---

## Method 2

```bash
sudo su -
```

---

# Configure sudo Access

Configuration file:

```
/etc/sudoers
```

Edit safely:

```bash
sudo visudo
```

Never edit `/etc/sudoers` directly.

---

## Grant sudo Access

Example:

```
john ALL=(ALL:ALL) ALL
```

Meaning:

| Part      | Description    |
| --------- | -------------- |
| john      | Username       |
| ALL       | Any host       |
| (ALL:ALL) | Any user/group |
| ALL       | Any command    |

---

# Add User to sudo Group

Ubuntu:

```bash
sudo usermod -aG sudo john
```

RHEL/CentOS:

```bash
sudo usermod -aG wheel john
```

Check:

```bash
groups john
```

---

# Production Scenario

## Create User

```bash
sudo useradd devuser
sudo passwd devuser
```

## Add sudo Permission

Ubuntu:

```bash
sudo usermod -aG sudo devuser
```

RHEL:

```bash
sudo usermod -aG wheel devuser
```

## Test

```bash
su - devuser
sudo whoami
```

Output:

```
root
```

---

# Important Commands

## Current User

```bash
whoami
```

---

## User ID

```bash
id
```

Example:

```bash
id john
```

---

## List Groups

```bash
groups
```

---

## Current Login User

```bash
logname
```

---

# Security Best Practices

## Use sudo Instead of Root Login

Recommended:

```bash
sudo command
```

Avoid:

```bash
su -
```

for daily tasks.

---

## Use visudo

```bash
sudo visudo
```

Never edit:

```
/etc/sudoers
```

using a normal editor.

---

## Grant Minimum Permissions

Only provide required access.

---

## Audit sudo Usage

View logs:

Ubuntu:

```bash
sudo cat /var/log/auth.log
```

RHEL:

```bash
sudo cat /var/log/secure
```

---

# Difference Between su and sudo

| Feature                | su          | sudo          |
| ---------------------- | ----------- | ------------- |
| Full user switch       | Yes         | No            |
| Requires root password | Usually Yes | User password |
| Command-specific       | No          | Yes           |
| Logging                | Limited     | Yes           |
| More secure            | No          | Yes           |
| Production recommended | Limited     | Yes           |

---

# Production Best Practice

Instead of:

```bash
su -
rm -rf /tmp/test
```

Use:

```bash
sudo rm -rf /tmp/test
```

This provides auditing and better security.

---

# Interview Questions

## What is `su`?

Switches to another user account.

---

## What is `sudo`?

Executes a command with elevated privileges.

---

## Difference between `su` and `su -`?

`su`:

* Switch user
* Keep current environment

`su -`:

* Switch user
* Load login environment

---

## How to become root using sudo?

```bash
sudo -i
```

---

## Which file controls sudo permissions?

```
/etc/sudoers
```

---

## Which command safely edits sudoers?

```bash
visudo
```

---

# Quick Revision

| Task                   | Command                      |
| ---------------------- | ---------------------------- |
| Switch to root         | `su -`                       |
| Switch to another user | `su username`                |
| Run command as root    | `sudo command`               |
| Become root using sudo | `sudo -i`                    |
| Check sudo permissions | `sudo -l`                    |
| Edit sudoers           | `sudo visudo`                |
| Add user to sudo group | `sudo usermod -aG sudo user` |
| Check current user     | `whoami`                     |
| Check user ID          | `id`                         |
| Check groups           | `groups`                     |

---

# Summary

* `su` is used to switch users.
* `su -` creates a full login session for the target user.
* `sudo` executes individual commands with elevated privileges.
* `sudo` is the preferred method in production because it provides security, auditing, and controlled privilege escalation.
* Use `visudo` to manage sudo permissions and follow the principle of least privilege.
