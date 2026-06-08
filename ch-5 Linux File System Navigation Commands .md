# Day 8 - Linux File System Navigation Commands (pwd, ls, cd)

# Introduction

Linux stores files and directories in a hierarchical structure.

To move around this structure, Linux provides navigation commands.

The three basic navigation commands are:

1. pwd
2. ls
3. cd

These commands are used daily by Linux users and developers.

---

# Linux File System

Suppose your Linux file system looks like this:

```text
/
├── home
│   └── prins
│       ├── Documents
│       ├── Downloads
│       └── Projects
├── etc
├── usr
└── var
```

Assume your current location is:

```
/home/prins
```

---

# 1. pwd

## What is pwd?

pwd stands for:

```
Print Working Directory
```

It displays your current location in the Linux file system.

---

## Syntax

```bash
pwd
```

---

## Example

```bash
pwd
```

Output:

```
/home/prins
```

Meaning:

You are currently inside:

```
/
└── home
    └── prins
```

---

## Internal Working

```
User

↓

Shell

↓

Kernel

↓

Current Directory Information

↓

Display Path
```

---

## Real-World Example

Imagine you are inside a shopping mall.

You ask:

"Where am I?"

The answer:

```
Second Floor
Electronics Section
```

pwd does the same.

---

# 2. ls

## What is ls?

ls stands for:

```
List
```

It displays files and directories.

---

## Syntax

```bash
ls
```

---

## Example

Current directory:

```
/home/prins
```

Run:

```bash
ls
```

Output:

```
Documents
Downloads
Projects
notes.txt
```

---

## Internal Working

```
User

↓

Shell

↓

Kernel

↓

File System

↓

Read Directory Entries

↓

Display Files
```

---

## Useful ls Options

### ls -l

Detailed information.

```bash
ls -l
```

Example:

```
-rw-r--r-- 1 prins users 100 notes.txt
```

---

### ls -a

Show hidden files.

```bash
ls -a
```

Output:

```
.
..
.bashrc
.profile
notes.txt
```

---

### ls -la

Detailed + hidden files.

```bash
ls -la
```

---

### ls -i

Display inode numbers.

```bash
ls -i
```

Example:

```
1256 notes.txt
1257 project
```

---

# 3. cd

## What is cd?

cd stands for:

```
Change Directory
```

It changes your current location.

---

## Syntax

```bash
cd directory_name
```

---

## Example

Current:

```
/home/prins
```

Move:

```bash
cd Documents
```

Now:

```
/home/prins/Documents
```

Check:

```bash
pwd
```

Output:

```
/home/prins/Documents
```

---

# Important cd Commands

## Home Directory

```bash
cd
```

or

```bash
cd ~
```

Moves to:

```
/home/prins
```

---

## Parent Directory

```bash
cd ..
```

Suppose:

```
/home/prins/Documents
```

After:

```bash
cd ..
```

You move to:

```
/home/prins
```

---

## Root Directory

```bash
cd /
```

Moves to:

```
/
```

---

## Previous Directory

```bash
cd -
```

Moves back to the previous directory.

---

## Absolute Path

```bash
cd /home/prins/Documents
```

---

## Relative Path

Current:

```
/home/prins
```

Move:

```bash
cd Documents
```

---

# Internal Working of cd

Suppose:

```bash
cd Documents
```

Linux performs:

```
User

↓

Shell

↓

Kernel

↓

File System

↓

Check Directory Exists

↓

Check Permissions

↓

Update Current Working Directory

↓

Success
```

---

# Navigation Example

Start:

```
/home/prins
```

Check:

```bash
pwd
```

Output:

```
/home/prins
```

List:

```bash
ls
```

Output:

```
Documents
Downloads
Projects
```

Move:

```bash
cd Documents
```

Check:

```bash
pwd
```

Output:

```
/home/prins/Documents
```

Go back:

```bash
cd ..
```

Check:

```bash
pwd
```

Output:

```
/home/prins
```

Go to root:

```bash
cd /
```

Check:

```bash
pwd
```

Output:

```
/
```

---

# Command Comparison

| Command | Meaning | Purpose |
|----------|---------|----------|
| pwd | Print Working Directory | Show current location |
| ls | List | Show files and directories |
| cd | Change Directory | Move between directories |

---

# Real-World Example

Imagine a city.

```
City

↓

Area

↓

Street

↓

House
```

You can:

Know your location:

```
pwd
```

See nearby houses:

```
ls
```

Move to another street:

```
cd
```

---

# Interview Questions

## What is pwd?

pwd stands for Print Working Directory and displays the current directory.

---

## What is ls?

ls displays files and directories.

---

## What is cd?

cd changes the current working directory.

---

## What is cd ..?

Moves to the parent directory.

---

## What is cd /?

Moves to the root directory.

---

# Summary

- pwd shows your current location.
- ls displays files and directories.
- cd changes directories.
- Linux navigation starts from the root directory (/).
- Absolute paths start from /.
- Relative paths start from the current directory.

---

# Key Points

✅ pwd = Print Working Directory.

✅ ls = List files and directories.

✅ cd = Change Directory.

✅ cd .. = Parent directory.

✅ cd / = Root directory.

✅ cd ~ = Home directory.

✅ cd - = Previous directory.

---

# Easy Trick to Remember

```
Know Where You Are

↓

pwd

See What's Here

↓

ls

Move Somewhere Else

↓

cd
```

---

# Practice Commands

```bash
pwd
ls
cd Documents
pwd
cd ..
pwd
cd /
pwd
cd ~
pwd
ls -l
ls -a
ls -la
ls -i
```