# Linux NIC (Network Interface Card) Information - ethtool

## What is ethtool?

`ethtool` is a Linux command-line utility used to display and modify the settings of a Network Interface Card (NIC). It helps Linux administrators troubleshoot network problems and optimize network performance.

Common uses:
- Check NIC speed and duplex mode
- Verify link status
- View supported features
- Display driver information
- Check NIC statistics
- Enable/disable features like checksum offloading
- Identify physical NIC ports

---

# Installation

## Ubuntu/Debian

```bash
sudo apt update
sudo apt install ethtool
```

## RHEL/CentOS/Rocky Linux

```bash
sudo yum install ethtool
```

or

```bash
sudo dnf install ethtool
```

---

# Find Available Network Interfaces

```bash
ip link
```

or

```bash
ip -br addr
```

Example:

```
lo      UNKNOWN
ens33   UP
eth0    UP
```

Suppose the interface is `eth0`.

---

# Basic NIC Information

```bash
ethtool eth0
```

Example Output:

```
Settings for eth0:
    Supported ports: [ TP ]
    Supported link modes:
        10baseT/Half
        10baseT/Full
        100baseT/Full
        1000baseT/Full

    Speed: 1000Mb/s
    Duplex: Full
    Auto-negotiation: on
    Port: Twisted Pair
    Link detected: yes
```

---

# Understanding the Output

## Supported Ports

```
Supported ports: [ TP ]
```

TP = Twisted Pair (Ethernet cable)

Possible values:

| Type | Meaning |
|-------|----------|
| TP | Ethernet cable |
| Fibre | Fiber optic |
| AUI | Attachment Unit Interface |
| MII | Media Independent Interface |

---

## Supported Link Modes

```
10Mbps
100Mbps
1000Mbps
```

Shows supported speeds.

Example:

```
10 Mbps
100 Mbps
1 Gbps
```

High-end NICs:

```
10 Gbps
25 Gbps
40 Gbps
100 Gbps
```

---

## Speed

```
Speed: 1000Mb/s
```

Current negotiated speed.

Possible:

```
10Mb/s
100Mb/s
1000Mb/s
10000Mb/s
```

---

## Duplex

```
Duplex: Full
```

### Half Duplex

Can either send or receive.

Not both.

### Full Duplex

Can send and receive simultaneously.

Production networks use Full Duplex.

---

## Auto-negotiation

```
Auto-negotiation: on
```

NIC automatically negotiates:

- Speed
- Duplex

---

## Port

```
Port: Twisted Pair
```

Physical connection type.

---

## Link Detected

```
Link detected: yes
```

Means cable connected.

```
yes = Connected

no = Cable unplugged or switch problem
```

---

# Check Driver Information

```bash
ethtool -i eth0
```

Example:

```
driver: e1000e
version: 5.15
firmware-version: 1.2
bus-info: 0000:00:19.0
```

---

# Driver Field

```
driver: e1000e
```

NIC driver loaded into kernel.

Common drivers:

| Driver | Vendor |
|---------|---------|
| e1000 | Intel |
| e1000e | Intel |
| igb | Intel Gigabit |
| ixgbe | Intel 10G |
| tg3 | Broadcom |
| r8169 | Realtek |
| mlx5 | Mellanox |

---

# Firmware Version

```
firmware-version:
```

NIC firmware installed.

Useful during upgrades.

---

# PCI Bus Information

```
bus-info:
```

Shows hardware location.

Example:

```
0000:03:00.0
```

---

# Check NIC Statistics

```bash
ethtool -S eth0
```

Example:

```
rx_packets
tx_packets
rx_errors
tx_errors
rx_dropped
tx_dropped
```

---

# Important Statistics

## RX Packets

Packets received.

## TX Packets

Packets transmitted.

## RX Errors

Receive errors.

## TX Errors

Transmission errors.

## Dropped Packets

Packets discarded.

Large values may indicate network issues.

---

# Check Link Status Only

```bash
ethtool eth0 | grep Link
```

Output:

```
Link detected: yes
```

---

# Check Speed Only

```bash
ethtool eth0 | grep Speed
```

Output:

```
Speed: 1000Mb/s
```

---

# Check Duplex Only

```bash
ethtool eth0 | grep Duplex
```

Output:

```
Duplex: Full
```

---

# Blink NIC LED

Useful in data centers.

```bash
ethtool -p eth0
```

LED blinks for identification.

Specify duration:

```bash
ethtool -p eth0 20
```

Blink for 20 seconds.

---

# Show Offload Features

```bash
ethtool -k eth0
```

Example:

```
rx-checksumming
tx-checksumming
scatter-gather
tcp-segmentation-offload
generic-segmentation-offload
```

---

# Disable Checksum Offloading

Temporary:

```bash
ethtool -K eth0 rx off
```

Enable:

```bash
ethtool -K eth0 rx on
```

Used during troubleshooting.

---

# Change NIC Speed (Temporary)

Example:

```bash
ethtool -s eth0 speed 100 duplex full autoneg off
```

Set:

- 100 Mbps
- Full Duplex
- Auto negotiation off

Verify:

```bash
ethtool eth0
```

---

# Display EEPROM Information

```bash
ethtool -e eth0
```

Shows NIC EEPROM contents.

Used by hardware engineers.

---

# Display Permanent MAC Address

```bash
ethtool -P eth0
```

Example:

```
Permanent address:
00:11:22:33:44:55
```

---

# Production Troubleshooting

## Check Interface

```bash
ip link
```

---

## Check IP

```bash
ip addr
```

---

## Check NIC

```bash
ethtool eth0
```

---

## Check Driver

```bash
ethtool -i eth0
```

---

## Check Statistics

```bash
ethtool -S eth0
```

---

## Check Routes

```bash
ip route
```

---

## Check Connectivity

```bash
ping gateway_ip
```

---

## Check DNS

```bash
ping google.com
```

---

## Capture Traffic

```bash
tcpdump -i eth0
```

---

# Related Commands

## Show NIC Hardware

```bash
lspci | grep Ethernet
```

---

## Show Interface Status

```bash
ip link
```

---

## Show MAC Address

```bash
ip link show eth0
```

---

## Show IP Address

```bash
ip addr
```

---

## Network Statistics

```bash
ip -s link
```

---

## Active Connections

```bash
ss -tulnp
```

---

# Real Production Example

A user reports slow network.

Steps:

```bash
ip link
```

Check interface UP.

```bash
ethtool eth0
```

Verify:

- Link detected
- Speed
- Duplex

```bash
ethtool -S eth0
```

Check:

- RX errors
- TX errors
- Dropped packets

```bash
ip route
```

Check gateway.

```bash
ping gateway
```

Test LAN.

```bash
ping 8.8.8.8
```

Test Internet.

```bash
ping google.com
```

Test DNS.

```bash
tcpdump -i eth0
```

Capture packets if issue persists.

---

# ethtool Cheat Sheet

```bash
ethtool eth0              # NIC information
ethtool -i eth0           # Driver information
ethtool -S eth0           # Statistics
ethtool -k eth0           # Offload features
ethtool -P eth0           # Permanent MAC
ethtool -p eth0           # Blink NIC LED
ethtool -e eth0           # EEPROM data
ethtool eth0 | grep Speed
ethtool eth0 | grep Duplex
ethtool eth0 | grep Link
ethtool -s eth0 speed 100 duplex full autoneg off
```

# Interview Questions

### Q1. What is ethtool?
A Linux utility for querying and controlling NIC driver and hardware settings.

### Q2. How do you check NIC speed?
```bash
ethtool eth0
```

### Q3. How do you check if the network cable is connected?
```bash
ethtool eth0
```
Look for:
```
Link detected: yes
```

### Q4. How do you check the NIC driver?
```bash
ethtool -i eth0
```

### Q5. How do you view NIC statistics?
```bash
ethtool -S eth0
```

### Q6. How do you identify a physical NIC in a server rack?
```bash
ethtool -p eth0
```

### Q7. What is the difference between Half Duplex and Full Duplex?
- Half Duplex: Send or receive at one time.
- Full Duplex: Send and receive simultaneously.

### Q8. What command shows the permanent hardware MAC address?
```bash
ethtool -P eth0
```