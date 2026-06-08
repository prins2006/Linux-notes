# Day 6 - What is a File System?

# Introduction

A File System is a method used by an operating system to organize, store, manage, and retrieve files and directories on a storage device.

In simple words:

> A File System is the way an operating system organizes and manages data on a storage device.

Without a file system, the operating system would not know:
- Where files are stored.
- How to find files.
- How to create or delete files.

---

# Simple Example

Think of a library.

```
Library
│
├── Science
│   ├── Physics.pdf
│   └── Chemistry.pdf
│
├── Mathematics
│   └── Algebra.pdf
│
└── Computer
    └── Linux.pdf
```

Here:

Library = File System

Science = Directory

Physics.pdf = File

Similarly, Linux stores files in directories.

---

# Why Do We Need a File System?

A file system helps the operating system:

- Store files.
- Find files.
- Delete files.
- Rename files.
- Protect files.
- Manage storage space.

---

# Linux File System Structure

Linux starts from a single root directory.

```
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── opt
├── proc
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

The "/" is called the Root Directory.

Everything in Linux starts from the root directory.

---

# Important Linux Directories

## /

Root directory.

The top-level directory.

---

## /home

Stores user home directories.

Example:

```
/home/prins
```

---

## /etc

Stores system configuration files.

Example:

```
/etc/passwd
```

---

## /bin

Contains essential commands.

Examples:

```
ls
cp
mv
cat
```

---

## /usr

Contains user programs and utilities.

---

## /tmp

Stores temporary files.

---

## /var

Stores variable data such as:

- Logs
- Cache
- Mail

---

# File and Directory

## File

A file stores data.

Examples:

```
notes.txt
image.png
script.sh
```

---

## Directory

A directory stores files and other directories.

Example:

```
Documents/
Downloads/
Projects/
```

---

# File Path

A path tells the operating system where a file is located.

Example:

```
/home/prins/Documents/linux.txt
```

Meaning:

```
/
↓
home
↓
prins
↓
Documents
↓
linux.txt
```

---

# Absolute Path

Starts from the root directory.

Example:

```
/home/prins/file.txt
```

---

# Relative Path

Starts from the current directory.

Example:

```
Documents/file.txt
```

---

# Linux File System vs Windows File System

## Linux

Starts from:

```
/
```

Example:

```
/home/prins/Documents
```

---

## Windows

Starts from drive letters:

```
C:\
D:\
E:\
```

Example:

```
C:\Users\Prins\Documents
```

---

# Basic File System Commands

## Current Directory

```bash
pwd
```

Example Output:

```
/home/prins
```

---

## List Files

```bash
ls
```

---

## Change Directory

```bash
cd Documents
```

---

## Create Directory

```bash
mkdir project
```

---

## Create File

```bash
touch notes.txt
```

---

## Remove File

```bash
rm notes.txt
```

---

## Remove Directory

```bash
rmdir project
```

---

# Real-World Example

Suppose you create a project.

```
Projects
│
└── Linux
    ├── notes.md
    ├── script.sh
    └── image.png
```

Linux uses the file system to know exactly where each file is stored.

---

# Types of Linux File Systems

Common Linux file systems:

- ext4
- ext3
- ext2
- XFS
- Btrfs

For beginners, ext4 is the most common.

---

# Interview Questions

## What is a File System?

A file system is a method used by an operating system to organize, store, and retrieve files and directories.

---

## What is the root directory?

The root directory is the top-level directory in Linux represented by "/".

---

## What is the difference between a file and a directory?

A file stores data.

A directory stores files and other directories.

---

## What is an absolute path?

A path that starts from the root directory.

Example:

```
/home/prins/file.txt
```

---

# Summary

- A file system organizes data.
- Linux starts from the root directory (/).
- Files store data.
- Directories store files and directories.
- Paths help locate files.
- Linux and Windows have different file system structures.

---

# Key Points

✅ File System = Organizes and manages data.

✅ Root Directory = /

✅ File = Stores data.

✅ Directory = Stores files and folders.

✅ Absolute Path starts with /.

✅ Relative Path starts from the current directory.

---

# Easy Trick to Remember

```
File System

↓

Stores

↓

Files + Directories

↓

Linux starts from

/

↓

Everything is inside the Root Directory
```

---

# Practice Commands

```bash
pwd
ls
mkdir linux-practice
cd linux-practice
touch notes.txt
ls
pwd
rm notes.txt
cd ..
rmdir linux-practice
```