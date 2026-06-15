# Enable Password Aging in Linux

## What is Password Aging?

Password Aging is a Linux security feature that controls how long a user can use the same password before changing it. It helps protect systems by forcing regular password updates and preventing old passwords from being used indefinitely.

### Production Use Cases

* Enterprise Linux servers
* Cloud infrastructure
* Banking applications
* Production application servers
* Compliance standards (PCI-DSS, ISO 27001, HIPAA)

---

# Password Aging Configuration Files

## 1. `/etc/login.defs`

This file contains the default password aging settings for newly created users.

### View Configuration

```bash
cat /etc/login.defs
```

Example:

```text
PASS_MAX_DAYS   90
PASS_MIN_DAYS   7
PASS_WARN_AGE   14
PASS_MIN_LEN    8
```

### Parameters

| Parameter     | Description                           |
| ------------- | ------------------------------------- |
| PASS_MAX_DAYS | Maximum password validity             |
| PASS_MIN_DAYS | Minimum days before changing password |
| PASS_WARN_AGE | Warning days before expiry            |
| PASS_MIN_LEN  | Minimum password length               |

---

## 2. `/etc/shadow`

This file stores encrypted passwords and password aging information.

View file:

```bash
sudo cat /etc/shadow
```

Example:

```text
john:$6$hash:20250:7:90:14:15::
```

### Shadow File Fields

| Field                | Description                  |
| -------------------- | ---------------------------- |
| Username             | User account                 |
| Password Hash        | Encrypted password           |
| Last Password Change | Days since Jan 1, 1970       |
| Minimum Days         | Minimum password age         |
| Maximum Days         | Maximum password age         |
| Warning Days         | Password expiry warning      |
| Inactive Days        | Disable account after expiry |
| Account Expiry       | Account expiration date      |

---

# chage Command

The `chage` command is used to manage password aging.

## Syntax

```bash
chage [options] username
```

---

# Check Password Aging

```bash
sudo chage -l username
```

Example:

```bash
sudo chage -l john
```

Output:

```text
Last password change                    : Jun 10, 2026
Password expires                        : Sep 08, 2026
Password inactive                       : never
Account expires                         : never
Minimum number of days between changes  : 7
Maximum number of days between changes  : 90
Warning before password expires         : 14
```

---

# Important chage Options

| Option | Purpose                            |
| ------ | ---------------------------------- |
| -l     | Display password aging information |
| -m     | Set minimum password age           |
| -M     | Set maximum password age           |
| -W     | Set warning days                   |
| -I     | Set inactive days                  |
| -E     | Set account expiry date            |
| -d     | Set last password change date      |

---

# Production Examples

## Set Maximum Password Age

```bash
sudo chage -M 90 john
```

Password expires after 90 days.

---

## Set Minimum Password Age

```bash
sudo chage -m 7 john
```

User cannot change password before 7 days.

---

## Set Warning Period

```bash
sudo chage -W 14 john
```

User receives a warning 14 days before password expiry.

---

## Set Inactive Period

```bash
sudo chage -I 15 john
```

Disable account after 15 inactive days.

---

## Set Account Expiry Date

```bash
sudo chage -E 2026-12-31 john
```

Account expires on December 31, 2026.

---

## Configure Complete Password Policy

```bash
sudo chage -m 7 -M 90 -W 14 -I 15 john
```

Verify:

```bash
sudo chage -l john
```

---

# Force Password Change at Next Login

Useful for:

* New employees
* Password resets
* Temporary accounts

```bash
sudo chage -d 0 john
```

Alternative:

```bash
sudo passwd --expire john
```

User will be forced to create a new password during the next login.

---

# Lock and Unlock User Accounts

## Lock Account

```bash
sudo passwd -l john
```

or

```bash
sudo usermod -L john
```

---

## Unlock Account

```bash
sudo passwd -u john
```

or

```bash
sudo usermod -U john
```

---

# Production Scenario

## Requirement

Create a new employee account with the following policy:

* Password expires after 90 days
* Minimum password age: 7 days
* Warning period: 14 days
* Inactive period: 15 days
* Force password change at first login

## Step 1: Create User

```bash
sudo useradd employee1
```

## Step 2: Set Password

```bash
sudo passwd employee1
```

## Step 3: Configure Password Aging

```bash
sudo chage -m 7 employee1
sudo chage -M 90 employee1
sudo chage -W 14 employee1
sudo chage -I 15 employee1
sudo chage -d 0 employee1
```

## Step 4: Verify Configuration

```bash
sudo chage -l employee1
```

---

# Enterprise Best Practices

## Default Policy

Edit:

```bash
sudo vi /etc/login.defs
```

Recommended:

```text
PASS_MAX_DAYS   90
PASS_MIN_DAYS   7
PASS_WARN_AGE   14
```

---

## Regular Auditing

Check user password policies:

```bash
sudo chage -l username
```

---

## Force Password Reset

```bash
sudo passwd username
sudo chage -d 0 username
```

---

## Disable Inactive Accounts

```bash
sudo chage -I 15 username
```

---

# Interview Questions

## Which file stores password aging information?

```
/etc/shadow
```

## Which file contains default password aging settings?

```
/etc/login.defs
```

## Which command displays password aging information?

```bash
chage -l username
```

## Force password change at next login?

```bash
chage -d 0 username
```

## Set password expiry to 90 days?

```bash
chage -M 90 username
```

---

# Quick Revision

| Task                  | Command            |
| --------------------- | ------------------ |
| Check password policy | `chage -l user`    |
| Set max age           | `chage -M 90 user` |
| Set min age           | `chage -m 7 user`  |
| Set warning           | `chage -W 14 user` |
| Set inactive period   | `chage -I 15 user` |
| Force password change | `chage -d 0 user`  |
| Lock account          | `passwd -l user`   |
| Unlock account        | `passwd -u user`   |
| Default policy file   | `/etc/login.defs`  |
| Password aging file   | `/etc/shadow`      |

---

# Summary

Password Aging is an important Linux security mechanism that helps enforce password rotation policies. In production environments, administrators use the `chage` command along with `/etc/login.defs` and `/etc/shadow` to manage password expiration, warning periods, inactive accounts, and first-login password changes. A common enterprise policy is:

* Maximum Age: 90 days
* Minimum Age: 7 days
* Warning: 14 days
* Inactive: 15 days
* Force password change for new users

This approach improves security and supports organizational compliance requirements.
