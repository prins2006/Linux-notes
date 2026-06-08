 # Day 7 - Linux File System Structure and Internal Working

# Introduction

A File System is a method used by Linux to organize, store, manage, and retrieve data from storage devices.

In simple words:

> The Linux file system tells the operating system where files are stored and how to find them.

Without a file system:
- Files cannot be stored.
- Files cannot be found.
- Data cannot be managed.

---

# Real-Life Example

Imagine a large library.

```
Library
│
├── Science
│   ├── Physics.pdf
│   └── Chemistry.pdf
│
├── Computer
│   ├── Linux.pdf
│   └── Git.pdf
│
└── Mathematics
    └── Algebra.pdf
```

Here:

Library = Hard Disk

Sections = Directories

Books = Files

Librarian = File System

The librarian knows exactly where every book is stored.

Similarly, Linux knows where every file is stored.

---

# Internal Working

Suppose you create a file.

```bash
touch notes.txt
```

What happens internally?

```
User
 ↓
Shell
 ↓
Kernel
 ↓
File System
 ↓
Hard Disk
```

Step 1:

User types:

```bash
touch notes.txt
```

↓

Step 2:

The Shell receives the command.

↓

Step 3:

The Shell asks the Linux Kernel.

↓

Step 4:

The Kernel asks the File System:

"Find free space for this file."

↓

Step 5:

The File System allocates storage blocks.

↓

Step 6:

The File System creates file information.

↓

Step 7:

The Hard Disk stores the file.

↓

Step 8:

Linux returns success.

---

# Linux File System Hierarchy

Linux starts from one directory.

```
/
```

This is called the Root Directory.

Everything starts from "/".

```
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

---

# Important Directories

## /

Root directory.

Top of the file system.

Everything exists under it.

---

## /home

Stores user files.

Example:

```
/home/prins
```

Your documents and projects are usually here.

---

## /root

Home directory for the root user.

```
/root
```

---

## /etc

Stores configuration files.

Examples:

```
/etc/passwd
/etc/hostname
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
pwd
```

---

## /usr

Contains user applications.

Examples:

```
/usr/bin
/usr/lib
```

---

## /tmp

Stores temporary files.

---

## /var

Stores changing data.

Examples:

- Logs
- Cache
- Mail

---

## /dev

Represents hardware devices.

Examples:

```
/dev/sda
/dev/null
```

---

## /proc

Contains process and kernel information.

Example:

```
/proc/cpuinfo
```

---

# How Files are Stored Internally

Suppose:

```
notes.txt
```

Internally Linux stores:

```
File Name
↓

Metadata

↓

Storage Block Numbers

↓

Actual Data
```

Example:

```
notes.txt

↓

Owner
Permissions
Size
Creation Time

↓

Block 25
Block 31
Block 50

↓

Hello Linux
```

---

# What is Metadata?

Metadata means information about a file.

Example:

```
File Name

notes.txt

Owner

prins

Size

50 bytes

Permissions

rw-r--r--

Creation Time

10:30 AM
```

---

# What is an Inode?

In Linux, every file has an inode.

An inode stores:

- Owner
- Group
- Permissions
- File size
- Creation time
- Modification time
- Storage block locations

It does NOT store the file name.

```
File Name

notes.txt

↓

Inode

↓

Metadata

↓

Storage Blocks

↓

Actual Data
```

---

# What are Data Blocks?

The hard disk is divided into blocks.

Example:

```
Disk

---------------------------------

Block 1

Block 2

Block 3

Block 4

Block 5

---------------------------------
```

Suppose:

```
notes.txt
```

contains:

```
Hello Linux
```

Linux may store:

```
Block 2

Hello

Block 4

Linux
```

The inode remembers where these blocks are located.

---

# File Path

Absolute Path:

```
/home/prins/Documents/notes.txt
```

Relative Path:

```
Documents/notes.txt
```

---

# Internal Flow

Suppose:

```bash
cat notes.txt
```

What happens?

```
User

↓

Shell

↓

Kernel

↓

File System

↓

Find inode

↓

Find data blocks

↓

Read data

↓

Display

↓

User
```

---

# Common File System Commands

Current directory:

```bash
pwd
```

List files:

```bash
ls
```

Detailed list:

```bash
ls -l
```

Show inode:

```bash
ls -i
```

Create file:

```bash
touch notes.txt
```

Create directory:

```bash
mkdir project
```

Display file:

```bash
cat notes.txt
```

---

# Real-World Example

Suppose you save a VS Code file.

```
notes.md
```

VS Code

↓

Linux Kernel

↓

File System

↓

Find inode

↓

Allocate blocks

↓

Write data

↓

Save file

---

# Interview Questions

## What is a File System?

A file system is a method used by an operating system to organize and manage files and directories.

---

## What is the Root Directory?

The root directory is the top-level directory represented by "/".

---

## What is an inode?

An inode is a data structure that stores information about a file except its name and actual data.

---

## What are data blocks?

Data blocks are storage locations where the actual contents of a file are stored.

---

# Summary

- Linux starts from the root directory (/).
- Everything is treated as a file.
- Files have inodes.
- Inodes store metadata.
- Data blocks store actual file contents.
- The file system connects users, the kernel, and storage devices.

---

# Key Points

✅ Root Directory = /

✅ Every file has an inode.

✅ Inode stores metadata.

✅ Data blocks store actual data.

✅ File System organizes files and directories.

✅ The kernel uses the file system to access storage.

---

# Easy Trick to Remember

```
User
 ↓
Shell
 ↓
Kernel
 ↓
File System
 ↓
Inode
 ↓
Data Blocks
 ↓
Hard Disk
```

---

# Practice Commands

```bash
pwd
ls
ls -l
ls -i
touch notes.txt
cat notes.txt
mkdir project
cd project
pwd
```