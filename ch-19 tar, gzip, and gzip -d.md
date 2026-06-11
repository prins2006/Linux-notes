# Linux `tar`, `gzip`, and `gzip -d` Commands - Complete Guide

## Introduction

In Linux, `tar` and `gzip` are used for file archiving and compression.

### Difference

| Command   | Purpose                                  |
| --------- | ---------------------------------------- |
| `tar`     | Combines multiple files into one archive |
| `gzip`    | Compresses a file to reduce size         |
| `gzip -d` | Decompresses a `.gz` file                |

**Important:**

* `tar` = Archive
* `gzip` = Compress
* `tar + gzip` = Archive and compress together

---

# 1. tar Command

## What is tar?

`tar` stands for **Tape Archive**.

It combines multiple files and directories into a single archive file.

By default, `tar` does not compress files.

---

# Syntax

```bash
tar [OPTION] archive_name files
```

---

# Create Sample Files

```bash
echo "Linux" > file1.txt
echo "Git" > file2.txt
echo "DevOps" > file3.txt
```

Check:

```bash
ls
```

Output:

```
file1.txt
file2.txt
file3.txt
```

---

# Create Archive (-c)

```bash
tar -cvf backup.tar file1.txt file2.txt file3.txt
```

Options:

| Option | Meaning          |
| ------ | ---------------- |
| -c     | Create archive   |
| -v     | Verbose output   |
| -f     | Archive filename |

Output:

```
file1.txt
file2.txt
file3.txt
```

---

# View Archive Contents (-t)

```bash
tar -tvf backup.tar
```

Output:

```
-rw-r--r--
-rw-r--r--
-rw-r--r--
```

---

# Extract Archive (-x)

```bash
tar -xvf backup.tar
```

Options:

| Option | Meaning |
| ------ | ------- |
| -x     | Extract |
| -v     | Verbose |
| -f     | Archive |

---

# Archive Directory

```bash
tar -cvf project.tar project/
```

---

# Extract to Specific Directory

```bash
tar -xvf backup.tar -C /tmp
```

---

# Common tar Options

| Option | Meaning              |
| ------ | -------------------- |
| -c     | Create               |
| -x     | Extract              |
| -t     | List contents        |
| -v     | Verbose              |
| -f     | Archive file         |
| -C     | Extract to directory |

---

# 2. gzip Command

## What is gzip?

`gzip` compresses files to save disk space.

Compressed files get the `.gz` extension.

---

# Syntax

```bash
gzip filename
```

---

# Compress File

Create:

```bash
echo "Linux Training" > notes.txt
```

Compress:

```bash
gzip notes.txt
```

Check:

```bash
ls
```

Output:

```
notes.txt.gz
```

Original file is replaced by the compressed file.

---

# Compress Multiple Files

```bash
gzip file1.txt file2.txt file3.txt
```

Output:

```
file1.txt.gz
file2.txt.gz
file3.txt.gz
```

---

# Keep Original File (-k)

```bash
gzip -k notes.txt
```

Output:

```
notes.txt
notes.txt.gz
```

---

# Display Compression Info (-l)

```bash
gzip -l notes.txt.gz
```

Shows:

* Compressed size
* Original size
* Compression ratio

---

# 3. gzip -d Command

## What is gzip -d?

`gzip -d` decompresses a `.gz` file.

Equivalent command:

```bash
gunzip filename.gz
```

---

# Syntax

```bash
gzip -d filename.gz
```

---

# Example

Compressed file:

```
notes.txt.gz
```

Decompress:

```bash
gzip -d notes.txt.gz
```

Output:

```
notes.txt
```

---

# Using gunzip

```bash
gunzip notes.txt.gz
```

Same result.

---

# tar and gzip Together

Most common Linux practice:

Create compressed archive:

```bash
tar -czvf backup.tar.gz file1.txt file2.txt
```

Options:

| Option | Meaning  |
| ------ | -------- |
| -c     | Create   |
| -z     | Use gzip |
| -v     | Verbose  |
| -f     | Filename |

Output:

```
backup.tar.gz
```

---

# Extract tar.gz File

```bash
tar -xzvf backup.tar.gz
```

Options:

| Option | Meaning  |
| ------ | -------- |
| -x     | Extract  |
| -z     | Use gzip |
| -v     | Verbose  |
| -f     | Archive  |

---

# Practical Examples

## Backup Home Directory

```bash
tar -czvf home_backup.tar.gz ~/Documents
```

---

## Extract Backup

```bash
tar -xzvf home_backup.tar.gz
```

---

## Compress Log File

```bash
gzip server.log
```

---

## Decompress Log File

```bash
gzip -d server.log.gz
```

---

## Compress Multiple Files

```bash
gzip *.txt
```

---

## Decompress Multiple Files

```bash
gunzip *.gz
```

---

# Difference Between tar and gzip

| tar                       | gzip             |
| ------------------------- | ---------------- |
| Archives files            | Compresses files |
| No compression by default | Compression only |
| Creates .tar              | Creates .gz      |
| Multiple files            | Single file      |

---

# Real-Life Examples

## Backup Project

```bash
tar -czvf project.tar.gz project/
```

---

## Extract Backup

```bash
tar -xzvf project.tar.gz
```

---

## Compress Database Dump

```bash
gzip database.sql
```

---

## Restore Database Dump

```bash
gzip -d database.sql.gz
```

---

# Interview Questions

## Q1. What is tar?

A command used to create and extract archive files.

---

## Q2. What is gzip?

A command used to compress files.

---

## Q3. What does gzip -d do?

It decompresses a `.gz` file.

Example:

```bash
gzip -d file.txt.gz
```

---

## Q4. What does tar -czvf do?

Creates a compressed `.tar.gz` archive.

---

## Q5. What does tar -xzvf do?

Extracts a compressed `.tar.gz` archive.

---

## Q6. What is the difference between tar and gzip?

* tar = Archive multiple files.
* gzip = Compress a file.

---

# Common Commands Cheat Sheet

```bash
tar -cvf backup.tar files

tar -tvf backup.tar

tar -xvf backup.tar

tar -cvf project.tar project/

tar -xvf backup.tar -C /tmp

gzip file.txt

gzip -k file.txt

gzip -l file.txt.gz

gzip -d file.txt.gz

gunzip file.txt.gz

tar -czvf backup.tar.gz files

tar -xzvf backup.tar.gz
```

# Memory Trick

```
tar

-c = Create
-x = Extract
-t = List
-v = Verbose
-f = File
-z = gzip

gzip

Compress:
gzip file.txt

Decompress:
gzip -d file.txt.gz

gunzip file.txt.gz

Create compressed archive:
tar -czvf backup.tar.gz files

Extract compressed archive:
tar -xzvf backup.tar.gz
```

# Quick Revision

| Command           | Purpose                    |
| ----------------- | -------------------------- |
| `tar -cvf`        | Create archive             |
| `tar -tvf`        | List archive               |
| `tar -xvf`        | Extract archive            |
| `gzip file`       | Compress file              |
| `gzip -d file.gz` | Decompress file            |
| `gunzip file.gz`  | Decompress file            |
| `tar -czvf`       | Create compressed archive  |
| `tar -xzvf`       | Extract compressed archive |

## Interview Tip

Remember the sequence:

```
tar  -> Archive
gzip -> Compress
tar + gzip -> Archive and Compress

Create:
tar -czvf backup.tar.gz folder/

Extract:
tar -xzvf backup.tar.gz
```

This is one of the most commonly used Linux backup commands by system administrators and DevOps engineers.
