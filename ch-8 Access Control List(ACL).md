# Linux Access Control List (ACL)

## What is ACL?

**ACL (Access Control List)** is a Linux feature that provides **more flexible file and directory permissions** than the standard owner, group, and others model.

With ACL, you can grant specific permissions to individual users or groups without changing the file owner or group.

---

# Why Do We Need ACL?

Standard Linux permissions support only:

* Owner
* Group
* Others

Suppose:

* Owner: `prins`
* Group: `developers`
* Others: Everyone else

What if you want to give user `rahul` read and write access to a file, but he is not the owner or a member of the group?

Standard permissions cannot do this.

ACL solves this problem.

---

# Standard Permissions vs ACL

## Standard Permissions

```text
Owner  Group  Others
 rwx    r-x    r--
```

Only three categories exist.

---

## ACL Permissions

```text
Owner
Group
Others
User: rahul
User: amit
Group: testing
```

ACL allows additional permission entries.

---

# Check ACL Support

Check mounted file systems:

```bash
mount
```

Most modern Linux distributions support ACL by default.

---

# ACL Commands

| Command | Purpose     |
| ------- | ----------- |
| getfacl | Display ACL |
| setfacl | Set ACL     |

---

# View ACL

Syntax:

```bash
getfacl filename
```

Example:

```bash
getfacl notes.txt
```

Output:

```text
# file: notes.txt
# owner: prins
# group: developers

user::rw-
user:rahul:rwx
group::r--
mask::rwx
other::r--
```

---

# Understanding the Output

```text
user::rw-
```

Owner permissions.

---

```text
user:rahul:rwx
```

User rahul has:

* Read
* Write
* Execute

---

```text
group::r--
```

Group permissions.

---

```text
other::r--
```

Permissions for everyone else.

---

# Grant ACL Permission

Syntax:

```bash
setfacl -m u:username:permissions filename
```

Example:

```bash
setfacl -m u:rahul:rwx notes.txt
```

Meaning:

| Part      | Meaning    |
| --------- | ---------- |
| u         | User       |
| rahul     | Username   |
| rwx       | Permission |
| notes.txt | File       |

---

# Check Result

```bash
getfacl notes.txt
```

Output:

```text
user:rahul:rwx
```

---

# Give Read Permission

```bash
setfacl -m u:rahul:r notes.txt
```

---

# Give Read and Write

```bash
setfacl -m u:rahul:rw notes.txt
```

---

# Set ACL for Group

Syntax:

```bash
setfacl -m g:groupname:permissions filename
```

Example:

```bash
setfacl -m g:testing:rwx project.txt
```

---

# Remove ACL for User

Syntax:

```bash
setfacl -x u:username filename
```

Example:

```bash
setfacl -x u:rahul notes.txt
```

---

# Remove ACL for Group

```bash
setfacl -x g:testing project.txt
```

---

# Remove All ACL Entries

```bash
setfacl -b notes.txt
```

---

# ACL on Directories

Give user rahul full access:

```bash
setfacl -m u:rahul:rwx projects
```

---

# Default ACL

Default ACL is inherited by new files and directories.

Syntax:

```bash
setfacl -d -m u:rahul:rwx projects
```

New files inside `projects` automatically get ACL entries.

---

# Practical Example

Suppose:

File:

```text
salary.xlsx
```

Owner:

```text
prins
```

Need:

* Rahul can read and write.
* Amit can only read.

Commands:

```bash
setfacl -m u:rahul:rw salary.xlsx
setfacl -m u:amit:r salary.xlsx
```

Check:

```bash
getfacl salary.xlsx
```

---

# Difference Between chmod and ACL

| chmod                       | ACL                                |
| --------------------------- | ---------------------------------- |
| Owner, Group, Others        | Individual users and groups        |
| Limited                     | Flexible                           |
| Simple                      | Advanced                           |
| Basic permission management | Fine-grained permission management |

---

# Important ACL Commands

View ACL:

```bash
getfacl file
```

Add user ACL:

```bash
setfacl -m u:user:rwx file
```

Add group ACL:

```bash
setfacl -m g:group:rwx file
```

Remove user ACL:

```bash
setfacl -x u:user file
```

Remove all ACL:

```bash
setfacl -b file
```

Set default ACL:

```bash
setfacl -d -m u:user:rwx directory
```

---

# Real-World Use Cases

## Shared Project Directory

Developers need different access levels.

ACL allows custom permissions.

---

## Team Collaboration

Different users can have different permissions on the same file.

---

## Backup Scripts

Grant backup users read-only access.

---

## Department Files

HR, Finance, and IT can have different permissions without changing ownership.

---

# Interview Questions

## What is ACL?

ACL (Access Control List) is a Linux feature that provides fine-grained file and directory permissions.

---

## Why is ACL used?

To give specific users or groups custom permissions beyond standard Linux permissions.

---

## Which command displays ACL?

```bash
getfacl filename
```

---

## Which command modifies ACL?

```bash
setfacl
```

---

## What is the difference between chmod and ACL?

* `chmod` manages owner, group, and others permissions.
* ACL allows permissions for individual users and additional groups.

---

# Summary

* ACL stands for Access Control List.
* ACL extends standard Linux permissions.
* Main commands:

```bash
getfacl file
setfacl -m u:user:rwx file
setfacl -x u:user file
setfacl -b file
setfacl -d -m u:user:rwx directory
```

* ACL is useful for shared directories, team projects, and fine-grained access control.
* ACL works alongside standard Linux file permissions and provides additional flexibility.
