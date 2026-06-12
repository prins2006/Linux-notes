# sed Command in Linux

## What is `sed`?

`sed` stands for **Stream Editor**.

It is a command-line utility used to:
- Search text
- Replace text
- Delete lines
- Insert lines
- Append text
- Print specific lines
- Edit files automatically

Unlike a text editor (`vi`/`vim`), `sed` processes text line by line.

---

# Syntax

```bash
sed [options] 'command' filename
```

Example:

```bash
sed 's/Linux/Ubuntu/' file.txt
```

---

# Sample File

Create a file:

```bash
cat > employee.txt
```

Content:

```
John Developer
Alice Tester
Bob DevOps
David Manager
Alice HR
```

Display file:

```bash
cat employee.txt
```

Output:

```
John Developer
Alice Tester
Bob DevOps
David Manager
Alice HR
```

---

# 1. Replace First Occurrence

Syntax:

```bash
sed 's/old/new/' file
```

Example:

```bash
sed 's/Alice/Emma/' employee.txt
```

Output:

```
John Developer
Emma Tester
Bob DevOps
David Manager
Alice HR
```

Only the first "Alice" is replaced.

---

# 2. Replace All Occurrences

```bash
sed 's/Alice/Emma/g' employee.txt
```

Output:

```
John Developer
Emma Tester
Bob DevOps
David Manager
Emma HR
```

`g` means Global.

---

# 3. Replace in a Specific Line

Replace on line 2 only:

```bash
sed '2 s/Alice/Emma/' employee.txt
```

Output:

```
John Developer
Emma Tester
Bob DevOps
David Manager
Alice HR
```

---

# 4. Print Specific Line

Print line 3:

```bash
sed -n '3p' employee.txt
```

Output:

```
Bob DevOps
```

---

# 5. Print Multiple Lines

```bash
sed -n '2,4p' employee.txt
```

Output:

```
Alice Tester
Bob DevOps
David Manager
```

---

# 6. Delete a Line

Delete line 2:

```bash
sed '2d' employee.txt
```

Output:

```
John Developer
Bob DevOps
David Manager
Alice HR
```

---

# 7. Delete Multiple Lines

```bash
sed '2,4d' employee.txt
```

---

# 8. Delete Empty Lines

Suppose:

```
A

B

C
```

Command:

```bash
sed '/^$/d' file.txt
```

Output:

```
A
B
C
```

---

# 9. Insert Text Before a Line

```bash
sed '2i New Employee' employee.txt
```

Output:

```
John Developer
New Employee
Alice Tester
Bob DevOps
David Manager
Alice HR
```

---

# 10. Append Text After a Line

```bash
sed '2a Team Lead' employee.txt
```

Output:

```
John Developer
Alice Tester
Team Lead
Bob DevOps
David Manager
Alice HR
```

---

# 11. Change Entire Line

```bash
sed '3c Bob Senior DevOps' employee.txt
```

Output:

```
John Developer
Alice Tester
Bob Senior DevOps
David Manager
Alice HR
```

---

# 12. In-place Editing

Edit the actual file.

Before:

```bash
cat employee.txt
```

```
Alice Tester
```

Run:

```bash
sed -i 's/Alice/Emma/g' employee.txt
```

Check:

```bash
cat employee.txt
```

```
Emma Tester
```

---

# 13. Backup Before Editing

```bash
sed -i.bak 's/Alice/Emma/g' employee.txt
```

Creates:

```
employee.txt
employee.txt.bak
```

---

# 14. Ignore Case

```bash
sed 's/linux/Linux/I' file.txt
```

Matches:

```
linux
Linux
LINUX
```

---

# 15. Replace Multiple Spaces

```bash
sed 's/  */ /g' file.txt
```

---

# Production Use Cases

## 1. Update Configuration File

Change port:

Before:

```
port=8080
```

Command:

```bash
sed -i 's/8080/9090/' app.conf
```

---

## 2. Update Kubernetes YAML

Before:

```yaml
replicas: 2
```

Command:

```bash
sed -i 's/replicas: 2/replicas: 5/' deployment.yaml
```

---

## 3. Change Docker Image Version

Before:

```
nginx:1.25
```

Command:

```bash
sed -i 's/1.25/1.26/' docker-compose.yml
```

---

## 4. Replace IP Address

```bash
sed -i 's/192.168.1.10/192.168.1.20/g' config.txt
```

---

## 5. Remove Comment Lines

Configuration file:

```
# Test
port=8080
# Data
```

Command:

```bash
sed '/^#/d' config.txt
```

Output:

```
port=8080
```

---

# sed with Pipe

```bash
cat employee.txt | sed 's/Developer/Engineer/'
```

---

# sed with grep

```bash
grep Alice employee.txt | sed 's/Alice/Emma/'
```

---

# Common sed Commands

| Command | Purpose |
|----------|----------|
| s/old/new/ | Replace first |
| s/old/new/g | Replace all |
| 2p | Print line 2 |
| 2d | Delete line 2 |
| 2i | Insert before line 2 |
| 2a | Append after line 2 |
| 2c | Change line 2 |
| /^$/d | Delete blank lines |
| /^#/d | Delete comments |
| -i | Edit file |
| -i.bak | Backup and edit |
| -n | Suppress automatic output |

---

# sed vs awk

| Feature | sed | awk |
|----------|------|------|
| Replace text | Yes | Yes |
| Delete lines | Yes | Yes |
| Pattern matching | Good | Excellent |
| Column processing | Limited | Excellent |
| Reports | No | Yes |
| Arithmetic | Limited | Yes |

---

# Interview Questions

## What is sed?

`sed` is a Stream Editor used to search, replace, insert, delete, and modify text in files or streams.

## What does `-i` do?

Edits the original file directly.

Example:

```bash
sed -i 's/old/new/g' file.txt
```

## What does `g` mean?

Global replacement (replace all occurrences in a line).

## How do you print line 5?

```bash
sed -n '5p' file.txt
```

## How do you delete blank lines?

```bash
sed '/^$/d' file.txt
```

---

# Production-Level Interview Use Cases

Linux/DevOps engineers commonly use `sed` for:

- Updating configuration files
- Editing Nginx and Apache configs
- Changing environment variables
- Updating Docker Compose files
- Modifying Kubernetes YAML manifests
- CI/CD pipeline automation
- Bulk text replacement across files
- Cleaning log files
- Automating deployment scripts

---

# Summary

- `sed` = Stream Editor.
- Processes text line by line.
- Can search, replace, insert, append, delete, and print text.
- `-i` edits files directly.
- Widely used in Linux administration, DevOps, Shell scripting, Docker, Kubernetes, and CI/CD automation.
- One of the most important Linux commands for production environments.