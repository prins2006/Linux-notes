# Linux `uniq` Command - Complete Guide

## What is `uniq`?

The `uniq` command in Linux is used to **remove or identify duplicate consecutive lines** in a file or command output.

> **Important:** `uniq` only works correctly on **adjacent (consecutive) duplicate lines**. For unsorted files, use `sort` before `uniq`.

---

# Syntax

```bash
uniq [OPTION] [INPUT_FILE] [OUTPUT_FILE]
```

Example:

```bash
uniq file.txt
```

---

# Sample File

Create a file:

```bash
cat > fruits.txt
```

Enter:

```
Apple
Apple
Banana
Banana
Banana
Orange
Mango
Mango
```

Press:

```
Ctrl+D
```

File:

```
Apple
Apple
Banana
Banana
Banana
Orange
Mango
Mango
```

---

# 1. Basic uniq

```bash
uniq fruits.txt
```

Output:

```
Apple
Banana
Orange
Mango
```

Removes adjacent duplicate lines.

---

# 2. Count Duplicate Lines (-c)

```bash
uniq -c fruits.txt
```

Output:

```
2 Apple
3 Banana
1 Orange
2 Mango
```

Shows how many times each consecutive line appears.

---

# 3. Show Only Duplicate Lines (-d)

```bash
uniq -d fruits.txt
```

Output:

```
Apple
Banana
Mango
```

Only duplicated lines are displayed.

---

# 4. Show Only Unique Lines (-u)

```bash
uniq -u fruits.txt
```

Output:

```
Orange
```

Displays lines that appear exactly once.

---

# 5. Ignore Case (-i)

Create:

```
Apple
apple
APPLE
Banana
BANANA
```

Command:

```bash
uniq -i file.txt
```

Treats uppercase and lowercase as the same.

Usually used with sort:

```bash
sort -f file.txt | uniq -i
```

---

# 6. Skip Characters (-s)

File:

```
001John
002John
003Alice
004Alice
```

Skip first 3 characters:

```bash
uniq -s3 file.txt
```

Compares only after the first 3 characters.

---

# 7. Compare First N Characters (-w)

File:

```
Apple123
Apple456
Banana789
Banana111
```

Compare first 5 characters:

```bash
uniq -w5 file.txt
```

---

# Important: uniq vs sort

Example:

unsorted.txt

```
Apple
Banana
Apple
Orange
Banana
```

Run:

```bash
uniq unsorted.txt
```

Output:

```
Apple
Banana
Apple
Orange
Banana
```

No duplicates removed because they are not adjacent.

Correct:

```bash
sort unsorted.txt | uniq
```

Output:

```
Apple
Banana
Orange
```

---

# Practical Examples

## Remove duplicate usernames

```bash
cut -d':' -f1 users.txt | sort | uniq
```

---

## Count duplicate usernames

```bash
cut -d':' -f1 users.txt | sort | uniq -c
```

---

## Find duplicate entries

```bash
sort users.txt | uniq -d
```

---

## Find unique entries

```bash
sort users.txt | uniq -u
```

---

# Pipe Examples

## Directory Listing

```bash
ls | sort | uniq
```

---

## Command History

```bash
history | sort | uniq
```

---

## User Names

```bash
cat /etc/passwd | cut -d':' -f1 | sort | uniq
```

---

# Common Options

| Option | Meaning |
|---------|----------|
| -c | Count occurrences |
| -d | Show duplicates |
| -u | Show unique lines |
| -i | Ignore case |
| -s N | Skip first N characters |
| -w N | Compare first N characters |

---

# Difference Between sort and uniq

| sort | uniq |
|-------|------|
| Sorts lines | Removes adjacent duplicates |
| Alphabetical order | Requires sorted input |
| Can work alone | Usually used with sort |

---

# Difference Between cut, sort, and uniq

| Command | Purpose |
|----------|----------|
| cut | Extract fields or characters |
| sort | Arrange lines |
| uniq | Remove or count duplicates |

Example:

```bash
cat users.txt | cut -d':' -f1 | sort | uniq
```

Steps:

1. cut → Extract usernames.
2. sort → Arrange alphabetically.
3. uniq → Remove duplicates.

---

# Real-Life Examples

## Remove duplicate names

```bash
sort names.txt | uniq
```

---

## Count duplicate names

```bash
sort names.txt | uniq -c
```

---

## Show duplicate records

```bash
sort names.txt | uniq -d
```

---

## Show unique records

```bash
sort names.txt | uniq -u
```

---

# Interview Questions

## Q1. What is uniq?

A Linux command that removes or identifies adjacent duplicate lines.

---

## Q2. Why is sort often used with uniq?

Because uniq only removes consecutive duplicate lines.

Example:

```bash
sort file.txt | uniq
```

---

## Q3. What does -c do?

Counts occurrences.

```bash
uniq -c file.txt
```

---

## Q4. What does -d do?

Shows only duplicate lines.

```bash
uniq -d file.txt
```

---

## Q5. What does -u do?

Shows only unique lines.

```bash
uniq -u file.txt
```

---

# Common Commands Cheat Sheet

```bash
uniq file.txt

uniq -c file.txt

uniq -d file.txt

uniq -u file.txt

uniq -i file.txt

uniq -s3 file.txt

uniq -w5 file.txt

sort file.txt | uniq

sort file.txt | uniq -c

sort file.txt | uniq -d

sort file.txt | uniq -u

cut -d':' -f1 /etc/passwd | sort | uniq
```

# Memory Trick

```
uniq = Unique lines

-c = Count
-d = Duplicates
-u = Unique only
-i = Ignore case
-s = Skip characters
-w = Compare first N characters

Important:

uniq works on consecutive duplicates.

Best practice:

sort file.txt | uniq
```

## Quick Example

File:

```
Apple
Apple
Banana
Orange
Orange
```

Command:

```bash
uniq -c fruits.txt
```

Output:

```
2 Apple
1 Banana
2 Orange
```

**Most common interview command:**

```bash
sort file.txt | uniq -c
```

This sorts the file and counts how many times each line appears.