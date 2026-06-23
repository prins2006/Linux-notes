# Rollback Patches and Updates in Linux

## What is Rollback?

Rollback is the process of reverting a system, package, or update to a previous working version when a patch or update causes problems such as:

* Application failures
* Service outages
* Compatibility issues
* Performance degradation
* Security patch conflicts

Rollback is an important production support and system administration task.

---

# **Why Rollback is Needed**

### Example

A server is updated:

```bash
sudo dnf update -y
```

After the update:

* Apache service fails
* Database cannot start
* Application crashes

Instead of troubleshooting immediately, administrators may rollback the update to restore service quickly.

---

# Rollback in RPM-Based Systems

## View Package History

```bash
sudo dnf history
```

Example Output:

```text
ID | Command Line      | Date
-------------------------------------
15 | dnf update        | 2026-06-20
14 | dnf install nginx | 2026-06-18
```

---

## Undo a Specific Transaction

Rollback transaction ID 15:

```bash
sudo dnf history undo 15
```

This removes updated packages and restores previous versions.

---

## Rollback Multiple Transactions

```bash
sudo dnf history rollback 14
```

Returns the system to the state before transaction 14.

---

# Rollback Using yum

Older systems (RHEL 7/CentOS 7):

View history:

```bash
sudo yum history
```

Undo transaction:

```bash
sudo yum history undo 10
```

Rollback:

```bash
sudo yum history rollback 10
```

---

# Downgrade a Package

Install an older version:

```bash
sudo dnf downgrade httpd
```

Example:

```bash
sudo dnf downgrade nginx
```

Verify version:

```bash
rpm -qa | grep nginx
```

---

# Check Installed Package Versions

```bash
rpm -qa
```

Specific package:

```bash
rpm -qi httpd
```

---

# Kernel Rollback

List installed kernels:

```bash
rpm -q kernel
```

Example:

```text
kernel-6.8.0
kernel-6.7.5
kernel-6.6.9
```

---

## Select Older Kernel

Check default kernel:

```bash
grubby --default-kernel
```

Set older kernel:

```bash
sudo grubby --set-default /boot/vmlinuz-6.7.5
```

Reboot:

```bash
sudo reboot
```

Verify:

```bash
uname -r
```

---

# Snapshot-Based Rollback

Production servers often use snapshots before patching.

## LVM Snapshot

Create snapshot:

```bash
lvcreate -L 5G -s -n rootsnap /dev/vg/root
```

Rollback:

```bash
lvconvert --merge /dev/vg/rootsnap
```

Reboot system.

---

# Virtual Machine Rollback

Before patching:

1. Create VM Snapshot.
2. Apply updates.
3. Test applications.
4. Revert snapshot if failure occurs.

Common in:

* VMware
* Hyper-V
* KVM
* VirtualBox

---

# Best Practices Before Updates

### Backup Important Data

```bash
rsync -av /data /backup
```

### Create Snapshot

```bash
lvcreate -s
```

### Document Current Version

```bash
rpm -qa > packages_before_update.txt
```

### Test in Staging Environment

Never patch production directly without testing.

---

# Production Update Workflow

1. Take Backup.
2. Create Snapshot.
3. Run Update.
4. Verify Services.
5. Verify Applications.
6. Monitor Logs.
7. Rollback if Issue Occurs.

---

# Important Commands Summary

```bash
dnf history
dnf history undo ID
dnf history rollback ID
dnf downgrade package_name

yum history
yum history undo ID
yum history rollback ID

rpm -qa
rpm -qi package_name

rpm -q kernel
grubby --default-kernel
uname -r
```

## One-Line Definition

**Rollback is the process of reverting a system, package, kernel, or update to a previous working state when a patch or upgrade causes problems.**
