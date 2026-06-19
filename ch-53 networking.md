# Linux Networking Fundamentals

## 154. Enable Internet on Linux VM

### Objective

A Linux Virtual Machine (VM) requires a network connection to access the internet for software installation, updates, downloading packages, and communicating with external systems.

---

## Network Modes in Virtual Machines

### 1. NAT (Network Address Translation)

* Default networking mode in most virtualization software.
* VM shares the host machine's internet connection.
* VM can access the internet.
* External devices cannot directly access the VM.

#### Example

```
Internet
    |
Host Machine
    |
NAT
    |
Linux VM
```

### 2. Bridged Adapter

* VM acts like a separate device on the network.
* Receives its own IP address from the router.
* Both internet and LAN devices can access the VM.

#### Example

```
Router
 ├── Host PC
 └── Linux VM
```

### 3. Host-Only Adapter

* Communication only between Host and VM.
* No internet access.

### 4. Internal Network

* Communication between multiple VMs only.
* No internet access.

---

## Checking Internet Connectivity

### Ping Google DNS

```bash
ping 8.8.8.8
```

Successful output:

```bash
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=20 ms
```

### Ping Google Domain

```bash
ping google.com
```

If IP ping works but domain ping fails:

* DNS issue exists.

---

## Check IP Address

### Modern Command

```bash
ip addr show
```

### Legacy Command

```bash
ifconfig
```

---

## Restart Network Service

Ubuntu/Debian:

```bash
sudo systemctl restart NetworkManager
```

or

```bash
sudo systemctl restart networking
```

---

## Check Default Gateway

```bash
ip route
```

Example:

```bash
default via 192.168.1.1 dev ens33
```

---

## Check DNS Configuration

```bash
cat /etc/resolv.conf
```

Example:

```bash
nameserver 8.8.8.8
nameserver 8.8.4.4
```

---

## Troubleshooting Internet Issues

### Step 1: Verify Interface Status

```bash
ip link show
```

### Step 2: Verify IP Assignment

```bash
ip addr show
```

### Step 3: Verify Gateway

```bash
ip route
```

### Step 4: Test Connectivity

```bash
ping 8.8.8.8
```

### Step 5: Test DNS

```bash
ping google.com
```

---

## Real-Life Example

Install updates from internet:

```bash
sudo apt update
sudo apt upgrade
```

Without internet connectivity these commands will fail.

---

# 155. Network Components

## What is a Network?

A network is a collection of devices connected together to exchange information and resources.

---

## Major Network Components

### 1. Network Interface Card (NIC)

A hardware component that connects a device to a network.

Examples:

* Ethernet Card
* Wi-Fi Adapter

Linux Command:

```bash
ip link show
```

---

### 2. IP Address

A unique identifier assigned to each device.

Example:

```text
192.168.1.100
```

Check IP:

```bash
ip addr
```

---

### 3. MAC Address

Physical address of a network card.

Example:

```text
00:1A:2B:3C:4D:5E
```

Check MAC:

```bash
ip link show
```

---

### 4. Switch

* Connects multiple devices within a LAN.
* Operates at Layer 2 (Data Link Layer).
* Uses MAC addresses.

Example:

```text
PC1
 |
Switch
 |---- PC2
 |---- PC3
```

---

### 5. Router

* Connects different networks.
* Routes packets using IP addresses.
* Provides internet access.

Example:

```text
Internet
    |
 Router
    |
 Switch
  / | \
PC PC PC
```

---

### 6. Hub

* Older networking device.
* Broadcasts data to every port.
* Less secure and less efficient.

---

### 7. Modem

Converts signals from ISP into usable network signals.

Examples:

* Cable Modem
* DSL Modem
* Fiber ONT

---

### 8. Firewall

Controls incoming and outgoing traffic.

Linux Firewall:

```bash
ufw
```

Check Status:

```bash
sudo ufw status
```

---

### 9. DNS Server

Domain Name System converts domain names into IP addresses.

Example:

```text
google.com → 142.250.183.206
```

Test:

```bash
nslookup google.com
```

---

### 10. Gateway

Acts as an exit point from one network to another.

View Gateway:

```bash
ip route
```

Example:

```bash
default via 192.168.1.1
```

---

## OSI Layer Mapping

| Component | OSI Layer         |
| --------- | ----------------- |
| Hub       | Layer 1           |
| Switch    | Layer 2           |
| Router    | Layer 3           |
| Firewall  | Layer 3-7         |
| DNS       | Application Layer |

---

# 156. Network Files and Commands

## Important Network Configuration Files

---

### 1. /etc/hosts

Maps hostnames to IP addresses.

Example:

```bash
127.0.0.1 localhost
192.168.1.10 server1
```

View:

```bash
cat /etc/hosts
```

---

### 2. /etc/resolv.conf

Stores DNS server information.

Example:

```bash
nameserver 8.8.8.8
nameserver 8.8.4.4
```

View:

```bash
cat /etc/resolv.conf
```

---

### 3. /etc/hostname

Stores system hostname.

View:

```bash
cat /etc/hostname
```

Change:

```bash
sudo hostnamectl set-hostname myserver
```

---

### 4. /etc/netplan/*.yaml (Ubuntu)

Network configuration file.

Example:

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
```

Apply:

```bash
sudo netplan apply
```

---

# Important Networking Commands

---

## 1. ping

### Purpose

Tests connectivity between systems.

### Syntax

```bash
ping <IP_or_Hostname>
```

### Example

```bash
ping google.com
```

### Sample Output

```bash
64 bytes from google.com: icmp_seq=1 ttl=117 time=22 ms
```

### Common Uses

* Verify network connectivity
* Verify DNS resolution
* Troubleshoot connection issues

---

## 2. ifup

### Purpose

Enable a network interface.

### Syntax

```bash
sudo ifup eth0
```

### Example

```bash
sudo ifup ens33
```

### Usage

* Bring interface online
* Activate network configuration

---

## 3. ifdown

### Purpose

Disable a network interface.

### Syntax

```bash
sudo ifdown eth0
```

### Example

```bash
sudo ifdown ens33
```

### Usage

* Temporarily disconnect network
* Troubleshooting

---

## 4. netstat

### Purpose

Displays network connections, routing tables, and listening ports.

### Syntax

```bash
netstat [options]
```

### Show Listening Ports

```bash
netstat -tuln
```

Where:

| Option | Meaning        |
| ------ | -------------- |
| -t     | TCP            |
| -u     | UDP            |
| -l     | Listening      |
| -n     | Numeric Output |

### Example Output

```bash
tcp 0 0 0.0.0.0:22 0.0.0.0:* LISTEN
```

Port 22 means SSH service is running.

---

### Show Routing Table

```bash
netstat -rn
```

---

### Modern Alternative

```bash
ss -tuln
```

---

## 5. tcpdump

### Purpose

Captures and analyzes network packets.

### Syntax

```bash
sudo tcpdump [options]
```

---

### Capture All Traffic

```bash
sudo tcpdump
```

---

### Capture on Specific Interface

```bash
sudo tcpdump -i ens33
```

---

### Capture ICMP Packets

```bash
sudo tcpdump icmp
```

Then from another terminal:

```bash
ping google.com
```

---

### Save Packets to File

```bash
sudo tcpdump -w capture.pcap
```

Open later using Wireshark.

---

### Read Saved File

```bash
sudo tcpdump -r capture.pcap
```

---

### Practical Uses

* Network troubleshooting
* Security analysis
* Detect suspicious traffic
* Monitor application communication

---

## Quick Revision Table

| Command     | Purpose                        |
| ----------- | ------------------------------ |
| ping        | Test connectivity              |
| ifup        | Enable network interface       |
| ifdown      | Disable network interface      |
| netstat     | View connections and ports     |
| ss          | Modern replacement for netstat |
| tcpdump     | Capture network packets        |
| ip addr     | Show IP address                |
| ip route    | Show routing table             |
| nslookup    | DNS lookup                     |
| hostnamectl | Manage hostname                |

---

# Interview Questions

### Q1: Difference between Ping and TCPDump?

**Ping**

* Tests connectivity.
* Uses ICMP packets.

**TCPDump**

* Captures and analyzes actual packets.
* Used for advanced troubleshooting.

---

### Q2: What is the purpose of DNS?

DNS translates domain names into IP addresses.

Example:

```text
google.com → 142.250.x.x
```

---

### Q3: Which command shows listening ports?

```bash
netstat -tuln
```

or

```bash
ss -tuln
```

---

### Q4: Which file stores DNS server information?

```bash
/etc/resolv.conf
```

---

### Q5: How do you capture packets on interface ens33?

```bash
sudo tcpdump -i ens33
```
