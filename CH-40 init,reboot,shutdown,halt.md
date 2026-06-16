# Linux System Maintenance Commands (`shutdown`, `init`, `reboot`, `halt`)

# Introduction

System maintenance commands are used to safely:
- Shut down the system.
- Restart the system.
- Change system runlevels/targets.
- Stop the CPU after shutdown.

These commands are commonly used by Linux administrators for:
- System updates.
- Scheduled maintenance.
- Kernel upgrades.
- Emergency shutdowns.
- Production server reboots.

---

# Linux Boot Process Overview

```
BIOS/UEFI
    |
Bootloader (GRUB)
    |
Linux Kernel
    |
systemd (or init)
    |
System Services
    |
User Login
```

System maintenance commands interact mainly with **systemd** (modern Linux) or **init** (older Linux).

---

# 1. shutdown Command

## Purpose

Safely powers off or reboots the system.

It:
- Stops running services.
- Notifies logged-in users.
- Unmounts filesystems.
- Prevents data corruption.

---

## Syntax

```bash
shutdown [OPTION] TIME MESSAGE
```

---

## Shutdown Immediately

```bash
sudo shutdown now
```

Equivalent:

```bash
sudo shutdown 0
```

---

## Shutdown After 10 Minutes

```bash
sudo shutdown +10
```

Message:

```
System will shutdown in 10 minutes.
```

---

## Shutdown at Specific Time

Shutdown at 11 PM:

```bash
sudo shutdown 23:00
```

---

## Broadcast Message

```bash
sudo shutdown +5 "Server maintenance"
```

Users receive:

```
Broadcast message:
Server maintenance
```

---

## Cancel Shutdown

```bash
sudo shutdown -c
```

---

## Reboot Using shutdown

```bash
sudo shutdown -r now
```

---

## Power Off

```bash
sudo shutdown -P now
```

---

# Production Example

Restart server after updates:

```bash
sudo shutdown -r +5 "Kernel update"
```

---

# 2. reboot Command

## Purpose

Restarts the Linux system.

---

## Syntax

```bash
reboot
```

or

```bash
sudo reboot
```

---

## Force Reboot

```bash
sudo reboot -f
```

Use carefully.

---

## How reboot Works

```
Running System
      |
Stop Services
      |
Unmount Filesystems
      |
Restart Kernel
      |
Boot Again
```

---

# Production Example

After kernel upgrade:

```bash
sudo reboot
```

---

# 3. halt Command

## Purpose

Stops the CPU and halts the operating system.

Modern Linux:

```bash
halt
```

usually behaves like:

```bash
systemctl halt
```

---

## Syntax

```bash
sudo halt
```

---

## Force Halt

```bash
sudo halt -f
```

---

## How halt Works

```
Stop Services
      |
Unmount Filesystems
      |
Stop CPU
```

Power may remain on depending on hardware.

---

# Production Example

Emergency system stop.

---

# 4. init Command

## Purpose

Changes the system runlevel.

Older Linux:

```
init
```

Modern Linux:

```
systemd
```

Equivalent command:

```
systemctl isolate
```

---

# Traditional Runlevels

| Runlevel | Purpose |
|-----------|----------|
| 0 | Shutdown |
| 1 | Single-user mode |
| 2 | Multi-user |
| 3 | Multi-user with networking |
| 4 | Custom |
| 5 | Graphical mode |
| 6 | Reboot |

---

## Shutdown

```bash
sudo init 0
```

Equivalent:

```bash
shutdown now
```

---

## Single User Mode

```bash
sudo init 1
```

Used for:
- Password recovery.
- Maintenance.

---

## Multi-user

```bash
sudo init 3
```

Text mode.

---

## GUI Mode

```bash
sudo init 5
```

Desktop environment.

---

## Reboot

```bash
sudo init 6
```

Equivalent:

```bash
sudo reboot
```

---

# Modern systemd Targets

| Old | New |
|------|------|
| 0 | poweroff.target |
| 1 | rescue.target |
| 3 | multi-user.target |
| 5 | graphical.target |
| 6 | reboot.target |

---

## Check Default Target

```bash
systemctl get-default
```

Example:

```
graphical.target
```

---

## Change Target

Text mode:

```bash
sudo systemctl isolate multi-user.target
```

GUI:

```bash
sudo systemctl isolate graphical.target
```

---

# Difference Between shutdown, reboot, halt, and init

| Command | Purpose |
|----------|----------|
| shutdown | Safely shutdown or reboot |
| reboot | Restart system |
| halt | Stop system |
| init | Change runlevels |

---

# Comparison Table

| Feature | shutdown | reboot | halt | init |
|----------|----------|---------|------|------|
| Shutdown | Yes | No | Yes | Yes |
| Restart | Yes | Yes | No | Yes |
| Schedule Time | Yes | No | No | No |
| Notify Users | Yes | No | No | No |
| Runlevel Change | No | No | No | Yes |

---

# Modern Equivalents

| Traditional | systemd |
|-------------|----------|
| shutdown | systemctl poweroff |
| reboot | systemctl reboot |
| halt | systemctl halt |
| init 0 | systemctl poweroff |
| init 6 | systemctl reboot |
| init 1 | systemctl rescue |

---

# Production Scenarios

## System Maintenance

Notify users:

```bash
sudo shutdown +15 "System maintenance"
```

---

## Kernel Update

```bash
sudo reboot
```

---

## Password Recovery

Boot into:

```bash
sudo init 1
```

or

```bash
sudo systemctl rescue
```

---

## Emergency Halt

```bash
sudo halt
```

---

# Important Difference

## shutdown vs reboot

| shutdown | reboot |
|-----------|---------|
| Shutdown or restart | Restart only |
| Can schedule | Immediate |
| Notify users | No |
| Maintenance friendly | Yes |

---

## reboot vs halt

| reboot | halt |
|----------|------|
| Restart system | Stop system |
| Boot again | No reboot |

---

## init vs systemd

| init | systemd |
|-------|----------|
| Old Linux | Modern Linux |
| Runlevels | Targets |
| Slower | Faster |
| Limited features | Advanced service management |

---

# Interview Questions

## Q1. What does shutdown do?

Safely powers off or restarts the system.

---

## Q2. How do you reboot immediately?

```bash
sudo reboot
```

or

```bash
sudo shutdown -r now
```

---

## Q3. What does init 6 do?

Reboots the system.

---

## Q4. What does init 0 do?

Shuts down the system.

---

## Q5. What is the difference between halt and shutdown?

- `halt` stops the system immediately.
- `shutdown` safely stops services and can schedule shutdowns.

---

## Q6. What replaced init in modern Linux?

```
systemd
```

---

# Production Best Practices

- Always use `shutdown` for planned maintenance.
- Notify users before restarting production servers.
- Use `reboot` after kernel or critical updates.
- Use `systemctl` commands on modern Linux distributions.
- Avoid forced (`-f`) options unless absolutely necessary.
- Verify active users before shutting down:

```bash
who
```

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `shutdown now` | Shutdown immediately |
| `shutdown -r now` | Reboot immediately |
| `shutdown +10` | Shutdown after 10 minutes |
| `shutdown -c` | Cancel shutdown |
| `reboot` | Restart system |
| `halt` | Stop system |
| `init 0` | Shutdown |
| `init 1` | Rescue mode |
| `init 6` | Reboot |
| `systemctl reboot` | Modern reboot |
| `systemctl poweroff` | Modern shutdown |

---

# Summary

- **`shutdown`** safely shuts down or reboots a Linux system and supports scheduling and user notifications.
- **`reboot`** restarts the system immediately.
- **`halt`** stops the operating system and CPU.
- **`init`** manages runlevels in traditional Linux systems and has largely been replaced by **systemd** targets.
- Modern Linux systems prefer `systemctl reboot`, `systemctl poweroff`, and `systemctl rescue` for system maintenance operations.