# Linux `tee` Command

## What is the `tee` Command?

The `tee` command reads input from a command or keyboard and writes it to:
1. The terminal (screen).
2. One or more files simultaneously.

### Syntax

```bash
command | tee [OPTION] filename
```

or

```bash
tee filename
```

---

# Why is it called `tee`?

It is named after the **T-shaped pipe fitting** because it splits one input into two outputs.

```
Input
  |
  v
 tee
 /  \
v    v
Screen File
```

---

# Basic Example

```bash
echo "Hello Linux" | tee file.txt
```

## Output on Terminal

```
Hello Linux
```

## File Content

```bash
cat file.txt
```

Output:

```
Hello Linux
```

---

# Create a File

```bash
tee notes.txt
```

Type:

```
Linux
Unix
Ubuntu
```

Press:

```
Ctrl + D
```

Check:

```bash
cat notes.txt
```

Output:

```
Linux
Unix
Ubuntu
```

---

# Overwrite a File

```bash
echo "New Data" | tee file.txt
```

Previous content is replaced.

---

# Append to a File

Use the `-a` option.

```bash
echo "Linux" > notes.txt

echo "Ubuntu" | tee -a notes.txt
```

Check:

```bash
cat notes.txt
```

Output:

```
Linux
Ubuntu
```

---

# Write to Multiple Files

```bash
echo "Hello" | tee file1.txt file2.txt
```

Check:

```bash
cat file1.txt
cat file2.txt
```

Output:

```
Hello
Hello
```

---

# Use with `ls`

```bash
ls | tee files.txt
```

Displays the file list on the terminal and saves it to `files.txt`.

---

# Use with `date`

```bash
date | tee log.txt
```

Example Output:

```
Wed Jun 10 10:30:00 IST 2026
```

---

# Use with `pwd`

```bash
pwd | tee path.txt
```

Displays the current directory and saves it.

---

# Use with `grep`

```bash
cat file.txt | grep Linux | tee result.txt
```

Displays matching lines and stores them in `result.txt`.

---

# Ignore Terminal Output

```bash
echo "Hello" | tee file.txt > /dev/null
```

The text is saved to the file but not displayed.

---

# Real-World Examples

## Save command output

```bash
df -h | tee disk.txt
```

## Save running processes

```bash
ps -ef | tee process.txt
```

## Save system information

```bash
uname -a | tee system.txt
```

## Save network configuration

```bash
ip addr | tee network.txt
```

---

# Common Options

| Option | Description |
|---------|-------------|
| `-a` | Append to file |
| `-i` | Ignore interrupt signals |
| `--help` | Display help |
| `--version` | Show version |

---

# Difference Between `>` and `tee`

| Feature | `>` | `tee` |
|----------|-----|-------|
| Save to file | ✅ | ✅ |
| Display on terminal | ❌ | ✅ |
| Append option | `>>` | `tee -a` |
| Multiple files | ❌ | ✅ |

---

# Example Comparison

## Using `>`

```bash
echo "Hello Linux" > file.txt
```

Output:

```
Nothing displayed.
```

File:

```
Hello Linux
```

---

## Using `tee`

```bash
echo "Hello Linux" | tee file.txt
```

Terminal:

```
Hello Linux
```

File:

```
Hello Linux
```

---

# Practice Commands

```bash
echo "Linux" | tee file.txt

echo "Ubuntu" | tee -a file.txt

ls | tee files.txt

date | tee log.txt

pwd | tee path.txt

echo "Hello" | tee file1.txt file2.txt

cat file.txt | grep Linux | tee result.txt
```

---

# Interview Questions

## What is the `tee` command?

The `tee` command reads input and writes it to both the terminal and one or more files.

## What does `tee -a` do?

It appends data to a file instead of overwriting it.

## Can `tee` write to multiple files?

Yes.

Example:

```bash
echo "Hello" | tee file1.txt file2.txt
```

## What is the difference between `>` and `tee`?

- `>` writes only to a file.
- `tee` writes to both the terminal and file.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `tee file.txt` | Write to terminal and file |
| `tee -a file.txt` | Append to file |
| `ls | tee files.txt` | Save directory listing |
| `date | tee log.txt` | Save current date |
| `echo "Hello" | tee file1 file2` | Write to multiple files |

## Memory Trick

- **`>` = Save only to a file.**
- **`tee` = Save to a file and show on the screen at the same time.**

### Visual Flow

```
Command Output
      |
      v
    tee
   /   \
  v     v
Screen  File
```