# Basic Shell Scripts in Linux

## What is a Shell Script?

A **Shell Script** is a text file containing a series of Linux commands that are executed automatically by the shell.

Shell scripts help automate repetitive tasks such as:
- File backups
- User management
- System monitoring
- Application deployment
- Scheduled tasks

The most commonly used shell is **Bash (Bourne Again Shell)**.

---

# Structure of a Shell Script

A basic shell script has three parts:

1. Shebang
2. Commands
3. Exit

Example:

```bash
#!/bin/bash

echo "Hello, Linux!"

exit 0
```

---

# Shebang (#!)

The first line of a script specifies the interpreter.

Example:

```bash
#!/bin/bash
```

Meaning:
- `#!` : Shebang
- `/bin/bash` : Bash interpreter

Other examples:

```bash
#!/bin/sh
```

```bash
#!/usr/bin/env bash
```

---

# Creating a Shell Script

Create a file:

```bash
nano hello.sh
```

Add:

```bash
#!/bin/bash

echo "Hello World"
```

Save the file.

---

# Make Script Executable

```bash
chmod +x hello.sh
```

---

# Run the Script

Method 1:

```bash
./hello.sh
```

Method 2:

```bash
bash hello.sh
```

Output:

```
Hello World
```

---

# Comments

Single-line comment:

```bash
# This is a comment
```

Example:

```bash
#!/bin/bash

# Display message

echo "Linux"
```

---

# Variables

Variables store values.

Syntax:

```bash
VARIABLE=value
```

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

# Multiple Variables

```bash
CITY="Ahmedabad"
COURSE="Linux"

echo $CITY
echo $COURSE
```

---

# Read User Input

```bash
#!/bin/bash

echo "Enter your name"

read NAME

echo "Welcome $NAME"
```

Output:

```
Enter your name

Prins

Welcome Prins
```

---

# Command Substitution

Store command output.

Example:

```bash
DATE=$(date)

echo $DATE
```

Alternative:

```bash
DATE=`date`
```

---

# Arithmetic Operations

```bash
A=10
B=20

SUM=$((A+B))

echo $SUM
```

Output:

```
30
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
NUM=3

if [ $NUM -gt 5 ]
then
echo "Greater"
else
echo "Smaller"
fi
```

---

# Nested If

```bash
AGE=20

if [ $AGE -gt 18 ]
then
if [ $AGE -lt 60 ]
then
echo "Adult"
fi
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

Output:

```
1
2
3
4
5
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

# Until Loop

```bash
COUNT=1

until [ $COUNT -gt 5 ]
do
echo $COUNT
COUNT=$((COUNT+1))
done
```

---

# Case Statement

```bash
echo "Enter option"

read CHOICE

case $CHOICE in

1)
echo "Start"
;;

2)
echo "Stop"
;;

3)
echo "Restart"
;;

*)
echo "Invalid"
;;

esac
```

---

# Functions

Define function:

```bash
hello(){

echo "Hello Linux"

}

hello
```

---

# Passing Arguments

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

# Special Variables

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

# Exit Status

Success:

```bash
echo $?
```

Output:

```
0
```

Error:

```
1
```

---

# File Check

```bash
FILE=test.txt

if [ -f $FILE ]
then
echo "File exists"
else
echo "File not found"
fi
```

---

# Directory Check

```bash
DIR=/home

if [ -d $DIR ]
then
echo "Directory exists"
fi
```

---

# Check User

```bash
echo $USER
```

---

# Display System Information

```bash
#!/bin/bash

echo "Hostname:"

hostname

echo "Kernel:"

uname -r

echo "Date:"

date
```

---

# Production Example 1

## Server Health Check

```bash
#!/bin/bash

echo "Hostname"

hostname

echo "CPU Load"

uptime

echo "Memory"

free -h

echo "Disk"

df -h
```

---

# Production Example 2

## Service Status

```bash
#!/bin/bash

systemctl status ssh
```

---

# Production Example 3

## Backup Script

```bash
#!/bin/bash

tar -czf backup.tar.gz /home
```

---

# Production Example 4

## User Creation

```bash
#!/bin/bash

sudo useradd testuser

echo "User created"
```

---

# Production Example 5

## Check Disk Usage

```bash
#!/bin/bash

df -h
```

---

# Debugging Script

Run with debugging:

```bash
bash -x script.sh
```

Example:

```
+ echo Hello
Hello
```

---

# Syntax Check

```bash
bash -n script.sh
```

Checks syntax without execution.

---

# Best Practices

✅ Use meaningful variable names.

✅ Add comments.

✅ Check exit status.

✅ Handle errors.

✅ Use functions.

✅ Validate user input.

✅ Test before production.

---

# Common Commands

| Command | Purpose |
|----------|----------|
| chmod +x file.sh | Make executable |
| ./file.sh | Run script |
| bash file.sh | Execute using bash |
| bash -x file.sh | Debug script |
| bash -n file.sh | Check syntax |
| echo $? | Exit status |

---

# Real Production Use Cases

## Daily Backup

Automatically create backups.

---

## Log Cleanup

Delete old log files.

---

## User Management

Create and remove users.

---

## Monitoring

Check CPU, RAM, and disk usage.

---

## Service Restart

Restart failed services automatically.

---

## Application Deployment

Deploy application updates.

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| #!/bin/bash | Bash interpreter |
| chmod +x script.sh | Make executable |
| ./script.sh | Run script |
| read VAR | User input |
| if | Condition |
| for | Loop |
| while | Loop |
| case | Multiple choices |
| function | Reusable code |
| echo $? | Exit status |
| bash -x | Debug |
| bash -n | Syntax check |

---

# Interview Questions

## Q1. What is a shell script?

A shell script is a text file containing Linux commands executed by a shell to automate tasks.

---

## Q2. What is the purpose of `#!/bin/bash`?

It specifies that the script should be executed using the Bash shell.

---

## Q3. How do you make a script executable?

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

## Q5. What does `$0` represent?

The name of the script being executed.

---

## Q6. What does `$?` represent?

The exit status of the last command.

- `0` = Success
- Non-zero = Error

---

## Q7. How do you debug a shell script?

```bash
bash -x script.sh
```

---

## Q8. How do you check a shell script for syntax errors?

```bash
bash -n script.sh
```

This checks the script without executing it.

---