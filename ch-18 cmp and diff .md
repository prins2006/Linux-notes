# Linux `cmp` and `diff` Commands - Complete Guide

## Introduction

The `cmp` and `diff` commands are used to compare two files.

* **cmp** → Compares files byte by byte.
* **diff** → Compares files line by line.

These commands are commonly used by Linux administrators, developers, and DevOps engineers.

---

# 1. `cmp` Command

## What is `cmp`?

The `cmp` command compares two files byte by byte and reports the first difference.

## Syntax

```bash
cmp [OPTION] file1 file2
```

---

## Create Sample Files

### file1.txt

```text
Linux
Ubuntu
Git
DevOps
```

### file2.txt

```text
Linux
Ubuntu
GitHub
DevOps
```

---

## Basic Comparison

```bash
cmp file1.txt file2.txt
```

Output:

```
file1.txt file2.txt differ: byte 18, line 3
```

Meaning:

* First difference at byte 18.
* Difference found on line 3.

---

## Compare Identical Files

```bash
cp file1.txt file3.txt

cmp file1.txt file3.txt
```

Output:

```
(No output)
```

No output means both files are identical.

---

## Show All Differences (-l)

```bash
cmp -l file1.txt file2.txt
```

Output:

```
18 40 110
19 12 111
```

Shows every differing byte.

---

## Silent Mode (-s)

```bash
cmp -s file1.txt file2.txt
echo $?
```

Output:

```
1
```

Exit codes:

| Code | Meaning             |
| ---- | ------------------- |
| 0    | Files are identical |
| 1    | Files differ        |
| 2    | Error               |

---

# Common cmp Options

| Option | Meaning                  |
| ------ | ------------------------ |
| -l     | Show all differing bytes |
| -s     | Silent mode              |

---

# 2. `diff` Command

## What is diff?

The `diff` command compares two files line by line.

It shows which lines were added, removed, or changed.

---

## Syntax

```bash
diff file1 file2
```

---

## Example

file1.txt

```
Linux
Ubuntu
Git
DevOps
```

file2.txt

```
Linux
Ubuntu
GitHub
DevOps
Docker
```

Command:

```bash
diff file1.txt file2.txt
```

Output:

```
3c3
< Git
---
> GitHub

4a5
> Docker
```

Meaning:

* Line 3 changed.
* One new line added after line 4.

---

# Side-by-Side Comparison (-y)

```bash
diff -y file1.txt file2.txt
```

Output:

```
Linux      Linux
Ubuntu     Ubuntu
Git      | GitHub
DevOps     DevOps
          > Docker
```

---

# Ignore Case (-i)

file1:

```
Linux
```

file2:

```
linux
```

```bash
diff -i file1 file2
```

Treats uppercase and lowercase as the same.

---

# Ignore Blank Spaces (-b)

```bash
diff -b file1 file2
```

Ignores extra spaces.

---

# Brief Output (-q)

```bash
diff -q file1.txt file2.txt
```

Output:

```
Files file1.txt and file2.txt differ
```

---

# Recursive Directory Comparison (-r)

Compare two directories:

```bash
diff -r dir1 dir2
```

---

# Unified Format (-u)

Commonly used with Git.

```bash
diff -u file1.txt file2.txt
```

Example:

```
--- file1.txt
+++ file2.txt

-Git
+GitHub
```

---

# Practical Examples

## Compare two configuration files

```bash
diff nginx1.conf nginx2.conf
```

---

## Compare two directories

```bash
diff -r folder1 folder2
```

---

## Compare two scripts

```bash
cmp script1.sh script2.sh
```

---

## Compare backups

```bash
diff backup1.txt backup2.txt
```

---

# Difference Between cmp and diff

| cmp                    | diff                       |
| ---------------------- | -------------------------- |
| Byte by byte           | Line by line               |
| Shows first difference | Shows all line differences |
| Good for binary files  | Good for text files        |
| Simple output          | Detailed output            |

---

# Real-Life Use Cases

## Compare configuration files

```bash
diff config_old.conf config_new.conf
```

---

## Compare source code

```bash
diff main_v1.c main_v2.c
```

---

## Check backup files

```bash
cmp backup1.tar backup2.tar
```

---

## Compare project directories

```bash
diff -r project1 project2
```

---

# Interview Questions

## Q1. What is cmp?

Compares two files byte by byte.

Example:

```bash
cmp file1 file2
```

---

## Q2. What is diff?

Compares two files line by line.

Example:

```bash
diff file1 file2
```

---

## Q3. Which command is better for text files?

`diff`

---

## Q4. Which command is better for binary files?

`cmp`

---

## Q5. What does `diff -y` do?

Displays differences side by side.

---

## Q6. What does `diff -r` do?

Compares directories recursively.

---

# Common Commands Cheat Sheet

```bash
cmp file1 file2

cmp -l file1 file2

cmp -s file1 file2

diff file1 file2

diff -y file1 file2

diff -i file1 file2

diff -b file1 file2

diff -q file1 file2

diff -u file1 file2

diff -r dir1 dir2
```

# Memory Trick

```
cmp  = Compare bytes
diff = Compare lines

cmp  -> Binary files
diff -> Text files

cmp -s = Silent
diff -q = Brief
diff -y = Side by side
diff -u = Unified format
diff -r = Recursive directories
```

## Quick Revision

| Command            | Purpose                   |
| ------------------ | ------------------------- |
| `cmp file1 file2`  | Byte-by-byte comparison   |
| `cmp -l`           | Show all byte differences |
| `cmp -s`           | Silent comparison         |
| `diff file1 file2` | Line-by-line comparison   |
| `diff -y`          | Side-by-side output       |
| `diff -q`          | Brief result              |
| `diff -u`          | Unified format            |
| `diff -r`          | Compare directories       |

### Interview Tip

* **`cmp`** tells you **whether files differ and where the first byte difference occurs**.
* **`diff`** tells you **what lines have changed, been added, or removed**, making it the preferred tool for comparing text files and source code.
