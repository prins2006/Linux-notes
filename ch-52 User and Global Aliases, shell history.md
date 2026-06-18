# Linux Shell Scripting

# 148. User and Global Aliases

# 149. Shell History (`history`)

---

# 148. User and Global Aliases

## What is an Alias?

An alias is a shortcut for a Linux command.

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

Linux executes:

```bash
ls -alF
```

---

# Types of Aliases

There are two types:

1. User Aliases
2. Global Aliases

---

# User Aliases

User aliases are available only for a particular user.

Usually stored in:

```bash
~/.bashrc
```

or

```bash
~/.bash_aliases
```

---

## Creating a User Alias

Example:

```bash
alias ll='ls -alF'
```

Test:

```bash
ll
```

Output:

```
total 100
drwx------
-rw-r--r--
```

---

## Check Existing Aliases

```bash
alias
```

Example:

```
alias ll='ls -alF'
alias la='ls -A'
```

---

## Create Multiple User Aliases

```bash
alias cls='clear'

alias df='df -h'

alias free='free -m'

alias myip='hostname -I'
```

---

## Permanent User Alias

Open:

```bash
nano ~/.bashrc
```

Add:

```bash
alias ll='ls -alF'
alias cls='clear'
alias myip='hostname -I'
```

Reload:

```bash
source ~/.bashrc
```

Check:

```bash
ll
```

---

# ~/.bash_aliases

Some Linux distributions use:

```bash
~/.bash_aliases
```

Create:

```bash
nano ~/.bash_aliases
```

Example:

```bash
alias update='sudo apt update && sudo apt upgrade'

alias health='df -h && free -m && uptime'
```

Reload:

```bash
source ~/.bashrc
```

---

# Global Aliases

Global aliases are available for every user.

Usually stored in:

```bash
/etc/bash.bashrc
```

or

```bash
/etc/profile
```

Some systems use:

```bash
/ect/profile.d/
```

---

## Creating Global Alias

Edit:

```bash
sudo nano /etc/bash.bashrc
```

Add:

```bash
alias ll='ls -alF'

alias rm='rm -i'

alias cp='cp -i'

alias mv='mv -i'
```

Save.

Reload:

```bash
source /etc/bash.bashrc
```

Now every user gets these aliases.

---

# Production Safe Aliases

## Safe Remove

```bash
alias rm='rm -i'
```

Confirmation:

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

## System Health

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

## Git Aliases

```bash
alias gs='git status'

alias ga='git add .'

alias gc='git commit -m'

alias gp='git push'
```

---

# Remove Alias

Single alias:

```bash
unalias ll
```

Remove all:

```bash
unalias -a
```

---

# User Alias vs Global Alias

| User Alias        | Global Alias                   |
| ----------------- | ------------------------------ |
| Single user       | All users                      |
| ~/.bashrc         | /etc/bash.bashrc               |
| No sudo           | Root required                  |
| Personal commands | Standard organization commands |

---

# Production Example

Company-wide safe aliases:

```bash
alias rm='rm -i'

alias cp='cp -i'

alias mv='mv -i'

alias df='df -h'

alias free='free -m'
```

---

# 149. Shell History

## What is Shell History?

Linux records previously executed commands.

This feature helps users:

* Repeat commands.
* Troubleshoot.
* Audit activities.
* Increase productivity.

---

# history Command

Display command history.

```bash
history
```

Example:

```
1 pwd
2 ls
3 cd /etc
4 cat passwd
5 history
```

---

# Show Last 10 Commands

```bash
history 10
```

Output:

```
45 ls
46 pwd
47 cd
48 history
```

---

# Execute Previous Commands

Run command number 45:

```bash
!45
```

Linux executes command 45.

---

# Repeat Previous Command

```bash
!!
```

Example:

```
sudo !!
```

Suppose:

```
apt update
```

Permission denied.

Run:

```
sudo !!
```

Linux executes:

```
sudo apt update
```

---

# Execute Last Matching Command

Example:

```bash
!ls
```

Runs the most recent command beginning with:

```
ls
```

---

# History File

History is stored in:

```bash
~/.bash_history
```

View:

```bash
cat ~/.bash_history
```

---

# Clear History

Current session:

```bash
history -c
```

Clear file:

```bash
history -w
```

Remove history file:

```bash
rm ~/.bash_history
```

---

# Append History

Write current session:

```bash
history -a
```

---

# Reload History

```bash
history -r
```

---

# Search History

Press:

```
Ctrl + R
```

Type:

```
ssh
```

Example:

```
(reverse-i-search)
```

Press Enter to execute.

---

# History Environment Variables

## HISTSIZE

Commands stored in memory.

Check:

```bash
echo $HISTSIZE
```

Set:

```bash
HISTSIZE=5000
```

---

## HISTFILESIZE

Commands stored in history file.

Check:

```bash
echo $HISTFILESIZE
```

Set:

```bash
HISTFILESIZE=10000
```

---

## HISTFILE

History file location.

Check:

```bash
echo $HISTFILE
```

Output:

```
/home/user/.bash_history
```

---

# Ignore Duplicate Commands

```bash
export HISTCONTROL=ignoredups
```

---

# Ignore Commands Starting with Space

```bash
export HISTCONTROL=ignorespace
```

Example:

```
 ls
```

Leading space prevents saving.

---

# Timestamp History

Add to:

```bash
~/.bashrc
```

```bash
export HISTTIMEFORMAT="%F %T "
```

Reload:

```bash
source ~/.bashrc
```

Now:

```bash
history
```

Output:

```
2026-06-18 10:15:20 ls
2026-06-18 10:16:00 pwd
```

---

# Production Example

System administrator checks:

```bash
history
```

Finds:

```
sudo systemctl restart ssh

sudo useradd test

sudo passwd test
```

Useful for:

* Troubleshooting
* Auditing
* Security investigations

---

# Interview Questions

## Q1. What is an alias?

A shortcut for a command.

---

## Q2. Where are user aliases stored?

```
~/.bashrc
```

or

```
~/.bash_aliases
```

---

## Q3. Where are global aliases stored?

```
/etc/bash.bashrc
```

or

```
/etc/profile
```

---

## Q4. Which command displays command history?

```bash
history
```

---

## Q5. What does `!!` do?

Repeats the previous command.

---

## Q6. What does `!100` do?

Executes command number 100 from history.

---

## Q7. Which shortcut searches command history?

```
Ctrl + R
```

---

# Summary

| Feature                | Command              |
| ---------------------- | -------------------- |
| Create alias           | `alias ll='ls -alF'` |
| List aliases           | `alias`              |
| Remove alias           | `unalias`            |
| User alias file        | `~/.bashrc`          |
| Global alias file      | `/etc/bash.bashrc`   |
| Show history           | `history`            |
| Repeat last command    | `!!`                 |
| Execute command number | `!n`                 |
| Search history         | `Ctrl + R`           |
| Clear history          | `history -c`         |
| History file           | `~/.bash_history`    |
| Reload history         | `history -r`         |
| Append history         | `history -a`         |

## Production Best Practices

1. Use user aliases for personal shortcuts.
2. Use global aliases for organization-wide standards.
3. Create safe aliases for `rm`, `cp`, and `mv`.
4. Enable timestamped history with `HISTTIMEFORMAT`.
5. Use `Ctrl + R` for fast command retrieval.
6. Monitor `~/.bash_history` for troubleshooting and auditing.
