# Linux Aliases (`alias`)

## Introduction

An **alias** in Linux is a shortcut for a command or a group of commands. Aliases help reduce typing, improve productivity, and standardize frequently used commands.

### Why Use Aliases?

* Save time.
* Avoid typing long commands.
* Prevent accidental mistakes.
* Create custom shortcuts.
* Simplify complex commands.

---

# What is an Alias?

An alias maps a short name to a command.

For example:

Instead of typing:

```bash
ls -alF
```

You can create:

```bash
alias ll='ls -alF'
```

Now simply type:

```bash
ll
```

Output:

```
total 20
drwxr-xr-x ...
-rw-r--r-- ...
```

---

# Syntax

```bash
alias alias_name='command'
```

Example:

```bash
alias l='ls -l'
```

Now:

```bash
l
```

is equivalent to:

```bash
ls -l
```

---

# View Existing Aliases

Display all aliases:

```bash
alias
```

Example output:

```bash
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
```

---

# Create Temporary Aliases

Temporary aliases work only for the current terminal session.

Example:

```bash
alias cls='clear'
```

Now:

```bash
cls
```

clears the terminal.

After closing the terminal, the alias disappears.

---

# Common Aliases

## List Files

```bash
alias ll='ls -l'
```

## List Hidden Files

```bash
alias la='ls -la'
```

## Human Readable Disk Usage

```bash
alias df='df -h'
```

## Human Readable Memory

```bash
alias free='free -m'
```

## Clear Screen

```bash
alias c='clear'
```

## Current Date

```bash
alias today='date'
```

---

# Command with Multiple Options

Example:

```bash
alias grep='grep --color=auto'
```

Search:

```bash
grep root /etc/passwd
```

Matches appear in color.

---

# Alias with Multiple Commands

```bash
alias update='sudo apt update && sudo apt upgrade'
```

Run:

```bash
update
```

Equivalent to:

```bash
sudo apt update
sudo apt upgrade
```

---

# Remove an Alias

Syntax:

```bash
unalias alias_name
```

Example:

```bash
unalias ll
```

Remove all aliases:

```bash
unalias -a
```

---

# Permanent Aliases

Temporary aliases disappear after logout.

To make them permanent, add them to your shell configuration file.

## Bash Users

Open:

```bash
nano ~/.bashrc
```

Add:

```bash
alias ll='ls -alF'
alias update='sudo apt update && sudo apt upgrade'
alias cls='clear'
```

Save and reload:

```bash
source ~/.bashrc
```

---

## System-wide Aliases

Location:

```bash
/etc/bash.bashrc
```

or

```bash
/etc/profile
```

System administrators can create aliases for all users.

---

# Production Examples

## Safe Delete

Prevent accidental deletion.

```bash
alias rm='rm -i'
```

Before deleting:

```
remove regular file?
```

---

## Safe Copy

```bash
alias cp='cp -i'
```

---

## Safe Move

```bash
alias mv='mv -i'
```

---

## Check Disk Space

```bash
alias disk='df -h'
```

---

## Check Memory

```bash
alias memory='free -m'
```

---

## Network Information

```bash
alias myip='hostname -I'
```

---

## Find Large Files

```bash
alias bigfiles='du -ah | sort -rh | head -10'
```

---

## Git Shortcuts

Git users often create aliases.

```bash
alias gs='git status'
```

```bash
alias ga='git add .'
```

```bash
alias gc='git commit -m'
```

```bash
alias gp='git push'
```

---

# Check an Alias

Example:

```bash
alias ll
```

Output:

```bash
alias ll='ls -alF'
```

---

# Override an Existing Alias

Suppose:

```bash
alias ls='ls --color=auto'
```

To use the original command:

```bash
\ls
```

or

```bash
command ls
```

---

# Alias vs Shell Script

| Alias           | Shell Script          |
| --------------- | --------------------- |
| Short command   | Multiple commands     |
| Interactive use | Automation            |
| Simple          | Complex               |
| Current shell   | Standalone executable |

Example Alias:

```bash
alias ll='ls -l'
```

Example Script:

```bash
#!/bin/bash

echo "System Information"
date
hostname
```

---

# Production Use Cases

## Daily System Check

```bash
alias health='df -h && free -m && uptime'
```

Run:

```bash
health
```

Shows:

* Disk usage
* Memory usage
* System uptime

---

## Service Status

```bash
alias sshstatus='systemctl status ssh'
```

---

## Restart Service

```bash
alias restartweb='sudo systemctl restart apache2'
```

---

# Interview Questions

## Q1. What is an alias?

An alias is a shortcut name for a Linux command or a sequence of commands.

---

## Q2. How do you create an alias?

```bash
alias ll='ls -l'
```

---

## Q3. How do you list all aliases?

```bash
alias
```

---

## Q4. How do you remove an alias?

```bash
unalias alias_name
```

Example:

```bash
unalias ll
```

---

## Q5. Where are permanent Bash aliases stored?

User-specific:

```bash
~/.bashrc
```

System-wide:

```bash
/etc/bash.bashrc
```

or

```bash
/etc/profile
```

---

## Q6. What is the difference between a temporary and permanent alias?

| Temporary                  | Permanent                     |
| -------------------------- | ----------------------------- |
| Current session only       | Available after login         |
| Lost after terminal closes | Stored in configuration files |

---

# Most Useful Linux Administrator Aliases

| Alias    | Command                      |
| -------- | ---------------------------- |
| `ll`     | `ls -alF`                    |
| `la`     | `ls -A`                      |
| `cls`    | `clear`                      |
| `df`     | `df -h`                      |
| `free`   | `free -m`                    |
| `rm`     | `rm -i`                      |
| `cp`     | `cp -i`                      |
| `mv`     | `mv -i`                      |
| `myip`   | `hostname -I`                |
| `health` | `df -h && free -m && uptime` |
| `gs`     | `git status`                 |

## Best Practice

* Use aliases for frequently used commands.
* Use interactive options (`-i`) for potentially destructive commands like `rm`, `cp`, and `mv`.
* Store personal aliases in `~/.bashrc`.
* Reload changes with:

```bash
source ~/.bashrc
```

This makes the new aliases available without logging out and back in.
