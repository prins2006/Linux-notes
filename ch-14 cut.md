# Linux `cut` Command

## What is `cut`?

The `cut` command is a Linux text-processing utility used to extract specific parts of each line from a file or command output.

It can extract:
- Characters
- Bytes
- Fields (columns)

## Syntax

```bash
cut OPTION... [FILE]
```

---

# 1. Character Mode (`-c`)

Extract specific character positions.

## Syntax

```bash
cut -c LIST filename
```

### Example File

Create:

```bash
echo "Linux" > feature.txt
echo "Ubuntu" >> feature.txt
echo "GitHub" >> feature.txt
echo "DevOps" >> feature.txt
```

File content:

```
Linux
Ubuntu
GitHub
DevOps
```

---

## Example 1: First Character

```bash
cut -c1 feature.txt
```

Output:

```
L
U
G
D
```

---

## Example 2: First Three Characters

```bash
cut -c1-3 feature.txt
```

Output:

```
Lin
Ubu
Git
Dev
```

---

## Example 3: Characters 2 to 5

```bash
cut -c2-5 feature.txt
```

Output:

```
inux
bunt
itHu
evOp
```

---

## Example 4: Specific Characters

```bash
cut -c1,3,5 feature.txt
```

Output:

```
Lnx
Uut
Gtb
Dvp
```

---

## Example 5: Third Character to End

```bash
cut -c3- feature.txt
```

Output:

```
nux
untu
tHub
vOps
```

---

# 2. Byte Mode (`-b`)

Extract bytes.

Syntax:

```bash
cut -b LIST filename
```

Example:

```bash
cut -b1-4 feature.txt
```

Output:

```
Linu
Ubun
GitH
DevO
```

Usually similar to -c for ASCII text.

---

# 3. Field Mode (`-f`)

Extract fields separated by a delimiter.

---

## Example File

Create:

```bash
cat > users.txt
john:1001:admin
alice:1002:user
bob:1003:developer
Ctrl+D
```

Content:

```
john:1001:admin
alice:1002:user
bob:1003:developer
```

---

## First Field

```bash
cut -d':' -f1 users.txt
```

Output:

```
john
alice
bob
```

---

## Second Field

```bash
cut -d':' -f2 users.txt
```

Output:

```
1001
1002
1003
```

---

## Third Field

```bash
cut -d':' -f3 users.txt
```

Output:

```
admin
user
developer
```

---

# 4. Delimiter Option (`-d`)

Specifies the separator.

Syntax:

```bash
cut -d'DELIMITER' -fFIELD file
```

Example:

```
john:1001:admin
```

Delimiter:

```
:
```

Fields:

```
1=john
2=1001
3=admin
```

Command:

```bash
cut -d':' -f2 users.txt
```

Output:

```
1001
1002
1003
```

---

# 5. Multiple Fields

```bash
cut -d':' -f1,3 users.txt
```

Output:

```
john:admin
alice:user
bob:developer
```

---

# 6. Range of Fields

```bash
cut -d':' -f1-2 users.txt
```

Output:

```
john:1001
alice:1002
bob:1003
```

---

# 7. From Field to End

```bash
cut -d':' -f2-
```

Output:

```
1001:admin
1002:user
1003:developer
```

---

# Practical Examples

## View usernames

```bash
cut -d':' -f1 /etc/passwd
```

---

## View user IDs

```bash
cut -d':' -f3 /etc/passwd
```

---

## CSV File

students.csv

```
John,90,A
Alice,95,A+
Bob,85,B
```

First field:

```bash
cut -d',' -f1 students.csv
```

Output:

```
John
Alice
Bob
```

Second field:

```bash
cut -d',' -f2 students.csv
```

Output:

```
90
95
85
```

---

# Use with Pipes

## Directory Listing

```bash
ls -l | cut -c1-10
```

Shows file permissions.

---

## Current User

```bash
who | cut -d' ' -f1
```

---

## Process IDs

```bash
ps -ef | cut -c1-20
```

---

# Common Options

| Option | Meaning |
|---------|----------|
| -c | Select characters |
| -b | Select bytes |
| -d | Delimiter |
| -f | Fields |
| --complement | Print everything except selected parts |
| -s | Skip lines without delimiter |

---

# Advanced Options

## Skip lines without delimiter

```bash
cut -s -d':' -f1 file.txt
```

---

## Complement

Suppose:

```
A:B:C
```

Command:

```bash
cut --complement -d':' -f2
```

Output:

```
A:C
```

---

# Difference Between -c and -f

| -c | -f |
|----|----|
| Character position | Field position |
| No delimiter needed | Delimiter required |
| Fixed width data | Structured data |

---

# Real-Life Examples

## Get usernames

```bash
cut -d':' -f1 /etc/passwd
```

## Get shells

```bash
cut -d':' -f7 /etc/passwd
```

## Get first column of CSV

```bash
cut -d',' -f1 students.csv
```

## Show permissions

```bash
ls -l | cut -c1-10
```

---

# Interview Questions

## Q1. What is the cut command?

It extracts selected portions of text from each line of a file.

---

## Q2. What does -c do?

Selects characters.

Example:

```bash
cut -c1-5 file.txt
```

---

## Q3. What does -d do?

Specifies the delimiter.

Example:

```bash
cut -d':' -f1 file.txt
```

---

## Q4. What does -f do?

Selects fields separated by the delimiter.

---

## Q5. Difference between -c and -f?

-c works with character positions.

-f works with delimiter-separated fields.

---

# Common Commands Cheat Sheet

```bash
cut -c1 file.txt
cut -c1-5 file.txt
cut -c3- file.txt
cut -c1,3,5 file.txt

cut -d':' -f1 file.txt
cut -d':' -f2 file.txt
cut -d':' -f1,3 file.txt
cut -d':' -f2- file.txt

cut -d',' -f1 students.csv

cut -s -d':' -f1 file.txt

cut --complement -d':' -f2 file.txt
```

# Memory Trick

```
-c  = Characters
-b  = Bytes
-d  = Delimiter
-f  = Fields
-s  = Skip lines without delimiter
--complement = Everything except selected fields

cut = Cut selected parts of text.
```