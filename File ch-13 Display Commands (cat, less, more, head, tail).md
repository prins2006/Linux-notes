# Linux File Display Commands
## `cat`, `less`, `more`, `head`, `tail`

File display commands are used to view the contents of files without modifying them.

---

# 1. `cat` Command

## Definition

`cat` stands for **Concatenate**. It displays, creates, combines, and appends file contents.

## Syntax

```bash
cat filename
```

## Display a file

```bash
cat file.txt
```

Output:

```
Hello Linux
Welcome to Ubuntu
```

## Display multiple files

```bash
cat file1.txt file2.txt
```

## Display line numbers

```bash
cat -n file.txt
```

Example Output:

```
1 Hello Linux
2 Welcome to Ubuntu
```

## Common Options

| Option | Description |
|---------|-------------|
| `-n` | Number all lines |
| `-b` | Number non-empty lines |
| `-E` | Show end of line |
| `-T` | Show tabs |

---

# 2. `more` Command

## Definition

The `more` command displays file contents one screen at a time.

Useful for reading large files.

## Syntax

```bash
more filename
```

Example:

```bash
more file.txt
```

### Navigation

| Key | Action |
|-----|----------|
| Space | Next page |
| Enter | Next line |
| q | Quit |

---

# 3. `less` Command

## Definition

The `less` command is an improved version of `more`.

It allows forward and backward navigation.

## Syntax

```bash
less filename
```

Example:

```bash
less file.txt
```

### Navigation

| Key | Action |
|-----|----------|
| Up Arrow | Previous line |
| Down Arrow | Next line |
| Space | Next page |
| b | Previous page |
| /word | Search |
| n | Next search result |
| q | Quit |

---

# Difference Between `more` and `less`

| Feature | more | less |
|----------|:----:|:----:|
| Forward navigation | ✅ | ✅ |
| Backward navigation | ❌ | ✅ |
| Search | Limited | ✅ |
| Large file support | Good | Better |

---

# 4. `head` Command

## Definition

The `head` command displays the first part of a file.

By default, it shows the first 10 lines.

## Syntax

```bash
head filename
```

Example:

```bash
head file.txt
```

---

## Display first 5 lines

```bash
head -5 file.txt
```

or

```bash
head -n 5 file.txt
```

---

# 5. `tail` Command

## Definition

The `tail` command displays the last part of a file.

By default, it shows the last 10 lines.

## Syntax

```bash
tail filename
```

Example:

```bash
tail file.txt
```

---

## Display last 5 lines

```bash
tail -5 file.txt
```

or

```bash
tail -n 5 file.txt
```

---

## Live Monitoring

```bash
tail -f logfile.txt
```

The `-f` option continuously displays new data added to the file.

Press:

```
Ctrl + C
```

to stop.

---

# Real-World Examples

## Display file

```bash
cat notes.txt
```

---

## Read a large log file

```bash
less /var/log/syslog
```

---

## View one page at a time

```bash
more notes.txt
```

---

## View first 10 lines

```bash
head server.log
```

---

## View last 10 lines

```bash
tail server.log
```

---

## Monitor server logs

```bash
tail -f server.log
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| `cat` | Display entire file |
| `more` | View one page at a time |
| `less` | Advanced file viewer |
| `head` | Display first lines |
| `tail` | Display last lines |

---

# Comparison Table

| Command | Small File | Large File | Live Updates |
|----------|:---------:|:----------:|:------------:|
| cat | ✅ | ❌ | ❌ |
| more | ✅ | ✅ | ❌ |
| less | ✅ | ✅ | ❌ |
| head | ✅ | ✅ | ❌ |
| tail | ✅ | ✅ | ✅ (`-f`) |

---

# Practice Commands

```bash
cat file.txt

cat -n file.txt

more file.txt

less file.txt

head file.txt

head -5 file.txt

tail file.txt

tail -5 file.txt

tail -f logfile.txt
```

---

# Interview Questions

## What does `cat` do?

Displays and combines file contents.

## What is the difference between `more` and `less`?

- `more` moves forward only.
- `less` allows forward and backward movement and searching.

## How many lines does `head` display by default?

10 lines.

## How many lines does `tail` display by default?

10 lines.

## What does `tail -f` do?

Continuously monitors a file for new content.

---

# Quick Revision

| Command | Example |
|----------|----------|
| Display file | `cat file.txt` |
| Page by page | `more file.txt` |
| Advanced viewer | `less file.txt` |
| First 10 lines | `head file.txt` |
| First 5 lines | `head -5 file.txt` |
| Last 10 lines | `tail file.txt` |
| Last 5 lines | `tail -5 file.txt` |
| Live logs | `tail -f logfile.txt` |

---

# Memory Trick

- **cat** → Show entire file.
- **more** → Read one page at a time.
- **less** → More powerful file viewer.
- **head** → Beginning of the file.
- **tail** → End of the file.
- **tail -f** → Watch a file in real time.

## Visual Representation

```
File
 │
 ├── cat  ──► Entire file
 │
 ├── more ─► Page by page
 │
 ├── less ─► Forward + Backward + Search
 │
 ├── head ─► First 10 lines
 │
 └── tail ─► Last 10 lines
             │
             └── tail -f ─► Live updates
```