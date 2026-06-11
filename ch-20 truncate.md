# Linux `truncate` Command - Complete Guide

## What is `truncate`?

The `truncate` command is used to **shrink or extend the size of a file**.

It can:
- Create an empty file.
- Reduce a file's size.
- Increase a file's size.
- Remove file contents without deleting the file.

> **Important:** `truncate` changes the file size but does not delete the file itself.

---

# Syntax

```bash
truncate OPTION... FILE
```

Basic syntax:

```bash
truncate -s SIZE filename
```

Where:
- `-s` = Set file size.

---

# Check File Size

Create a file:

```bash
echo "Linux DevOps Training" > notes.txt
```

Check size:

```bash
ls -lh notes.txt
```

Output:

```
-rw-r--r-- 1 user user 23 Jun 11 notes.txt
```

---

# 1. Make File Empty

```bash
truncate -s 0 notes.txt
```

Check:

```bash
cat notes.txt
```

Output:

```
(No output)
```

Check size:

```bash
ls -lh notes.txt
```

Output:

```
0 bytes
```

---

# 2. Set File Size

Create:

```bash
echo "Linux" > file.txt
```

Set size to 20 bytes:

```bash
truncate -s 20 file.txt
```

Check:

```bash
ls -l file.txt
```

Output:

```
20 bytes
```

---

# View Content

```bash
cat file.txt
```

Output:

```
Linux
```

Extra space is filled with null (`\0`) bytes.

---

# 3. Reduce File Size

File:

```
Linux DevOps Training
```

Command:

```bash
truncate -s 5 file.txt
```

Output:

```
Linux
```

Only the first 5 bytes remain.

---

# 4. Increase File Size

```bash
truncate -s 50 file.txt
```

Check:

```bash
ls -l file.txt
```

Output:

```
50 bytes
```

---

# 5. Relative Size

## Add 10 bytes

```bash
truncate -s +10 file.txt
```

---

## Remove 5 bytes

```bash
truncate -s -5 file.txt
```

---

# Size Units

| Unit | Meaning |
|------|----------|
| K | Kilobytes |
| M | Megabytes |
| G | Gigabytes |
| T | Terabytes |

---

## Create a 1 KB File

```bash
truncate -s 1K test.txt
```

---

## Create a 5 MB File

```bash
truncate -s 5M data.bin
```

---

## Create a 1 GB File

```bash
truncate -s 1G disk.img
```

---

# Practical Examples

## Empty a Log File

Instead of deleting:

```bash
rm server.log
```

Use:

```bash
truncate -s 0 server.log
```

The file remains but becomes empty.

---

## Clear Apache Log

```bash
truncate -s 0 /var/log/apache2/access.log
```

---

## Create Test File

```bash
truncate -s 100M testfile.img
```

---

## Reduce Backup File

```bash
truncate -s 50M backup.img
```

---

# Difference Between truncate and rm

| truncate | rm |
|-----------|-----|
| Empties or resizes file | Deletes file |
| File remains | File removed |
| Keeps permissions | Removes permissions with file |

Example:

```bash
truncate -s 0 log.txt
```

vs

```bash
rm log.txt
```

---

# Difference Between truncate and touch

| touch | truncate |
|--------|-----------|
| Create/update timestamp | Change file size |
| Doesn't clear content | Can clear content |

Example:

Create file:

```bash
touch file.txt
```

Empty file:

```bash
truncate -s 0 file.txt
```

---

# Real-Life Use Cases

## Clear log files

```bash
truncate -s 0 server.log
```

---

## Create virtual disk image

```bash
truncate -s 2G disk.img
```

---

## Create test data file

```bash
truncate -s 500M test.bin
```

---

## Resize a file

```bash
truncate -s 100K sample.txt
```

---

# Common Options

| Option | Meaning |
|---------|----------|
| -s | Set size |
| + | Increase size |
| - | Decrease size |

---

# Interview Questions

## Q1. What is truncate?

A Linux command used to change the size of a file.

---

## Q2. What does this command do?

```bash
truncate -s 0 file.txt
```

Makes the file empty without deleting it.

---

## Q3. What does this command do?

```bash
truncate -s 1M file.txt
```

Sets the file size to 1 MB.

---

## Q4. How do you increase a file size?

```bash
truncate -s +10K file.txt
```

---

## Q5. How do you decrease a file size?

```bash
truncate -s -5K file.txt
```

---

# Common Commands Cheat Sheet

```bash
truncate -s 0 file.txt

truncate -s 10 file.txt

truncate -s 1K file.txt

truncate -s 5M file.txt

truncate -s 1G file.txt

truncate -s +10K file.txt

truncate -s -5K file.txt

truncate -s 100M disk.img

truncate -s 2G virtual.img
```

# Memory Trick

```
truncate = Change file size

-s 0     -> Empty file
-s 1K    -> 1 KB
-s 5M    -> 5 MB
-s 1G    -> 1 GB

+s       -> Increase size
-s       -> Decrease size

truncate keeps the file,
rm removes the file.
```

# Quick Revision

| Command | Purpose |
|----------|----------|
| `truncate -s 0 file.txt` | Empty file |
| `truncate -s 1K file.txt` | Set size to 1 KB |
| `truncate -s 5M file.txt` | Set size to 5 MB |
| `truncate -s +10K file.txt` | Increase size |
| `truncate -s -5K file.txt` | Decrease size |
| `truncate -s 1G disk.img` | Create 1 GB file |

## Interview Tip

A very common Linux administration task is clearing log files without deleting them:

```bash
truncate -s 0 /var/log/syslog
```

This empties the log file while preserving the file, its permissions, and any application using it.