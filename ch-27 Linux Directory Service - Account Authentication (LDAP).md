# Linux Directory Service - Account Authentication (LDAP)

## What is LDAP?

**LDAP (Lightweight Directory Access Protocol)** is a protocol used to access and manage directory information over a network. It provides a centralized database for storing user accounts, passwords, groups, computers, and other organizational information.

In simple terms:

> **LDAP allows multiple Linux servers and applications to use a central user database for authentication and authorization.**

---

# Why LDAP?

Without LDAP:

* Each Linux server has its own `/etc/passwd` and `/etc/shadow`.
* Users need separate accounts on every server.
* Administrators must manage users individually.

With LDAP:

* One central server stores user information.
* Users can log in to multiple servers with the same credentials.
* User management becomes easier.

---

# Production Use Cases

LDAP is widely used in:

* Enterprise Linux environments
* Data centers
* Cloud infrastructure
* Banking systems
* Universities
* Corporate networks
* Email systems
* VPN authentication
* Web applications

Example:

```
Employee Login

        User
         |
         |
+----------------+
| Linux Server 1 |
+----------------+
         |
         |
+----------------+
| Linux Server 2 |
+----------------+
         |
         |
+----------------+
| Linux Server 3 |
+----------------+
         |
         |
+----------------+
| LDAP Server    |
+----------------+
```

One username and password work across all servers.

---

# What is a Directory Service?

A directory service stores information in a hierarchical structure.

Example:

```
Company

|
+-- Employees
|
|   +-- John
|   +-- Alice
|
+-- IT
|
|   +-- Admin
|   +-- Developer
|
+-- HR
```

LDAP manages this information efficiently.

---

# LDAP Components

## 1. LDAP Server

Stores directory information.

Popular LDAP servers:

* OpenLDAP
* 389 Directory Server
* Active Directory

---

## 2. LDAP Client

Linux machine connecting to LDAP.

Packages:

```
sssd
nslcd
openldap-clients
```

---

## 3. LDAP Database

Stores:

* Users
* Passwords
* Groups
* Computers
* Printers
* Policies

---

# LDAP Directory Structure

Example:

```
dc=company,dc=com

|
+-- ou=People
|     |
|     +-- uid=john
|     +-- uid=alice
|
+-- ou=Groups
      |
      +-- developers
      +-- admins
```

---

# LDAP Terms

## DN (Distinguished Name)

Unique identifier.

Example:

```
uid=john,ou=People,dc=company,dc=com
```

---

## CN

Common Name

Example:

```
cn=John Doe
```

---

## OU

Organizational Unit

Example:

```
ou=People
ou=IT
```

---

## DC

Domain Component

Example:

```
dc=company
dc=com
```

Domain:

```
company.com
```

becomes

```
dc=company,dc=com
```

---

## UID

User ID

Example:

```
uid=john
```

---

# Authentication Process

```
User Login
    |
    |
Linux Server
    |
    |
LDAP Client
    |
    |
LDAP Server
    |
    |
Verify Username & Password
    |
    |
Authentication Success
```

---

# LDAP Authentication in Linux

Linux checks:

```
/etc/nsswitch.conf
```

Example:

```
passwd: files ldap
group:  files ldap
shadow: files ldap
```

Meaning:

1. Check local files.
2. Check LDAP.

---

# Important Configuration Files

## LDAP Client

```
/etc/openldap/
/etc/ldap/
/etc/sssd/sssd.conf
```

---

## NSS

```
/etc/nsswitch.conf
```

---

## PAM

```
/etc/pam.d/
```

---

# Common LDAP Commands

## Search LDAP

```bash
ldapsearch
```

Example:

```bash
ldapsearch -x
```

---

## Add Entry

```bash
ldapadd
```

---

## Delete Entry

```bash
ldapdelete
```

---

## Modify Entry

```bash
ldapmodify
```

---

## Compare Entry

```bash
ldapcompare
```

---

# Install LDAP Client

Ubuntu:

```bash
sudo apt install ldap-utils
```

RHEL:

```bash
sudo yum install openldap-clients
```

---

# LDAP Search Example

```bash
ldapsearch -x -LLL
```

Search user:

```bash
ldapsearch -x uid=john
```

---

# LDAP vs Local Authentication

| Local                | LDAP                    |
| -------------------- | ----------------------- |
| Separate accounts    | Central accounts        |
| Difficult management | Easy management         |
| Single server        | Multiple servers        |
| Local login          | Network login           |
| Small environments   | Enterprise environments |

---

# LDAP vs Active Directory

| LDAP               | Active Directory            |
| ------------------ | --------------------------- |
| Protocol           | Microsoft directory service |
| Open standard      | Uses LDAP internally        |
| Cross-platform     | Windows-focused             |
| OpenLDAP available | AD Domain Controller        |

---

# Production Scenario

A company has:

* 500 employees
* 100 Linux servers

Without LDAP:

```
500 × 100 accounts
```

Need to manage 50,000 account entries.

With LDAP:

```
500 accounts
```

Centralized management.

Employee changes password once.

New password works everywhere.

---

# Security

LDAP traffic:

```
ldap://
```

Port:

```
389
```

Secure LDAP:

```
ldaps://
```

Port:

```
636
```

Production environments should use:

* LDAPS
* TLS encryption
* Strong passwords
* Access controls

---

# Related Linux Authentication Services

## PAM

Pluggable Authentication Modules

Controls authentication.

Configuration:

```
/etc/pam.d/
```

---

## NSS

Name Service Switch

Configuration:

```
/etc/nsswitch.conf
```

---

## SSSD

System Security Services Daemon

Provides:

* LDAP authentication
* Active Directory integration
* Kerberos support
* Credential caching

Configuration:

```
/etc/sssd/sssd.conf
```

---

# Interview Questions

## What does LDAP stand for?

Lightweight Directory Access Protocol.

---

## What is LDAP used for?

Centralized authentication and directory services.

---

## Default LDAP Port?

```
389
```

---

## Secure LDAP Port?

```
636
```

---

## What is DN?

Distinguished Name.

Example:

```
uid=john,ou=People,dc=company,dc=com
```

---

## What is OU?

Organizational Unit.

Example:

```
ou=IT
```

---

## What is DC?

Domain Component.

Example:

```
dc=company,dc=com
```

---

# Quick Revision

| Task         | Command/File          |
| ------------ | --------------------- |
| Search LDAP  | `ldapsearch`          |
| Add entry    | `ldapadd`             |
| Delete entry | `ldapdelete`          |
| Modify entry | `ldapmodify`          |
| LDAP port    | `389`                 |
| LDAPS port   | `636`                 |
| NSS config   | `/etc/nsswitch.conf`  |
| PAM config   | `/etc/pam.d/`         |
| SSSD config  | `/etc/sssd/sssd.conf` |

---

# Real Production Example

A developer logs in to a Linux server.

```
Username: john
Password: ********
```

Linux server:

1. Receives login request.
2. Contacts the LDAP server.
3. Verifies credentials.
4. Retrieves user and group information.
5. Grants access if authentication succeeds.

If John changes his password on the LDAP server, the new password automatically works across all integrated Linux servers and applications.

---

# Summary

* LDAP stands for **Lightweight Directory Access Protocol**.
* It provides centralized user authentication and directory management.
* LDAP stores users, groups, and organizational information.
* Linux integrates LDAP using NSS, PAM, and SSSD.
* Common ports are **389 (LDAP)** and **636 (LDAPS)**.
* Enterprise environments use LDAP to simplify user management and provide single, centralized authentication across multiple systems.
