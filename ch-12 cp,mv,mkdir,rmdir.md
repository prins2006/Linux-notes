# Linux File Maintenance Commands
## `cp`, `mv`, `rm`, `mkdir`, `rmdir`

These commands are used to create, copy, move, rename, and delete files and directories.

---

# 1. `cp` Command

## Definition

The `cp` (copy) command is used to copy files and directories.

## Syntax

```bash
cp source destination
```

## Examples

### Copy a file

```bash
cp file1.txt file2.txt
```

Copies the contents of `file1.txt` to `file2.txt`.

---

### Copy multiple files

```bash
cp file1.txt file2.txt backup/
```

Copies both files into the `backup` directory.

---

### Copy a directory

```bash
cp -r project backup_project
```

The `-r` option means recursive and copies all files and subdirectories.

---

## Common Options

| Option | Description |
|---------|-------------|
| `-r` | Copy directories recursively |
| `-i` | Ask before overwrite |
| `-v` | Show copied files |

Example:

```bash
cp -iv file1.txt file2.txt
```

---

# 2. `mv` Command

## Definition

The `mv` (move) command moves or renames files and directories.

## Syntax

```bash
mv source destination
```

---

## Rename a file

```bash
mv old.txt new.txt
```

---

## Move a file

```bash
mv file.txt Documents/
```

---

## Move multiple files

```bash
mv file1.txt file2.txt backup/
```

---

## Rename a directory

```bash
mv project linux_project
```

---

## Common Options

| Option | Description |
|---------|-------------|
| `-i` | Confirm before overwrite |
| `-v` | Show moved files |

Example:

```bash
mv -v file.txt backup/
```

---

# 3. `rm` Command

## Definition

The `rm` (remove) command deletes files and directories.

## Syntax

```bash
rm filename
```

---

## Delete a file

```bash
rm file.txt
```

---

## Delete multiple files

```bash
rm file1.txt file2.txt
```

---

## Delete a directory

```bash
rm -r project
```

---

## Force delete

```bash
rm -f file.txt
```

---

## Force delete directory

```bash
rm -rf project
```

⚠️ **Warning:** Be careful with `rm -rf`. Deleted files cannot be easily recovered.

---

## Common Options

| Option | Description |
|---------|-------------|
| `-r` | Delete directories recursively |
| `-f` | Force deletion |
| `-i` | Ask before deleting |
| `-v` | Show deleted files |

---

# 4. `mkdir` Command

## Definition

The `mkdir` (make directory) command creates directories.

## Syntax

```bash
mkdir directory_name
```

---

## Create one directory

```bash
mkdir project
```

---

## Create multiple directories

```bash
mkdir test backup docs
```

---

## Create nested directories

```bash
mkdir -p project/src/java
```

The `-p` option creates parent directories automatically.

---

## Common Options

| Option | Description |
|---------|-------------|
| `-p` | Create parent directories |
| `-v` | Show created directories |

---

# 5. `rmdir` Command

## Definition

The `rmdir` command removes empty directories.

## Syntax

```bash
rmdir directory_name
```

---

## Remove an empty directory

```bash
rmdir project
```

---

## Remove nested empty directories

```bash
rmdir -p project/src/java
```

---

## Note

If the directory contains files:

```bash
rmdir project
```

Output:

```
rmdir: failed to remove 'project': Directory not empty
```

Use:

```bash
rm -r project
```

to remove non-empty directories.

---

# Real-World Example

## Step 1: Create directories

```bash
mkdir LinuxNotes
mkdir LinuxBackup
```

---

## Step 2: Create a file

```bash
echo "Linux Commands" > notes.txt
```

---

## Step 3: Copy the file

```bash
cp notes.txt LinuxNotes/
```

---

## Step 4: Move the file

```bash
mv notes.txt LinuxBackup/
```

---

## Step 5: Remove the copied file

```bash
rm LinuxNotes/notes.txt
```

---

## Step 6: Remove empty directory

```bash
rmdir LinuxNotes
```

---

# Command Summary

| Command | Purpose |
|----------|----------|
| `cp` | Copy files/directories |
| `mv` | Move or rename files/directories |
| `rm` | Delete files/directories |
| `mkdir` | Create directories |
| `rmdir` | Remove empty directories |

---

# Comparison Table

| Command | File | Directory | Rename |
|----------|:----:|:---------:|:------:|
| cp | ✅ | ✅ (`-r`) | ❌ |
| mv | ✅ | ✅ | ✅ |
| rm | ✅ | ✅ (`-r`) | ❌ |
| mkdir | ❌ | ✅ | ❌ |
| rmdir | ❌ | Empty only | ❌ |

---

# Practice Commands

```bash
mkdir project

mkdir -p project/src

echo "Hello Linux" > file.txt

cp file.txt backup.txt

cp -r project backup_project

mv backup.txt Documents/

mv project linux_project

rm file.txt

rm -r linux_project

mkdir test

rmdir test
```

---

# Interview Questions

## What does `cp` do?

Copies files and directories.

## What does `mv` do?

Moves or renames files and directories.

## What is the difference between `rm` and `rmdir`?

- `rm` removes files and non-empty directories (`-r`).
- `rmdir` removes only empty directories.

## What does `mkdir -p` do?

Creates parent and child directories automatically.

## What does `rm -rf` do?

Forcefully removes files and directories recursively.

⚠️ Use with caution.

---

# Quick Revision

| Command | Example |
|----------|----------|
| Copy file | `cp a.txt b.txt` |
| Copy directory | `cp -r dir1 dir2` |
| Move file | `mv a.txt docs/` |
| Rename file | `mv old.txt new.txt` |
| Delete file | `rm a.txt` |
| Delete directory | `rm -r dir1` |
| Create directory | `mkdir test` |
| Create nested directories | `mkdir -p a/b/c` |
| Remove empty directory | `rmdir test` |

# Memory Trick

- **cp** → Copy
- **mv** → Move or Rename
- **rm** → Remove
- **mkdir** → Make Directory
- **rmdir** → Remove Empty Directory