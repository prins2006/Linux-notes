# Day 2 - What is Linux?

# Introduction

Linux is an **open-source operating system** that manages computer hardware and software resources and provides a platform for running applications.

In simple words:

> **Linux is an operating system that acts as a bridge between the user and the computer hardware.**

Examples of operating systems:

* Linux
* Windows
* macOS
* Android
* iOS

---

# What is the Meaning of Linux?

Linux is named after its creator, Linus Torvalds, and Unix.

* **Linus** → Creator's name.
* **Unix** → The operating system that inspired Linux.

Linux was created by Linus Torvalds in 1991.

---

# What is Linux?

Linux is a collection of software that helps the computer perform tasks such as:

* Running applications.
* Managing files.
* Managing memory.
* Managing hardware devices.
* Managing users and security.

---

# Simple Architecture

```text
User
  ↓
Applications
(VS Code, Git, Chrome)
  ↓
Linux Operating System
  ↓
Hardware
(CPU, RAM, Disk, Keyboard)
```

Linux receives commands from users and applications and communicates with the hardware.

---

# Why was Linux Created?

Before Linux, many operating systems required expensive licenses.

Linux was created to provide:

* Free software.
* Open-source development.
* Better customization.
* Stable and secure computing.

---

# Features of Linux

## 1. Open Source

Anyone can study, modify, and improve the source code.

---

## 2. Free

Linux can be downloaded and used without purchasing a license.

---

## 3. Multi-user

Many users can work on the same Linux system.

Example:

A company server may have hundreds of users.

---

## 4. Multitasking

Linux can run many programs at the same time.

Example:

* Chrome
* VS Code
* Git
* Terminal

All can run together.

---

## 5. Secure

Linux provides:

* User accounts
* Passwords
* File permissions

---

## 6. Stable

Linux systems can run for long periods with high reliability.

---

# Components of Linux

## Kernel

The kernel is the core of Linux.

It communicates directly with the hardware.

It manages:

* CPU
* Memory
* Devices
* Files

---

## Shell

The shell accepts user commands.

Example:

```bash
pwd
ls
mkdir
```

The shell sends these commands to the kernel.

---

## File System

Linux organizes data into directories and files.

Example:

```text
/home
/etc
/var
/usr
```

---

## Applications

Programs such as:

* VS Code
* Git
* Firefox
* Terminal utilities

run on Linux.

---

# Linux Distributions

A Linux distribution (distro) is a complete Linux operating system.

Popular distributions:

* Ubuntu
* Debian
* Fedora
* CentOS
* Arch Linux
* Linux Mint

---

# Why Do Developers Learn Linux?

Linux is widely used for:

* Software development
* Git and GitHub
* Cloud computing
* Web servers
* DevOps
* Docker
* Cybersecurity

---

# Linux vs Windows

| Linux               | Windows              |
| ------------------- | -------------------- |
| Open source         | Proprietary          |
| Free                | Licensed             |
| Highly customizable | Less customizable    |
| Popular for servers | Popular for desktops |
| Strong command line | GUI focused          |

---

# Real-World Example

When you type:

```bash
mkdir project
```

What happens?

1. You type the command.
2. The shell receives it.
3. The shell sends the request to the Linux kernel.
4. The kernel communicates with the hardware.
5. The storage device creates the directory.
6. Linux displays the result.

---

# Basic Linux Commands

## Current User

```bash
whoami
```

---

## Current Directory

```bash
pwd
```

---

## List Files

```bash
ls
```

---

## Linux Information

```bash
uname
```

---

## Detailed System Information

```bash
uname -a
```

---

## Linux Distribution Information

```bash
cat /etc/os-release
```

---

# Interview Questions

## What is Linux?

Linux is an open-source operating system that manages computer hardware and software resources and provides services for applications.

---

## Who created Linux?

Linus Torvalds created Linux in 1991.

---

## What are the main features of Linux?

* Open source
* Free
* Secure
* Stable
* Multi-user
* Multitasking

---

# Summary

* Linux is an operating system.
* Linux is open source and free.
* Linux acts as a bridge between users and hardware.
* The kernel is the core component of Linux.
* Linux is widely used in servers and software development.

---

# Key Points

✅ Linux is an operating system.

✅ Linux was created by Linus Torvalds in 1991.

✅ Linux is open source and free.

✅ Main components:

* Kernel
* Shell
* File System
* Applications

✅ Linux is widely used for development, servers, and cloud computing.

---

# Practice Commands

```bash
whoami
pwd
ls
uname
uname -a
cat /etc/os-release
date
```
