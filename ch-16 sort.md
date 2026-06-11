# Linux `sort` Command - Complete Guide

## What is `sort`?

The `sort` command in Linux is used to **sort the lines of a text file or command output**.

It can sort:
- Alphabetically
- Numerically
- Reverse order
- By specific columns (fields)
- Remove duplicates
- Ignore case

---

# Syntax

```bash
sort [OPTION] filename
```

Example:

```bash
sort file.txt
```

---

# Create a Sample File

```bash
cat > names.txt
```

Enter:

```
John
Alice
David
Bob
Charlie
```

Press:

```
Ctrl+D
```

File content:

```
John
Alice
David
Bob
Charlie
```

---

# 1. Default Sort

```bash
sort names.txt
```

Output:

```
Alice
Bob
Charlie
David
John
```

Sorts alphabetically.

---

# 2. Reverse Sort (-r)

```bash
sort -r names.txt
```

Output:

```
John
David
Charlie
Bob
Alice
```

---

# 3. Numeric Sort (-n)

Create:

```bash
cat > numbers.txt
```

```
100
50
25
500
10
```

```bash
sort -n numbers.txt
```

Output:

```
10
25
50
100
500
```

---

# Without -n

```bash
sort numbers.txt
```

Output:

```
10
100
25
50
500
```

Alphabetical sorting.

---

# 4. Remove Duplicate Lines (-u)

Create:

```
Apple
Banana
Apple
Orange
Banana
```

```bash
sort -u fruits.txt
```

Output:

```
Apple
Banana
Orange
```

---

# 5. Ignore Case (-f)

Create:

```
apple
Banana
APPLE
banana
```

```bash
sort -f file.txt
```

Treats uppercase and lowercase as the same.

---

# 6. Check if File is Sorted (-c)

```bash
sort -c names.txt
```

If sorted:

No output.

If not sorted:

```
sort: names.txt:2: disorder: Alice
```

---

# 7. Sort by Month (-M)

Create:

```
Jan
Mar
Feb
Dec
Apr
```

```bash
sort -M months.txt
```

Output:

```
Jan
Feb
Mar
Apr
Dec
```

---

# 8. Sort by Field (-k)

Create:

employees.txt

```
101 John
103 David
102 Alice
104 Bob
```

Sort by first field:

```bash
sort -k1 employees.txt
```

Output:

```
101 John
102 Alice
103 David
104 Bob
```

Sort by second field:

```bash
sort -k2 employees.txt
```

Output:

```
102 Alice
104 Bob
103 David
101 John
```

---

# 9. Numeric Field Sort

salary.txt

```
John 50000
Alice 45000
Bob 60000
```

```bash
sort -k2 -n salary.txt
```

Output:

```
Alice 45000
John 50000
Bob 60000
```

---

# 10. Reverse Numeric Sort

```bash
sort -k2 -nr salary.txt
```

Output:

```
Bob 60000
John 50000
Alice 45000
```

---

# Practical Examples

## Sort usernames

```bash
cut -d':' -f1 /etc/passwd | sort
```

---

## Sort and remove duplicates

```bash
sort -u names.txt
```

---

## Sort processes

```bash
ps -ef | sort
```

---

## Sort directory listing

```bash
ls | sort
```

---

## Sort command history

```bash
history | sort
```

---

# Common Options

| Option | Meaning |
|---------|----------|
| -n | Numeric sort |
| -r | Reverse order |
| -u | Unique |
| -f | Ignore case |
| -c | Check sorted |
| -k | Sort by field |
| -M | Month sort |
| -b | Ignore leading blanks |

---

# Combining Options

## Numeric Reverse

```bash
sort -nr numbers.txt
```

---

## Unique Reverse

```bash
sort -ur file.txt
```

---

## Field Numeric Reverse

```bash
sort -k2 -nr salary.txt
```

---

# Pipe Examples

## Sort usernames

```bash
cat /etc/passwd | cut -d':' -f1 | sort
```

---

## Sort unique usernames

```bash
cat /etc/passwd | cut -d':' -f1 | sort -u
```

---

## Sort files

```bash
ls -l | sort -k9
```

---

# Difference Between sort and uniq

| sort | uniq |
|-------|------|
| Sort lines | Remove adjacent duplicates |
| Can sort alphabetically | Cannot sort |
| Often used with uniq | Requires sorted input |

Example:

```
Apple
Banana
Apple
```

```bash
sort fruits.txt | uniq
```

Output:

```
Apple
Banana
```

---

# Real-Life Use Cases

## Sort employee records

```bash
sort employees.txt
```

## Sort marks

```bash
sort -n marks.txt
```

## Remove duplicate usernames

```bash
sort -u users.txt
```

## Sort by salary

```bash
sort -k2 -n salary.txt
```

---

# Interview Questions

## Q1. What is the sort command?

It sorts lines of text files or command output.

---

## Q2. What does -n do?

Numeric sorting.

```bash
sort -n numbers.txt
```

---

## Q3. What does -r do?

Reverse sorting.

```bash
sort -r names.txt
```

---

## Q4. What does -u do?

Removes duplicate lines.

```bash
sort -u file.txt
```

---

## Q5. What does -k do?

Sorts by a specific field.

```bash
sort -k2 employees.txt
```

---

## Q6. What does -M do?

Sorts month names.

```bash
sort -M months.txt
```

---

# Common Commands Cheat Sheet

```bash
sort file.txt

sort -r file.txt

sort -n numbers.txt

sort -nr numbers.txt

sort -u file.txt

sort -f file.txt

sort -c file.txt

sort -M months.txt

sort -k1 file.txt

sort -k2 file.txt

sort -k2 -n file.txt

sort -k2 -nr file.txt

cut -d':' -f1 /etc/passwd | sort

sort file.txt | uniq
```

# Memory Trick

```
sort = Arrange data

-n = Number
-r = Reverse
-u = Unique
-f = Ignore case
-k = Key (field)
-M = Month
-c = Check

sort + uniq = Sort and remove duplicates
```

## Quick Example

Suppose `marks.txt`:

```
Rahul 85
Amit 95
Priya 90
```

Sort by marks:

```bash
sort -k2 -n marks.txt
```

Output:

```
Rahul 85
Priya 90
Amit 95
```

This is a common Linux and DevOps interview example for the `sort` command.