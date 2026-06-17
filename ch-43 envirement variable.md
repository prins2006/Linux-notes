# Linux Environment Variables

## What are Environment Variables?

Environment variables are dynamic values stored by the operating system that affect the behavior of processes and applications.

They store information such as:
- User name
- Home directory
- Current shell
- System PATH
- Language settings
- Terminal type

Applications and shell scripts use these variables to determine how they should run.

---

# Why are Environment Variables Important?

Environment variables help:
- Configure applications.
- Locate executable programs.
- Store user-specific settings.
- Pass configuration data between processes.
- Avoid hardcoding paths in scripts.

---

# Types of Environment Variables

## 1. System Environment Variables

Available to all users.

Examples:
```
PATH
HOME
SHELL
LANG
HOSTNAME
```

---

## 2. User Environment Variables

Available only to a specific user.

Example:

```bash
export PROJECT=DevOps
```

---

# Display Environment Variables

## Print all environment variables

```bash
printenv
```

or

```bash
env
```

Example:

```
HOME=/home/prins
USER=prins
SHELL=/bin/bash
PATH=/usr/local/bin:/usr/bin:/bin
```

---

## Display a specific variable

Syntax:

```bash
echo $VARIABLE
```

Example:

```bash
echo $HOME
```

Output:

```
/home/prins
```

---

# Common Environment Variables

| Variable | Purpose |
|----------|----------|
| HOME | User home directory |
| USER | Current username |
| PATH | Executable search path |
| PWD | Current working directory |
| SHELL | Current shell |
| HOSTNAME | System hostname |
| LANG | Language setting |
| TERM | Terminal type |
| LOGNAME | Login username |
| OLDPWD | Previous directory |

---

# HOME Variable

Displays user's home directory.

```bash
echo $HOME
```

Example:

```
/home/prins
```

---

# USER Variable

Displays current user.

```bash
echo $USER
```

Example:

```
prins
```

---

# PWD Variable

Displays current working directory.

```bash
echo $PWD
```

Example:

```
/home/prins/Documents
```

---

# SHELL Variable

Displays current shell.

```bash
echo $SHELL
```

Example:

```
/bin/bash
```

---

# HOSTNAME Variable

Displays system hostname.

```bash
echo $HOSTNAME
```

Example:

```
server01
```

---

# PATH Variable

## What is PATH?

PATH tells Linux where to search for executable commands.

View PATH:

```bash
echo $PATH
```

Example:

```
/usr/local/bin:/usr/bin:/bin:/usr/sbin
```

Directories are separated by a colon (`:`).

---

## How PATH Works

When you type:

```bash
ls
```

Linux searches:

```
/usr/local/bin
/usr/bin
/bin
/usr/sbin
```

until it finds the executable.

Check location:

```bash
which ls
```

Output:

```
/usr/bin/ls
```

---

# Set Temporary Environment Variable

Example:

```bash
export PROJECT=Linux
```

Check:

```bash
echo $PROJECT
```

Output:

```
Linux
```

This variable exists only for the current session.

---

# Remove Environment Variable

```bash
unset PROJECT
```

Check:

```bash
echo $PROJECT
```

Output:

```

```

---

# Create Permanent Environment Variable

## Bash User

Edit:

```bash
nano ~/.bashrc
```

Add:

```bash
export PROJECT=Linux
```

Save and reload:

```bash
source ~/.bashrc
```

Verify:

```bash
echo $PROJECT
```

---

# System-Wide Environment Variables

Edit:

```bash
sudo nano /etc/environment
```

Example:

```
JAVA_HOME=/usr/lib/jvm/java-21-openjdk
```

Reload:

```bash
source /etc/environment
```

---

# Add Directory to PATH

Suppose custom scripts are stored in:

```
/home/prins/scripts
```

Add to PATH:

```bash
export PATH=$PATH:/home/prins/scripts
```

Verify:

```bash
echo $PATH
```

Now scripts can be executed from anywhere.

---

# Display All Shell Variables

```bash
set
```

Shows:
- Environment variables
- Shell variables
- Functions

---

# List Exported Variables

```bash
export
```

Example:

```
declare -x HOME="/home/prins"
declare -x USER="prins"
```

---

# Environment Variables in Scripts

Create:

```bash
nano test.sh
```

Content:

```bash
#!/bin/bash

echo "User: $USER"
echo "Home: $HOME"
echo "Shell: $SHELL"
echo "Path: $PATH"
```

Run:

```bash
chmod +x test.sh
./test.sh
```

Output:

```
User: prins
Home: /home/prins
Shell: /bin/bash
Path: /usr/bin:/bin
```

---

# Child Process Environment

Create variable:

```bash
export PROJECT=DevOps
```

Start new shell:

```bash
bash
```

Check:

```bash
echo $PROJECT
```

Output:

```
DevOps
```

Exported variables are inherited by child processes.

---

# Useful Commands

## Display environment

```bash
env
```

---

## Display all exported variables

```bash
export
```

---

## Display shell variables

```bash
set
```

---

## Display PATH

```bash
echo $PATH
```

---

## Find executable

```bash
which python3
```

---

## Create variable

```bash
export NAME=Linux
```

---

## Remove variable

```bash
unset NAME
```

---

# Important Files

| File | Purpose |
|-------|----------|
| ~/.bashrc | User shell configuration |
| ~/.profile | User login settings |
| ~/.bash_profile | Bash login configuration |
| /etc/environment | System-wide environment variables |
| /etc/profile | Global shell settings |

---

# Production Use Cases

## Java

```bash
export JAVA_HOME=/usr/lib/jvm/java-21-openjdk
```

---

## Python

```bash
export PYTHONPATH=/opt/python/libs
```

---

## Kubernetes

```bash
export KUBECONFIG=/home/user/.kube/config
```

---

## Custom Scripts

```bash
export PATH=$PATH:/opt/scripts
```

---

## Application Configuration

```bash
export APP_ENV=production
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| env | Display environment variables |
| printenv | Print environment |
| echo $HOME | Home directory |
| echo $PATH | Display PATH |
| echo $USER | Current user |
| export VAR=value | Create variable |
| unset VAR | Remove variable |
| export | List exported variables |
| set | List shell variables |
| which command | Find executable location |
| source ~/.bashrc | Reload bash configuration |

---

# Interview Questions

## Q1. What are environment variables?

Environment variables are key-value pairs that store configuration information used by the operating system and applications.

---

## Q2. What is the PATH variable?

PATH is a list of directories where Linux searches for executable commands.

---

## Q3. What is the difference between `env` and `set`?

- `env` displays environment variables.
- `set` displays shell variables, environment variables, and shell functions.

---

## Q4. What is the purpose of `export`?

`export` makes a variable available to child processes.

Example:

```bash
export PROJECT=Linux
```

---

## Q5. How do you create a permanent environment variable?

Add the variable to:

```bash
~/.bashrc
```

or

```bash
/etc/environment
```

and reload the configuration using:

```bash
source ~/.bashrc
```

---

## Q6. What is the difference between temporary and permanent environment variables?

- Temporary variables exist only for the current shell session.
- Permanent variables are loaded automatically whenever a user logs in.

---