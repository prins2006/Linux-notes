# Linux Input and Output Redirection

## What is Redirection?

Redirection is used to change the default input or output of a command.

- **Standard Input (stdin)** : Keyboard
- **Standard Output (stdout)** : Terminal screen
- **Standard Error (stderr)** : Terminal screen for error messages

| Stream | Number | Default |
|--------|---------|----------|
| stdin | 0 | Keyboard |
| stdout | 1 | Terminal |
| stderr | 2 | Terminal |

---

# Input Redirection (`<`)

Input redirection takes data from a file instead of the keyboard.

## Syntax

```bash
command < filename
```

## Example

Create a file:

```bash
echo "Hello Linux" > input.txt
```

Use input redirection:

```bash
cat < input.txt
```

Output:

```
Hello Linux
```

---

# Output Redirection (`>`)

Sends command output to a file.

## Syntax

```bash
command > filename
```

## Example

```bash
echo "Linux" > file.txt
```

Check:

```bash
cat file.txt
```

Output:

```
Linux
```

### Note

If the file exists, `>` overwrites its contents.

Example:

```bash
echo "Ubuntu" > file.txt
```

Now:

```
Ubuntu
```

---

# Append Output (`>>`)

Adds output to the end of a file.

## Syntax

```bash
command >> filename
```

Example:

```bash
echo "Linux" > notes.txt
echo "Unix" >> notes.txt
echo "Ubuntu" >> notes.txt
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

# Error Redirection (`2>`)

Stores error messages in a file.

## Syntax

```bash
command 2> error.txt
```

Example:

```bash
ls abc.txt 2> error.txt
```

View error:

```bash
cat error.txt
```

Output:

```
ls: cannot access 'abc.txt': No such file or directory
```

---

# Append Error (`2>>`)

Adds error messages to an existing file.

```bash
ls abc.txt 2>> error.txt
```

---

# Redirect Output and Error Together

## Syntax

```bash
command > output.txt 2>&1
```

Example:

```bash
ls file.txt abc.txt > result.txt 2>&1
```

Stores both normal output and errors in `result.txt`.

---

# Discard Output

Use `/dev/null` to ignore output.

## Ignore normal output

```bash
command > /dev/null
```

## Ignore errors

```bash
command 2> /dev/null
```

## Ignore everything

```bash
command > /dev/null 2>&1
```

Example:

```bash
ls abc.txt > /dev/null 2>&1
```

No output is displayed.

---

# Pipe (`|`)

A pipe sends the output of one command as the input to another command.

## Syntax

```bash
command1 | command2
```

Example:

```bash
cat file.txt | sort
```

Example:

```bash
ls | wc -l
```

Counts the number of files.

---

# Real-World Examples

## Save directory listing

```bash
ls > files.txt
```

## Append today's date

```bash
date >> log.txt
```

## Save errors

```bash
find /root 2> errors.txt
```

## Count lines in a file

```bash
cat file.txt | wc -l
```

## Sort names

```bash
cat names.txt | sort
```

---

# Redirection Operators Summary

| Operator | Purpose |
|----------|----------|
| `>` | Write output to file (overwrite) |
| `>>` | Append output to file |
| `<` | Read input from file |
| `2>` | Write errors to file |
| `2>>` | Append errors to file |
| `2>&1` | Combine output and errors |
| `|` | Pass output to another command |
| `/dev/null` | Discard output |

---

# Practice Commands

```bash
echo "Hello" > file.txt

echo "Linux" >> file.txt

cat < file.txt

ls > files.txt

ls abc.txt 2> error.txt

ls > output.txt 2>&1

cat file.txt | wc -l

cat names.txt | sort

ls | grep txt
```

---

# Interview Questions

## What is redirection?

Redirection changes the default input or output of a command.

## What does `>` do?

It writes output to a file and overwrites existing content.

## What does `>>` do?

It appends output to the end of a file.

## What does `<` do?

It takes input from a file instead of the keyboard.

## What does `2>` do?

It redirects error messages to a file.

## What is a pipe (`|`)?

A pipe passes the output of one command as input to another command.

---

# Quick Revision

| Command | Work |
|----------|------|
| `>` | Overwrite output |
| `>>` | Append output |
| `<` | Input from file |
| `2>` | Error output |
| `2>&1` | Output + Error |
| `|` | Connect commands |
| `/dev/null` | Discard output |

## Memory Trick

- **`>` = Create/Overwrite**
- **`>>` = Add**
- **`<` = Take input**
- **`2>` = Save errors**
- **`|` = Connect commands**
- **`/dev/null` = Throw away output**