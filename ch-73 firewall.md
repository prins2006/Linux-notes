# Linux Firewall Complete Guide (Markdown)

> **Version:** 1.0
> **Topic:** Linux Firewall (UFW, Firewalld, iptables, nftables)
> **Level:** Beginner → Advanced

---

# Table of Contents

1. Introduction
2. What is a Firewall?
3. Why Firewall is Required
4. How Firewall Works
5. Packet Flow
6. Stateful vs Stateless Firewall
7. Netfilter Architecture
8. Firewall Types
9. UFW
10. Firewalld
11. iptables
12. nftables
13. Tables
14. Chains
15. Targets
16. Installation
17. Configuration
18. Commands
19. Port Permission
20. IP Permission
21. Rich Rules
22. NAT
23. Port Forwarding
24. Logging
25. Troubleshooting
26. Interview Questions

---

# Chapter 1 - What is Firewall?

A firewall is a **network security system** that monitors incoming and outgoing network traffic and decides whether to allow or block packets based on predefined security rules.

Think of a firewall like a **security guard** at the entrance of a building.

Everyone who enters must pass through the guard.

The guard checks:

* Who is coming?
* Where are they coming from?
* Which room do they want?
* Are they allowed?

If the answer is yes, the guard allows entry.

Otherwise, the guard blocks them.

Linux firewall works exactly the same way.

---

# Why Firewall?

Without Firewall

Internet
↓

Server

Everyone can connect.

With Firewall

Internet
↓

Firewall
↓

Server

Firewall checks every packet.

---

# Benefits

* Blocks hackers
* Blocks malware
* Allows only required ports
* Controls incoming traffic
* Controls outgoing traffic
* Protects servers
* Reduces attack surface

---

# Real Example

Your server has

SSH → Port 22

HTTP → Port 80

HTTPS → Port 443

Database → Port 3306

You want

Everyone

can access

80

443

Only Admin

can access

22

Nobody

can access

3306

Firewall performs these rules.

---

# Linux Firewall Components

Linux Kernel

↓

Netfilter

↓

iptables / nftables

↓

UFW / Firewalld

Applications like UFW and Firewalld are only management tools.

Actual filtering happens inside the Linux Kernel using **Netfilter**.

---

# Netfilter

Netfilter is the firewall framework inside Linux Kernel.

It examines every packet entering or leaving the computer.

Packet

↓

Netfilter

↓

Rule Match

↓

Allow or Block

---

# Packet Flow

Incoming Packet

↓

Network Card

↓

Netfilter

↓

INPUT Chain

↓

Application

Outgoing Packet

↓

OUTPUT Chain

↓

Netfilter

↓

Network Card

Router Packet

↓

FORWARD Chain

---

# Stateful Firewall

Stateful firewall remembers connection state.

Client

↓

SSH Request

↓

Firewall remembers

↓

Reply packet automatically allowed.

Example

You connect

```
ssh server
```

Reply packets are automatically accepted.

---

# Stateless Firewall

Checks every packet independently.

No memory.

Less secure.

---

# UFW

UFW

means

Uncomplicated Firewall

Ubuntu default firewall.

Easy to use.

Internally uses

iptables/nftables.

---

# Install UFW

Ubuntu

```bash
sudo apt update
sudo apt install ufw
```

---

# Start UFW

```bash
sudo systemctl start ufw
```

Meaning

Starts firewall service.

---

# Enable UFW

```bash
sudo ufw enable
```

Meaning

Firewall becomes active.

Rules begin filtering traffic.

---

# Disable

```bash
sudo ufw disable
```

Meaning

Stops firewall filtering.

---

# Reload

```bash
sudo ufw reload
```

Meaning

Reloads configuration.

No reboot required.

---

# Status

```bash
sudo ufw status
```

Shows

Active

Inactive

Rules

---

Verbose Status

```bash
sudo ufw status verbose
```

Shows

Policies

Logging

Rules

Default Action

---

Reset

```bash
sudo ufw reset
```

Deletes all rules.

Returns default configuration.

---

Allow SSH

```bash
sudo ufw allow 22
```

Meaning

Allow TCP port 22.

---

Allow HTTP

```bash
sudo ufw allow 80
```

Allow web server.

---

Allow HTTPS

```bash
sudo ufw allow 443
```

Allow secure websites.

---

Allow FTP

```bash
sudo ufw allow 21
```

---

Allow SMTP

```bash
sudo ufw allow 25
```

---

Allow DNS

```bash
sudo ufw allow 53
```

---

Allow by Service Name

```bash
sudo ufw allow ssh
```

Instead of

22

Uses service database.

---

Delete Rule

```bash
sudo ufw delete allow 22
```

Removes SSH rule.

---

Block Port

```bash
sudo ufw deny 22
```

Blocks SSH.

---

Reject Port

```bash
sudo ufw reject 22
```

Difference

deny

Drops silently.

reject

Replies

Connection refused.

---

Allow Specific IP

```bash
sudo ufw allow from 192.168.1.100
```

Only this IP allowed.

---

Allow Network

```bash
sudo ufw allow from 192.168.1.0/24
```

Allows subnet.

---

Allow Specific IP to SSH

```bash
sudo ufw allow from 192.168.1.10 to any port 22
```

Very common.

---

Block IP

```bash
sudo ufw deny from 10.10.10.20
```

Blocks attacker.

---

Allow Port Range

```bash
sudo ufw allow 1000:2000/tcp
```

---

Delete by Number

```bash
sudo ufw status numbered
```

Then

```bash
sudo ufw delete 3
```

Deletes rule number 3.

---

# Firewalld

Default firewall

RHEL

Rocky

CentOS

Fedora

Uses

Zones

Services

Rich Rules

---

Install

```bash
sudo dnf install firewalld
```

Ubuntu

```bash
sudo apt install firewalld
```

---

Enable

```bash
sudo systemctl enable firewalld
```

Starts at boot.

---

Start

```bash
sudo systemctl start firewalld
```

---

Status

```bash
sudo systemctl status firewalld
```

---

Reload

```bash
sudo firewall-cmd --reload
```

Apply configuration.

---

State

```bash
firewall-cmd --state
```

Returns

running

not running

---

List Zones

```bash
firewall-cmd --get-zones
```

---

Default Zone

```bash
firewall-cmd --get-default-zone
```

---

Active Zones

```bash
firewall-cmd --get-active-zones
```

---

Allow HTTP

```bash
firewall-cmd --add-service=http
```

Temporary.

Permanent

```bash
firewall-cmd --permanent --add-service=http
```

Reload

```bash
firewall-cmd --reload
```

---

Allow Port

```bash
firewall-cmd --add-port=8080/tcp
```

Permanent

```bash
firewall-cmd --permanent --add-port=8080/tcp
```

---

Remove Port

```bash
firewall-cmd --remove-port=8080/tcp
```

---

List Services

```bash
firewall-cmd --list-services
```

---

List Ports

```bash
firewall-cmd --list-ports
```

---

Allow Source IP

```bash
firewall-cmd --add-source=192.168.1.50
```

---

Rich Rule

```bash
firewall-cmd \
--permanent \
--add-rich-rule='rule family="ipv4" source address="192.168.1.50" service name="ssh" accept'
```

Only that IP can use SSH.

---

# iptables

iptables directly manages Linux firewall.

Kernel

↓

Netfilter

↓

iptables

---

List Rules

```bash
sudo iptables -L
```

Meaning

Display all rules.

---

Verbose

```bash
sudo iptables -L -v -n
```

Shows

Packets

Bytes

Numeric IP

---

Show Policies

```bash
sudo iptables -S
```

---

Flush Rules

```bash
sudo iptables -F
```

Deletes all rules.

---

Allow SSH

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Meaning

Append rule

INPUT chain

TCP

Destination Port

22

Action

Accept

---

Block SSH

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j DROP
```

---

Reject SSH

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j REJECT
```

---

Allow HTTP

```bash
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

---

Allow HTTPS

```bash
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

---

Allow Ping

```bash
sudo iptables -A INPUT -p icmp -j ACCEPT
```

---

Block IP

```bash
sudo iptables -A INPUT -s 10.10.10.10 -j DROP
```

---

Allow Network

```bash
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
```

---

Delete Rule

```bash
sudo iptables -D INPUT 1
```

Deletes first rule.

---

Save Rules

Ubuntu

```bash
sudo netfilter-persistent save
```

---

# nftables

Modern replacement of iptables.

Check

```bash
sudo nft list ruleset
```

---

Add Rule

```bash
sudo nft add rule inet filter input tcp dport 22 accept
```

---

Flush Rules

```bash
sudo nft flush ruleset
```

---

# Troubleshooting

Firewall Status

```bash
sudo ufw status
```

Firewalld

```bash
sudo firewall-cmd --state
```

iptables

```bash
sudo iptables -L
```

nftables

```bash
sudo nft list ruleset
```

---

# Common Errors

## Unit iptables.service not found

Reason

Ubuntu does not provide an `iptables.service`.

Solution

Use:

```bash
sudo systemctl status ufw
```

or manage rules with:

```bash
sudo iptables -L
```

---

## SSH Locked Out

Always allow SSH before enabling a firewall:

```bash
sudo ufw allow ssh
sudo ufw enable
```

or

```bash
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

---

# Interview Questions

**Q1:** What is a firewall?

**Answer:** A firewall filters incoming and outgoing network traffic based on security rules.

**Q2:** What is Netfilter?

**Answer:** Netfilter is the Linux kernel framework that performs packet filtering, NAT, and connection tracking.

**Q3:** Difference between UFW and iptables?

* UFW: Easy-to-use frontend.
* iptables: Low-level packet filtering tool.

**Q4:** Difference between DROP and REJECT?

* DROP: Silently discards packets.
* REJECT: Sends an error response to the sender.

**Q5:** Difference between Runtime and Permanent rules in Firewalld?

* Runtime: Lost after reboot.
* Permanent: Saved and survives reboot.

---

# Summary

* **Netfilter** is the firewall engine inside the Linux kernel.
* **iptables** and **nftables** manage Netfilter rules directly.
* **UFW** is the default firewall frontend on Ubuntu.
* **Firewalld** is the default firewall manager on RHEL-based systems.
* Always allow required services (such as SSH) before enabling a firewall.
* Use least-privilege rules: allow only the ports and IP addresses that are necessary.
