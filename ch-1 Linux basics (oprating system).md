# Day 1 - What is an Operating System (OS)?

## Introduction

An Operating System (OS) is the most important software in a computer. It manages the computer's hardware and software resources and provides a way for users and applications to communicate with the hardware.

In simple words:

> **An Operating System acts as a bridge between the user and the computer hardware.**

Without an operating system, a computer cannot perform everyday tasks like opening applications, saving files, or accessing the internet.

---

# Simple Example

Suppose you want to watch a video.

1. You click the video player.
2. The Operating System loads the application.
3. The Operating System reads the video file from storage.
4. The CPU processes the data.
5. The sound card plays audio.
6. The monitor displays the video.

All of these activities are managed by the Operating System.

---

# Operating System Architecture

```
+----------------------+
|        User          |
+----------------------+
           |
           v
+----------------------+
|     Applications     |
| Chrome, VS Code, Git |
+----------------------+
           |
           v
+----------------------+
|   Operating System   |
+----------------------+
           |
           v
+----------------------+
|       Hardware       |
| CPU RAM Disk Mouse   |
+----------------------+
```

The Operating System sits between the applications and the hardware.

---

# Why Do We Need an Operating System?

Imagine a computer without an Operating System.

* The keyboard would not know where to send input.
* The monitor would not know what to display.
* Applications could not use memory.
* Files could not be stored properly.

The Operating System coordinates all these tasks.

---

# Main Functions of an Operating System

## 1. Process Management

A process is a running program.

Examples:

* Chrome
* VS Code
* Music Player
* Terminal

The OS:

* Starts processes.
* Stops processes.
* Allocates CPU time.
* Allows multiple programs to run together.

Example:

You can use Chrome while listening to music and editing code because the OS manages these processes.

---

## 2. Memory Management

Memory means RAM.

The OS:

* Allocates memory to programs.
* Frees memory when programs close.
* Prevents one program from corrupting another's memory.

Example:

Chrome: 2 GB

VS Code: 500 MB

Terminal: 50 MB

The OS manages this allocation.

---

## 3. File Management

The OS organizes data into files and directories.

Example:

```
Home
|
|-- Documents
|-- Downloads
|-- Pictures
|-- Projects
```

The OS allows users to:

* Create files.
* Delete files.
* Rename files.
* Copy files.
* Move files.

---

## 4. Device Management

The OS controls hardware devices.

Examples:

* Keyboard
* Mouse
* Printer
* USB drive
* Monitor

When you press a key, the OS receives the signal and displays the character.

---

## 5. Security Management

The OS provides security features.

Examples:

* User accounts.
* Password protection.
* File permissions.
* Access control.

Linux uses permissions such as:

* Read
* Write
* Execute

to protect files.

---

# Types of Operating Systems

## Single User

One user works at a time.

Example:

* Older desktop systems.

---

## Multi User

Many users can use the system.

Example:

* Linux servers.

---

## Multitasking

Runs many applications simultaneously.

Examples:

* Linux
* Windows
* macOS

---

# Examples of Operating Systems

| Operating System | Company               |
| ---------------- | --------------------- |
| Linux            | Open Source Community |
| Windows          | Microsoft             |
| macOS            | Apple                 |
| Android          | Google                |
| iOS              | Apple                 |

---

# Real-Life Analogy

Think of a restaurant.

| Restaurant | Computer         |
| ---------- | ---------------- |
| Customer   | User             |
| Waiter     | Operating System |
| Kitchen    | Hardware         |
| Food       | Output           |

The customer tells the waiter what they want.

The waiter communicates with the kitchen.

The kitchen prepares the food.

The waiter delivers the food.

Similarly, the Operating System receives user requests and coordinates the hardware to produce results.

---

# Real-World Example

You type:

```bash
mkdir project
```

What happens?

1. The keyboard sends input.
2. The OS receives the command.
3. The OS checks permissions.
4. The OS asks the disk to create a directory.
5. The directory is created.
6. The OS displays the result.

---

# Advantages of an Operating System

* Easy to use.
* Efficient resource management.
* Supports multitasking.
* Provides security.
* Manages files and hardware.

---

# Interview Questions

## What is an Operating System?

An Operating System is system software that manages computer hardware and software resources and acts as an interface between the user and the hardware.

---

## Why is an Operating System important?

Because it manages CPU, memory, files, devices, and applications, allowing users to interact with the computer efficiently.

---

## What are the main functions of an Operating System?

* Process management
* Memory management
* File management
* Device management
* Security management

---

# Summary

* An Operating System is system software.
* It acts as a bridge between users and hardware.
* It manages processes, memory, files, devices, and security.
* Examples include Linux, Windows, macOS, Android, and iOS.
* Without an Operating System, a computer cannot effectively run applications or manage hardware resources.

---

# Practice Commands

```bash
whoami
pwd
date
uname -a
cat /etc/os-release
ls
```

These commands help you explore your Linux operating system and understand how it provides information about the current user, location, kernel, and system.
