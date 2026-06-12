# vi vs vim - Detailed Guide with Production Use Cases

# Introduction

`vi` and `vim` are command-line text editors used in Linux and Unix systems for creating and editing files.

- **vi** = Visual Editor (Original Unix editor)
- **vim** = Vi IMproved (Enhanced version of vi)

In production Linux servers, knowing `vim` is an essential skill for Linux administrators, DevOps engineers, Cloud engineers, and Software developers.

---

# History

| Editor | Released | Developer |
|---------|----------|-----------|
| vi | 1976 | Bill Joy |
| vim | 1991 | Bram Moolenaar |

Vim was created to improve the limitations of vi.

---

# Why use vi/vim?

Many production servers do not have graphical editors like VS Code.

Suppose your web server crashes at midnight.

You SSH into the server:

```bash
ssh user@server-ip
```

Need to edit:

```bash
/etc/nginx/nginx.conf
```

You cannot open VS Code.

You use:

```bash
vim /etc/nginx/nginx.conf
```

Fix the configuration.

Save:

```
:wq
```

Restart service:

```bash
sudo systemctl restart nginx
```

Problem solved.

---

# vi Architecture

vi has three major modes.

```
          +-------------+
          | Normal Mode |
          +-------------+
           /     |      \
          /      |       \
         /       |        \
Insert Mode  Visual Mode Command Mode
```

---

# Mode 1: Normal Mode

Default mode.

Used for:

- Navigation
- Copy
- Delete
- Paste
- Search

Example:

Open file:

```bash
vim test.txt
```

Press:

```
Esc
```

Now Normal mode is active.

---

# Cursor Movement

Move left:

```
h
```

Move down:

```
j
```

Move up:

```
k
```

Move right:

```
l
```

Production engineers use these keys instead of arrow keys.

---

# Word Navigation

Next word:

```
w
```

Previous word:

```
b
```

End of word:

```
e
```

---

# Beginning and End

Start of line:

```
0
```

First non-space:

```
^
```

End of line:

```
$
```

First line:

```
gg
```

Last line:

```
G
```

Line number 100:

```
100G
```

---

# Insert Mode

Enter insert mode.

```
i
```

Insert before cursor.

```
a
```

Append after cursor.

```
I
```

Beginning of line.

```
A
```

End of line.

```
o
```

New line below.

```
O
```

New line above.

Exit:

```
Esc
```

---

# Delete Operations

Delete character:

```
x
```

Delete word:

```
dw
```

Delete line:

```
dd
```

Delete 5 lines:

```
5dd
```

Delete to end of line:

```
D
```

---

# Copy Operations

Copy line:

```
yy
```

Copy 3 lines:

```
3yy
```

Paste below:

```
p
```

Paste above:

```
P
```

---

# Undo and Redo

Undo:

```
u
```

Redo:

```
Ctrl+r
```

---

# Search

Search word:

```
/error
```

Next result:

```
n
```

Previous result:

```
N
```

---

# Replace

Replace one character:

```
r
```

Replace whole line:

```
cc
```

Replace word:

```
cw
```

---

# Save and Quit

Save:

```
:w
```

Quit:

```
:q
```

Save and quit:

```
:wq
```

Force quit:

```
:q!
```

Save and exit:

```
:x
```

---

# Production Use Case 1

## Edit Nginx Configuration

Open:

```bash
sudo vim /etc/nginx/nginx.conf
```

Modify server block.

Save:

```
:wq
```

Test:

```bash
sudo nginx -t
```

Restart:

```bash
sudo systemctl restart nginx
```

---

# Production Use Case 2

## Edit SSH Configuration

Open:

```bash
sudo vim /etc/ssh/sshd_config
```

Change:

```
Port 22
```

To:

```
Port 2222
```

Save:

```
:wq
```

Restart:

```bash
sudo systemctl restart sshd
```

---

# Production Use Case 3

## Edit Hosts File

```bash
sudo vim /etc/hosts
```

Add:

```
192.168.1.10 database
```

Save and exit.

---

# Production Use Case 4

## Kubernetes YAML

Open deployment:

```bash
vim deployment.yaml
```

Change replicas:

```yaml
replicas: 3
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

---

# Production Use Case 5

## Docker Compose

Edit:

```bash
vim docker-compose.yml
```

Update image version.

Deploy:

```bash
docker compose up -d
```

---

# Production Use Case 6

## Git Commit Message

Git automatically opens vim:

```bash
git commit
```

Write:

```
Added login validation
```

Save:

```
:wq
```

Commit completes.

---

# Production Use Case 7

## Crontab

Open:

```bash
crontab -e
```

Usually vim opens.

Add:

```
0 2 * * * /home/user/backup.sh
```

Save.

Cron job is installed.

---

# Useful vim Commands

| Command | Purpose |
|----------|----------|
| gg | First line |
| G | Last line |
| dd | Delete line |
| yy | Copy line |
| p | Paste |
| u | Undo |
| Ctrl+r | Redo |
| /text | Search |
| n | Next search |
| :set number | Show line numbers |
| :set nonumber | Hide line numbers |
| :%s/old/new/g | Replace all |
| :w | Save |
| :q | Quit |
| :wq | Save and quit |
| :q! | Quit without saving |

---

# vi vs vim

| Feature | vi | vim |
|----------|----|-----|
| Basic Editing | Yes | Yes |
| Syntax Highlighting | No | Yes |
| Multiple Undo | No | Yes |
| Plugins | No | Yes |
| Split Windows | No | Yes |
| Auto Complete | No | Yes |
| Tabs | No | Yes |
| Programming Support | Limited | Excellent |

---

# Production Interview Questions

## Q1. Why is vim important in DevOps?

Because almost every Linux production server has vim or vi, and engineers use it to edit configuration files remotely.

## Q2. Why not use VS Code?

Production servers usually do not have GUI access. SSH + vim is faster and universally available.

## Q3. How do you quit without saving?

```
:q!
```

## Q4. How do you save and quit?

```
:wq
```

## Q5. How do you search for a word?

```
/word
```

---

# Summary

- `vi` is the original Unix text editor.
- `vim` is an advanced version of `vi`.
- Vim is widely used in Linux administration, DevOps, Docker, Kubernetes, Git, and cloud environments.
- Common production tasks include editing configuration files, YAML files, cron jobs, and Git commit messages.
- Mastering vim improves productivity when working on remote Linux servers.