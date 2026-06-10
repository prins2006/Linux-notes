# Linux Commands: `echo` and `cat`

## 1. `echo` Command

### Definition
The `echo` command is used to display text or variables on the terminal and write text to files.

### Syntax

```bash
echo [option] [text]
```

### Examples

#### Print text

```bash
echo "Hello Linux"
```

**Output:**

```
Hello Linux
```

#### Display a variable

```bash
name="Prins"
echo $name
```

**Output:**

```
Prins
```

#### Write to a file

```bash
echo "Hello Linux" > file.txt
```

#### Append to a file

```bash
echo "Learning Linux" >> file.txt
```

### Common Options

| Option | Description |
|--------|-------------|
| `-n` | Do not print a new line |
| `-e` | Enable escape characters |

Example:

```bash
echo -e "Linux\nUbuntu"
```

Output:

```
Linux
Ubuntu
```

---

# 2. `cat` Command

## Definition

`cat` stands for **Concatenate**.

It is used to:
- Display file contents
- Create files
- Append text
- Merge files

## Syntax

```bash
cat filename
```

## Examples

### Display a file

```bash
cat file.txt
```

### Create a file

```bash
cat > notes.txt
```

Type the content and press:

```
Ctrl + D
```

### Append to a file

```bash
cat >> notes.txt
```

Press:

```
Ctrl + D
```

### Display line numbers

```bash
cat -n file.txt
```

### Merge files

```bash
cat file1.txt file2.txt > file3.txt
```

---

# Difference Between `echo` and `cat`

| Feature | echo | cat |
|----------|:----:|:---:|
| Print text | ✅ | ❌ |
| Display file contents | ❌ | ✅ |
| Create files | ✅ | ✅ |
| Append text | ✅ | ✅ |
| Merge files | ❌ | ✅ |

---

# Practice Commands

```bash
echo "Hello Linux"

echo "Hello Linux" > file.txt

echo "Welcome" >> file.txt

cat file.txt

cat -n file.txt

cat file1.txt file2.txt > merged.txt
```

---

# Interview Questions

## What is the `echo` command?

The `echo` command prints text or variables to the terminal and can write text to files.

## What is the `cat` command?

The `cat` command displays, creates, appends, and combines file contents.

## What is the difference between `>` and `>>`?

- `>` Overwrites a file.
- `>>` Appends to a file.

---

# Quick Revision

## echo

```bash
echo Hello
echo $USER
echo "Text" > file.txt
echo "More" >> file.txt
```

## cat

```bash
cat file.txt
cat > file.txt
cat >> file.txt
cat -n file.txt
cat file1.txt file2.txt > merged.txt
```

## Memory Tip

- **echo = Print text**
- **cat = Read and combine files**