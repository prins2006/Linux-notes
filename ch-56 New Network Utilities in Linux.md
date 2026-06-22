# New Network Utilities in Linux
## (nmtui, nmcli, nm-connection-editor, and GNOME Settings)

# Introduction

Modern Linux distributions such as:
- RHEL 8/9
- Rocky Linux
- AlmaLinux
- Fedora
- Ubuntu
- Debian

use **NetworkManager** to manage network connections.

Earlier, administrators manually edited configuration files like:

```
/etc/sysconfig/network-scripts/
```

or

```
/etc/network/interfaces
```

Nowadays, NetworkManager provides several tools to manage networking.

| Utility | Interface Type | Best For |
|----------|-------------|----------|
| nmcli | Command Line | Servers and automation |
| nmtui | Text User Interface | SSH and terminal management |
| nm-connection-editor | GUI Application | Desktop users |
| GNOME Settings | Desktop GUI | Basic network configuration |

---

# 1. NetworkManager

## What is NetworkManager?

NetworkManager is a Linux service that automatically detects and configures network devices.

It manages:

- Ethernet
- Wi-Fi
- VPN
- Mobile Broadband
- VLAN
- Bridges
- Bonding
- Teaming
- DNS
- Routing

Service:

```
NetworkManager.service
```

Check status:

```bash
systemctl status NetworkManager
```

Start service:

```bash
sudo systemctl start NetworkManager
```

Enable at boot:

```bash
sudo systemctl enable NetworkManager
```

Check version:

```bash
nmcli --version
```

Example:

```
nmcli tool, version 1.46
```

---

# 2. nmcli

# Meaning

```
nm = Network Manager
cli = Command Line Interface
```

nmcli is a command-line tool to control NetworkManager.

## Use Cases

Production servers

Remote SSH management

Automation scripts

Cloud servers

Network troubleshooting

DevOps environments

---

# Basic Commands

## Show network devices

```bash
nmcli device
```

Example:

```
DEVICE TYPE STATE CONNECTION

ens33 ethernet connected Wired
lo loopback unmanaged --
```

---

## Show all connections

```bash
nmcli connection show
```

Example:

```
NAME UUID TYPE DEVICE

Wired 1 xxxx ethernet ens33
```

---

## Show active connections

```bash
nmcli connection show --active
```

---

## Show device status

```bash
nmcli device status
```

---

## Display IP address

```bash
nmcli device show
```

Specific interface:

```bash
nmcli device show ens33
```

---

## Connect interface

```bash
nmcli device connect ens33
```

---

## Disconnect interface

```bash
nmcli device disconnect ens33
```

---

## Create Static IP

```bash
nmcli con mod "Wired connection 1" \
ipv4.addresses 192.168.1.100/24
```

Gateway:

```bash
nmcli con mod "Wired connection 1" \
ipv4.gateway 192.168.1.1
```

DNS:

```bash
nmcli con mod "Wired connection 1" \
ipv4.dns "8.8.8.8 8.8.4.4"
```

Manual mode:

```bash
nmcli con mod "Wired connection 1" \
ipv4.method manual
```

Restart:

```bash
nmcli con down "Wired connection 1"

nmcli con up "Wired connection 1"
```

---

## DHCP Configuration

Automatic IP:

```bash
nmcli con mod "Wired connection 1" \
ipv4.method auto
```

Restart:

```bash
nmcli con up "Wired connection 1"
```

---

## Change Hostname

```bash
nmcli general hostname
```

Set:

```bash
sudo nmcli general hostname linux-server
```

---

## Scan Wi-Fi

```bash
nmcli device wifi list
```

Connect:

```bash
nmcli device wifi connect "OfficeWiFi" password "mypassword"
```

---

# Production Use Cases of nmcli

## Server Static IP

```bash
nmcli con mod ens33 ipv4.addresses 10.10.10.20/24
```

---

## DNS Change

```bash
nmcli con mod ens33 ipv4.dns "8.8.8.8"
```

---

## Automation Script

```bash
nmcli networking off

nmcli networking on
```

---

# 3. nmtui

# Meaning

```
Network Manager Text User Interface
```

A text-based menu system.

Works inside terminal.

No graphical desktop required.

Excellent for SSH sessions.

---

# Start

```bash
nmtui
```

Screen:

```
+---------------------+

Activate Connection

Edit Connection

Set System Hostname

Quit

+---------------------+
```

---

# Functions

## Activate Connection

Enable or disable interfaces.

Example:

```
ens33

Activate
```

---

## Edit Connection

Change:

IP Address

Subnet

Gateway

DNS

Hostname

DHCP

Static IP

---

## Set Hostname

Change machine hostname.

Example:

```
linux-server
```

---

# Production Use Cases

Data center servers

Minimal installations

Rescue mode

Remote SSH administration

Quick network fixes

---

# 4. nm-connection-editor

# Meaning

```
Network Manager Connection Editor
```

Graphical application for advanced network configuration.

Launch:

```bash
nm-connection-editor
```

or

```
Applications → Network Connections
```

---

# Features

Configure:

Ethernet

Wi-Fi

VPN

Bridge

Bond

VLAN

Team

Static IP

DHCP

DNS

Routes

MAC Address

IPv6

MTU

Proxy

---

# Example

Edit Ethernet

↓

IPv4 Settings

↓

Manual

↓

Add

IP: 192.168.1.100

Gateway: 192.168.1.1

DNS: 8.8.8.8

↓

Save

---

# Production Use Cases

Desktop Linux

Advanced VPN setup

Bridge configuration

VLAN setup

Virtualization host networking

---

# 5. GNOME Settings

Meaning:

GNOME Desktop's built-in network management utility.

Open:

```
Settings

↓

Network
```

or

```
Settings

↓

Wi-Fi
```

---

# Features

Ethernet

Wi-Fi

VPN

Proxy

Hotspot

Airplane Mode

IP information

MAC Address

DNS

Gateway

---

# Configure Static IP

Settings

↓

Network

↓

Gear Icon

↓

IPv4

↓

Manual

↓

Enter:

IP Address

Gateway

DNS

↓

Apply

---

# Production Use Cases

Desktop users

Laptop users

Quick troubleshooting

Office systems

Basic network management

---

# Comparison Table

| Tool | Interface | GUI | Server | Desktop |
|------|----------|------|---------|----------|
| nmcli | CLI | No | Excellent | Good |
| nmtui | Text Menu | No | Excellent | Good |
| nm-connection-editor | GUI | Yes | Fair | Excellent |
| GNOME Settings | GUI | Yes | Not Common | Excellent |

---

# Which Tool Should You Use?

| Scenario | Recommended Tool |
|-----------|----------------|
| SSH Remote Server | nmcli |
| Minimal Linux Installation | nmtui |
| Desktop Advanced Configuration | nm-connection-editor |
| Basic Desktop Networking | GNOME Settings |
| Automation Scripts | nmcli |
| Static IP on Server | nmcli |
| Quick Terminal Configuration | nmtui |
| Wi-Fi on Laptop | GNOME Settings |

---

# Important Files Used by NetworkManager

Check active connections:

```bash
nmcli connection show
```

Connection profiles are commonly stored in:

```
/etc/NetworkManager/system-connections/
```

Service:

```
NetworkManager.service
```

Configuration file:

```
/etc/NetworkManager/NetworkManager.conf
```

Restart NetworkManager:

```bash
sudo systemctl restart NetworkManager
```

Reload configuration:

```bash
nmcli connection reload
```

---

# Interview Questions

## What is NetworkManager?

A Linux service that manages network devices and connections automatically.

---

## Difference between nmcli and nmtui?

| nmcli | nmtui |
|--------|--------|
| Command line | Text menu |
| Scriptable | Interactive |
| Automation | Manual configuration |
| Production servers | Beginner friendly |

---

## What is nm-connection-editor?

A graphical utility for advanced NetworkManager connection configuration.

---

## What is GNOME Settings?

A desktop GUI for basic network management.

---

## Which utility is preferred in production Linux servers?

**nmcli**, because it supports scripting, automation, remote administration, and full NetworkManager functionality.

---

# Quick Revision

✅ **nmcli** = Network Manager Command Line Interface (best for servers & automation)

✅ **nmtui** = Network Manager Text User Interface (best for terminal-based management)

✅ **nm-connection-editor** = Advanced graphical network configuration tool

✅ **GNOME Settings** = Simple desktop GUI for everyday networking tasks

✅ **NetworkManager** = Background service that manages all these utilities and Linux network connections.