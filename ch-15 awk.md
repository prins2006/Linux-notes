# Linux `awk` Command - Complete Guide

## What is `awk`?

`awk` is a powerful text-processing tool in Linux. It is used to:
- Search for patterns
- Extract columns (fields)
- Filter data
- Perform calculations
- Generate reports

`awk` reads input line by line and processes each line according to specified rules.

---

# Syntax

```bash
awk 'pattern { action }' filename
```

- **pattern** : Condition to match.
- **action** : Operation to perform.

Example:

```bash
awk '{print}' file.txt
```

Prints every line.

---

# Sample File

Create a file:

```bash
cat > employee.txt
```

Enter:

```
101 John IT 50000
102 Alice HR 45000
103 Bob Sales 40000
104 David IT 55000
```

Press:

```
Ctrl+D
```

---

# Print Entire File

```bash
awk '{print}' employee.txt
```

Output:

```
101 John IT 50000
102 Alice HR 45000
103 Bob Sales 40000
104 David IT 55000
```

---

# Print First Column

```bash
awk '{print $1}' employee.txt
```

Output:

```
101
102
103
104
```

---

# Print Second Column

```bash
awk '{print $2}' employee.txt
```

Output:

```
John
Alice
Bob
David
```

---

# Print Multiple Columns

```bash
awk '{print $1,$2}' employee.txt
```

Output:

```
101 John
102 Alice
103 Bob
104 David
```

---

# Print Last Column

```bash
awk '{print $NF}' employee.txt
```

Output:

```
50000
45000
40000
55000
```

`NF` means Number of Fields.

---

# Print Line Number

```bash
awk '{print NR,$0}' employee.txt
```

Output:

```
1 101 John IT 50000
2 102 Alice HR 45000
3 103 Bob Sales 40000
4 104 David IT 55000
```

- `NR` = Record Number
- `$0` = Entire line

---

# Search for a Pattern

Print IT department employees:

```bash
awk '/IT/' employee.txt
```

Output:

```
101 John IT 50000
104 David IT 55000
```

---

# Condition Example

Salary greater than 45000:

```bash
awk '$4>45000 {print $2,$4}' employee.txt
```

Output:

```
John 50000
David 55000
```

---

# Field Separator

Default separator is space.

For CSV files:

students.csv

```
John,90,A
Alice,95,A+
Bob,85,B
```

Print names:

```bash
awk -F',' '{print $1}' students.csv
```

Output:

```
John
Alice
Bob
```

Print marks:

```bash
awk -F',' '{print $2}' students.csv
```

Output:

```
90
95
85
```

---

# BEGIN Block

Print heading before processing:

```bash
awk 'BEGIN{print "Employee List"} {print $2}' employee.txt
```

Output:

```
Employee List
John
Alice
Bob
David
```

---

# END Block

Count records:

```bash
awk 'END{print NR}' employee.txt
```

Output:

```
4
```

---

# BEGIN and END Together

```bash
awk 'BEGIN{print "Start"} {print $2} END{print "End"}' employee.txt
```

Output:

```
Start
John
Alice
Bob
David
End
```

---

# Calculate Total Salary

```bash
awk '{sum+=$4} END{print sum}' employee.txt
```

Output:

```
190000
```

---

# Average Salary

```bash
awk '{sum+=$4} END{print sum/NR}' employee.txt
```

Output:

```
47500
```

---

# Practical Examples

## List usernames

```bash
awk -F':' '{print $1}' /etc/passwd
```

---

## Show login shell

```bash
awk -F':' '{print $7}' /etc/passwd
```

---

## Show disk usage

```bash
df -h | awk '{print $1,$5}'
```

---

## Show running processes

```bash
ps -ef | awk '{print $2,$8}'
```

---

## List files

```bash
ls -l | awk '{print $9}'
```

---

# Built-in Variables

| Variable | Meaning |
|----------|----------|
| $0 | Entire line |
| $1 | First field |
| $2 | Second field |
| $NF | Last field |
| NR | Record number |
| NF | Number of fields |

---

# Common Options

| Option | Purpose |
|---------|----------|
| -F | Field separator |
| -v | Pass variable |
| BEGIN | Before processing |
| END | After processing |

---

# Difference Between cut and awk

| cut | awk |
|------|------|
| Simple extraction | Advanced processing |
| Fixed fields | Conditional fields |
| No calculations | Supports calculations |
| Limited features | Powerful scripting |

---

# Interview Questions

## What is awk?

A text-processing utility used to search, extract, filter, and manipulate text.

---

## What does `$1` mean?

First field.

---

## What does `$0` mean?

Entire line.

---

## What does `NR` mean?

Current line number.

---

## What does `NF` mean?

Number of fields.

---

## What does `-F` do?

Sets the field separator.

Example:

```bash
awk -F':' '{print $1}' /etc/passwd
```

---

# Common Commands Cheat Sheet

```bash
awk '{print}' file

awk '{print $1}' file

awk '{print $1,$2}' file

awk '{print $NF}' file

awk '{print NR,$0}' file

awk '/pattern/' file

awk '$4>50000' file

awk -F':' '{print $1}' /etc/passwd

awk 'BEGIN{print "Start"}'

awk 'END{print NR}'

awk '{sum+=$4} END{print sum}'

awk '{sum+=$4} END{print sum/NR}'
```

# Memory Trick

```
$0  = Whole line
$1  = First field
$2  = Second field
$NF = Last field
NR  = Line number
NF  = Number of fields
-F  = Field separator

awk = Search + Filter + Extract + Calculate
```