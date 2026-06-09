# Linux File Permissions

## Introduction

File permissions in Linux control who can:

* Read a file
* Write (modify) a file
* Execute a file

Linux uses permissions to protect files and directories from unauthorized access.

---

# Why are File Permissions Important?

File permissions help:

* Protect sensitive data.
* Prevent accidental file modifications.
* Control who can access files and directories.
* Improve system security.

---

# Check File Permissions

Use the `ls -l` command:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 prins developers 1024 Jun 10 10:30 notes.txt
```

---

# Understanding the Output

```text
-rw-r--r--
```

Breakdown:

```text
- rw- r-- r--
| |   |   |
| |   |   +-- Others
| |   +------ Group
| +---------- Owner
+------------ File Type
```

---

# File Types

| Symbol | Meaning          |
| ------ | ---------------- |
| -      | Regular file     |
| d      | Directory        |
| l      | Symbolic link    |
| c      | Character device |
| b      | Block device     |

Example:

```text
-rw-r--r--  File
drwxr-xr-x  Directory
lrwxrwxrwx  Symbolic Link
```

---

# Permission Types

There are three permissions:

| Symbol | Meaning |
| ------ | ------- |
| r      | Read    |
| w      | Write   |
| x      | Execute |

---

# User Categories

Linux divides users into three groups:

| Category   | Description             |
| ---------- | ----------------------- |
| Owner (u)  | File creator            |
| Group (g)  | Users in the same group |
| Others (o) | Everyone else           |

Example:

```text
rw- r-- r--
```

| Owner | Group | Others |
| ----- | ----- | ------ |
| rw-   | r--   | r--    |

Owner:

* Read
* Write

Group:

* Read

Others:

* Read

---

# Numeric Permissions

Each permission has a value:

| Permission | Value |
| ---------- | ----- |
| Read       | 4     |
| Write      | 2     |
| Execute    | 1     |

Add the values together.

## Examples

### 7 = 4 + 2 + 1

```text
rwx
```

### 6 = 4 + 2

```text
rw-
```

### 5 = 4 + 1

```text
r-x
```

### 4

```text
r--
```

### 0

```text
---
```

---

# Common Permission Values

| Number | Permission |
| ------ | ---------- |
| 777    | rwxrwxrwx  |
| 755    | rwxr-xr-x  |
| 744    | rwxr--r--  |
| 700    | rwx------  |
| 666    | rw-rw-rw-  |
| 644    | rw-r--r--  |

---

# chmod Command

The `chmod` command changes file permissions.

## Syntax

```bash
chmod permissions filename
```

Example:

```bash
chmod 755 script.sh
```

Permission becomes:

```text
rwxr-xr-x
```

---

# Symbolic Method

Add execute permission:

```bash
chmod +x script.sh
```

Remove write permission:

```bash
chmod -w file.txt
```

Add write permission for owner:

```bash
chmod u+w file.txt
```

Remove execute permission for others:

```bash
chmod o-x script.sh
```

---

# Numeric Method

Give full permission:

```bash
chmod 777 file.txt
```

Owner full access, others read only:

```bash
chmod 744 file.txt
```

Common for scripts:

```bash
chmod 755 script.sh
```

Common for normal files:

```bash
chmod 644 notes.txt
```

---

# Change File Owner

Use `chown`.

```bash
sudo chown prins notes.txt
```

Change owner and group:

```bash
sudo chown prins:developers notes.txt
```

---

# Change Group

Use `chgrp`.

```bash
chgrp developers notes.txt
```

---

# Directory Permissions

Directory permissions work differently.

| Permission | Meaning                |
| ---------- | ---------------------- |
| r          | List files             |
| w          | Create or delete files |
| x          | Enter the directory    |

Example:

```bash
drwxr-xr-x
```

Owner:

* Read
* Write
* Enter

Group:

* Read
* Enter

Others:

* Read
* Enter

---

# Practical Examples

## Make a script executable

```bash
chmod +x install.sh
```

Run:

```bash
./install.sh
```

---

## Secure a private file

```bash
chmod 600 secrets.txt
```

Only the owner can read and write.

---

## Create a public script

```bash
chmod 755 app.sh
```

Owner can modify it.
Everyone can execute it.

---

# Interview Questions

## What are Linux file permissions?

They control who can read, write, and execute files.

---

## What does 755 mean?

```text
Owner : rwx
Group : r-x
Others: r-x
```

---

## What does 644 mean?

```text
Owner : rw-
Group : r--
Others: r--
```

---

## What is chmod?

A command used to change file permissions.

---

## What is chown?

A command used to change the file owner.

---

## What is the difference between chmod and chown?

| chmod               | chown         |
| ------------------- | ------------- |
| Changes permissions | Changes owner |

---

# Summary

* Linux has three permission types:

  * Read (r)
  * Write (w)
  * Execute (x)

* Three user categories:

  * Owner
  * Group
  * Others

* Important commands:

```bash
ls -l
chmod 755 file
chmod +x script.sh
chown user file
chgrp group file
```

* Common permissions:

  * 644 for normal files.
  * 755 for executable scripts.
  * 700 for private files.
  * 600 for confidential files.
