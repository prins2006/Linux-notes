# 137. Types of Shells in Linux

## What is a Shell?

A **Shell** is a command-line interpreter that acts as an interface between the user and the Linux kernel.

It accepts commands from the user, processes them, and sends requests to the kernel for execution.

### Linux Architecture

```
+------------------+
|      User        |
+------------------+
          |
          v
+------------------+
|      Shell       |
+------------------+
          |
          v
+------------------+
|   Linux Kernel   |
+------------------+
          |
          v
+------------------+
|    Hardware      |
+------------------+
```

---

# Types of Shells

## 1. Bourne Shell (sh)

Developed by Stephen Bourne.

Executable:

```bash
/bin/sh
```

Features:
- Basic shell scripting
- Command execution
- Portable across UNIX systems

Check:

```bash
sh --version
```

Example:

```bash
sh script.sh
```

---

## 2. Bourne Again Shell (bash)

Most common Linux shell.

Executable:

```bash
/bin/bash
```

Features:
- Command history
- Auto-completion
- Variables
- Aliases
- Advanced scripting

Check:

```bash
bash --version
```

Start bash:

```bash
bash
```

---

## 3. Korn Shell (ksh)

Developed by David Korn.

Features:
- Better scripting
- Command history
- Arithmetic operations

Start:

```bash
ksh
```

---

## 4. C Shell (csh)

Syntax similar to C programming.

Features:
- Aliases
- History
- Job control

Start:

```bash
csh
```

---

## 5. TENEX C Shell (tcsh)

Enhanced version of csh.

Features:
- Command completion
- Filename completion
- Command editing

Start:

```bash
tcsh
```

---

## 6. Z Shell (zsh)

Modern advanced shell.

Features:
- Auto completion
- Plugins
- Themes
- Git integration

Start:

```bash
zsh
```

Check version:

```bash
zsh --version
```

---

## 7. Fish Shell

Friendly Interactive Shell.

Features:
- Smart suggestions
- Syntax highlighting
- Auto completion

Start:

```bash
fish
```

---

# Check Current Shell

```bash
echo $SHELL
```

Example:

```
/bin/bash
```

---

# List Available Shells

```bash
cat /etc/shells
```

Example:

```
/bin/sh
/bin/bash
/bin/dash
/bin/zsh
/bin/ksh
/bin/tcsh
```

---

# Change User Shell

Syntax:

```bash
chsh -s shell_path
```

Example:

```bash
chsh -s /bin/bash
```

For Z Shell:

```bash
chsh -s /bin/zsh
```

Logout and login again.

---

# Shell Comparison

| Shell | Features |
|--------|----------|
| sh | Basic UNIX shell |
| bash | Most popular |
| ksh | Advanced scripting |
| csh | C-like syntax |
| tcsh | Enhanced csh |
| zsh | Powerful and customizable |
| fish | Beginner-friendly |

---

# Production Use Cases

## System Administration

Use:

```
bash
```

---

## DevOps Automation

Use:

```
bash
```

or

```
zsh
```

---

## Development

Use:

```
zsh
```

with plugins.

---

# Common Shell Commands

| Command | Purpose |
|----------|----------|
| echo $SHELL | Current shell |
| cat /etc/shells | Available shells |
| bash | Start bash |
| sh | Start sh |
| zsh | Start zsh |
| fish | Start fish |
| chsh -s /bin/bash | Change shell |

---

# Interview Questions

## Q1. What is a shell?

A shell is a command interpreter that acts as an interface between the user and the Linux kernel.

---

## Q2. Which shell is the default on most Linux distributions?

Bash (Bourne Again Shell).

---

## Q3. How do you check your current shell?

```bash
echo $SHELL
```

---

## Q4. How do you list installed shells?

```bash
cat /etc/shells
```

---

# 138. Shell Scripting

## What is Shell Scripting?

A shell script is a text file containing Linux commands that are executed sequentially by a shell.

It automates repetitive tasks.

File extension:

```
.sh
```

Example:

```
backup.sh
```

---

# Advantages

✅ Automation

✅ Task scheduling

✅ Server administration

✅ Monitoring

✅ Backups

✅ Application deployment

---

# Create Script

Create file:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello World"
```

Save.

---

# Give Execute Permission

```bash
chmod +x hello.sh
```

---

# Run Script

Method 1:

```bash
./hello.sh
```

Method 2:

```bash
bash hello.sh
```

---

# Shebang

First line:

```bash
#!/bin/bash
```

Specifies the shell interpreter.

---

# Variables

Example:

```bash
#!/bin/bash

NAME="Prins"

echo $NAME
```

Output:

```
Prins
```

---

# User Input

```bash
#!/bin/bash

echo "Enter name"

read NAME

echo "Hello $NAME"
```

---

# Command Substitution

```bash
DATE=$(date)

echo $DATE
```

---

# If Statement

```bash
#!/bin/bash

NUM=10

if [ $NUM -gt 5 ]
then
echo "Greater"
fi
```

---

# If Else

```bash
if [ $NUM -gt 5 ]
then
echo "Greater"
else
echo "Smaller"
fi
```

---

# For Loop

```bash
for i in 1 2 3 4 5
do
echo $i
done
```

---

# While Loop

```bash
COUNT=1

while [ $COUNT -le 5 ]
do
echo $COUNT
COUNT=$((COUNT+1))
done
```

---

# Case Statement

```bash
read OPTION

case $OPTION in

1)
echo "Start"
;;

2)
echo "Stop"
;;

*)
echo "Invalid"
;;

esac
```

---

# Functions

```bash
hello(){

echo "Hello Linux"

}

hello
```

---

# Arguments

Script:

```bash
echo $1
echo $2
```

Run:

```bash
./test.sh Linux Bash
```

Output:

```
Linux
Bash
```

---

# Exit Status

Check:

```bash
echo $?
```

Success:

```
0
```

Failure:

```
1
```

---

# Common Special Variables

| Variable | Meaning |
|----------|----------|
| $0 | Script name |
| $1 | First argument |
| $2 | Second argument |
| $# | Number of arguments |
| $@ | All arguments |
| $$ | Process ID |
| $? | Exit status |

---

# Production Example

## Disk Usage Check

```bash
#!/bin/bash

df -h

free -h

uptime
```

---

## Backup Script

```bash
#!/bin/bash

tar -czf backup.tar.gz /home
```

---

## Service Check

```bash
#!/bin/bash

systemctl status ssh
```

---

## Log Monitoring

```bash
#!/bin/bash

tail -10 /var/log/syslog
```

---

# Common Commands

| Command | Purpose |
|----------|----------|
| chmod +x file.sh | Make executable |
| ./file.sh | Run script |
| bash file.sh | Run with bash |
| sh file.sh | Run with sh |
| echo $? | Exit status |

---

# Real Production Use Cases

## Server Health Check

Collect:

- CPU
- Memory
- Disk
- Uptime

---

## Daily Backup

Create automated backups.

---

## User Management

Automatically create users.

---

## Service Monitoring

Check and restart failed services.

---

## Deployment

Deploy applications automatically.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| nano file.sh | Create script |
| chmod +x file.sh | Execute permission |
| ./file.sh | Run script |
| bash file.sh | Run with bash |
| echo $? | Check exit status |
| read VAR | User input |
| if | Conditional |
| for | Loop |
| while | Loop |
| case | Multiple conditions |

---

# Interview Questions

## Q1. What is shell scripting?

Shell scripting is writing a sequence of shell commands in a file to automate tasks.

---

## Q2. What is the purpose of `#!/bin/bash`?

It tells the system to execute the script using the Bash shell.

---

## Q3. How do you make a shell script executable?

```bash
chmod +x script.sh
```

---

## Q4. How do you run a shell script?

```bash
./script.sh
```

or

```bash
bash script.sh
```

---

## Q5. What does `$?` represent?

The exit status of the last executed command.

- `0` = Success
- Non-zero = Error

---

## Q6. What is the difference between `$*` and `$@`?

- `$*` treats all arguments as a single string.
- `$@` treats each argument as a separate string, making it safer for loops and scripting.

---