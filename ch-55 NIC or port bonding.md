# Linux NIC or Port Bonding (Network Bonding)

# What is NIC Bonding?

NIC Bonding (also called Network Bonding, Port Bonding, Teaming, or Link Aggregation) is the process of combining two or more physical Network Interface Cards (NICs) into a single logical interface.

The main purposes are:
- High Availability (Fault Tolerance)
- Load Balancing
- Increased Bandwidth
- Network Redundancy

---

# Why NIC Bonding is Used?

Imagine a server with two NICs:

```
         +-------------+
         |   Server    |
         +-------------+
          |         |
       eth0       eth1
          |         |
          +---------+
               |
            Switch
```

If `eth0` fails:
- Without bonding → Network connection lost.
- With bonding → `eth1` automatically takes over.

This minimizes downtime in production environments.

---

# Real Production Example

Servers like:
- Database Servers
- VMware ESXi Hosts
- Kubernetes Nodes
- Web Servers
- Storage Servers

often use multiple NICs.

Example:

```
eth0 -> 1 Gbps
eth1 -> 1 Gbps

Bond Interface:
bond0 -> 2 Gbps (depending on mode)
```

---

# Benefits of NIC Bonding

| Benefit | Description |
|----------|------------|
| High Availability | NIC failure doesn't disconnect server |
| Load Balancing | Traffic distributed across NICs |
| Increased Bandwidth | Multiple links combined |
| Redundancy | Backup network path |
| Fault Tolerance | Automatic failover |

---

# Linux Bonding Architecture

```
                bond0
                  |
        -------------------
        |                 |
      eth0             eth1
        |                 |
      Switch           Switch
```

Applications only communicate with `bond0`.

Linux handles physical NIC selection.

---

# Bonding Modes

Linux supports several bonding modes.

```
0
1
2
3
4
5
6
```

---

# Mode 0: Balance Round Robin

```
mode=0
```

Traffic is sent alternately:

```
Packet1 -> eth0
Packet2 -> eth1
Packet3 -> eth0
Packet4 -> eth1
```

Advantages:
- Load balancing
- Increased bandwidth

Disadvantages:
- Switch support required
- Packet ordering issues possible

Use Case:
High throughput environments.

---

# Mode 1: Active Backup

```
mode=1
```

One NIC active.

Other NIC standby.

```
bond0

eth0 ACTIVE

eth1 BACKUP
```

If eth0 fails:

```
eth1 becomes ACTIVE
```

Advantages:
- Simple
- Excellent redundancy

Switch support:
Not required.

Production:
Most commonly used for server redundancy.

---

# Mode 2: Balance XOR

```
mode=2
```

Traffic selection based on:

```
Source MAC

Destination MAC

Hash Algorithm
```

Requires switch configuration.

---

# Mode 3: Broadcast

```
mode=3
```

Every packet sent through every NIC.

```
eth0 -> Packet

eth1 -> Packet
```

Maximum redundancy.

Bandwidth does not increase.

---

# Mode 4: IEEE 802.3ad (LACP)

Most popular enterprise mode.

```
mode=4
```

Uses:

```
LACP

Link Aggregation Control Protocol
```

Requires managed switch.

Example:

```
2 x 1Gb NIC

bond0

2Gb aggregated
```

Advantages:
- Redundancy
- Load balancing
- Industry standard

Production:
Very common.

Cisco equivalent:

```
EtherChannel
```

---

# Mode 5: Balance TLB

Adaptive Transmit Load Balancing.

```
mode=5
```

Balances outgoing traffic.

Incoming traffic uses one NIC.

Switch support:
Not required.

---

# Mode 6: Balance ALB

Adaptive Load Balancing.

```
mode=6
```

Balances:

Incoming

Outgoing

No switch support.

---

# Summary of Bond Modes

| Mode | Name | Switch Required |
|------|-------|----------------|
| 0 | Round Robin | Yes |
| 1 | Active Backup | No |
| 2 | XOR | Yes |
| 3 | Broadcast | No |
| 4 | LACP | Yes |
| 5 | TLB | No |
| 6 | ALB | No |

---

# Check Bonding Module

```
lsmod | grep bonding
```

Load module:

```
modprobe bonding
```

---

# Bond Configuration File

Older RHEL:

```
/etc/sysconfig/network-scripts/
```

Ubuntu:

```
/etc/netplan/
```

Kernel information:

```
/proc/net/bonding/
```

---

# Check Bond Status

```
cat /proc/net/bonding/bond0
```

Example:

```
Bonding Mode: active-backup

Primary Slave: eth0

Currently Active Slave: eth0

Slave Interface: eth0

MII Status: up

Slave Interface: eth1

MII Status: up
```

---

# Check Bond Interface

```
ip addr
```

Example:

```
bond0
```

---

# Interface Status

```
ip link
```

Example:

```
bond0: UP
eth0: UP
eth1: UP
```

---

# Check Bond Driver

```
modinfo bonding
```

---

# Check NIC Information

```
ethtool eth0

ethtool eth1
```

---

# Testing Failover

Current active:

```
cat /proc/net/bonding/bond0
```

Disconnect cable:

```
eth0 DOWN
```

Check:

```
cat /proc/net/bonding/bond0
```

Now:

```
Active Slave:

eth1
```

Applications continue working.

---

# Ubuntu Netplan Bond Example

```
network:
  version: 2
  bonds:
    bond0:
      interfaces:
        - eth0
        - eth1
      parameters:
        mode: active-backup
      dhcp4: yes
```

Apply:

```
netplan apply
```

---

# RHEL Example

```
DEVICE=bond0
TYPE=Bond
BONDING_OPTS="mode=1 miimon=100"
BOOTPROTO=dhcp
ONBOOT=yes
```

Slave:

```
DEVICE=eth0
MASTER=bond0
SLAVE=yes
```

---

# Important Bonding Parameters

## mode

Bonding mode.

Example:

```
mode=1
```

---

## miimon

Checks link health.

```
miimon=100
```

Every 100 ms.

---

## primary

Preferred active NIC.

```
primary=eth0
```

---

## downdelay

Delay before marking down.

---

## updelay

Delay before marking up.

---

# Production Monitoring

Check bond:

```
cat /proc/net/bonding/bond0
```

Check interfaces:

```
ip link
```

Check routes:

```
ip route
```

Check NIC:

```
ethtool eth0
```

Check traffic:

```
ip -s link
```

---

# Bonding vs Teaming

| Bonding | Teaming |
|----------|----------|
| Older | Newer |
| Kernel-based | User-space daemon |
| Stable | Flexible |
| Widely used | Modern alternative |

---

# Bonding vs Bridging

Bonding:

```
Multiple NICs -> One Interface
```

Bridging:

```
Multiple Networks -> One Bridge
```

Example:

```
Bond:

eth0 + eth1 = bond0

Bridge:

eth0 + VM = br0
```

---

# Common Production Commands

```
ip addr

ip link

cat /proc/net/bonding/bond0

lsmod | grep bonding

modprobe bonding

ethtool eth0

ethtool eth1

ip -s link

ip route
```

---

# Production Best Practices

✅ Use Mode 1 for simple redundancy.

✅ Use Mode 4 (LACP) with managed switches.

✅ Enable `miimon=100`.

✅ Test failover regularly.

✅ Monitor `/proc/net/bonding/bond0`.

✅ Ensure switch configuration matches Linux mode.

---

# Interview Questions

## Q1. What is NIC Bonding?

Combining multiple physical NICs into one logical interface for redundancy and/or increased bandwidth.

---

## Q2. What is the most common Linux bonding mode?

**Mode 1 (Active Backup)** for redundancy and **Mode 4 (LACP)** for enterprise environments.

---

## Q3. Which bonding mode does not require switch support?

Modes:
```
1
3
5
6
```

---

## Q4. Which bonding mode uses IEEE 802.3ad?

```
Mode 4
```

LACP (Link Aggregation Control Protocol).

---

## Q5. How do you check bond status?

```
cat /proc/net/bonding/bond0
```

---

## Q6. What is `miimon=100`?

Linux checks NIC link status every 100 milliseconds to detect failures quickly.

---

## Q7. What happens in Active-Backup mode if the active NIC fails?

The backup NIC automatically becomes active, providing uninterrupted network connectivity.

---

# Linux Admin Quick Cheat Sheet

```
lsmod | grep bonding
modprobe bonding
ip addr
ip link
cat /proc/net/bonding/bond0
ethtool eth0
ethtool eth1
ip -s link
ip route
```

## Exam Tip

For Linux administration interviews and RHCSA/RHCE-style practical work:

- **Mode 1 (Active-Backup)** → Best for high availability without special switch configuration.
- **Mode 4 (802.3ad/LACP)** → Best for enterprise environments requiring both redundancy and load balancing with managed switches.