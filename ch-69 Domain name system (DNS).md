# DNS - Download, Install and Configure (Domain Name System)

## What is DNS?

DNS (Domain Name System) is the Internet's phonebook. It translates human-readable domain names into IP addresses.

Example:

```text
google.com  →  142.250.183.78
facebook.com → 157.240.22.35
```

Without DNS, users would need to remember IP addresses instead of domain names.

---

# DNS Components

## DNS Server

Stores and provides DNS records.

Examples:

* Google DNS: 8.8.8.8
* Cloudflare DNS: 1.1.1.1

## DNS Resolver

Receives DNS queries from clients and finds the correct IP address.

## DNS Zone

A portion of the DNS namespace managed by a DNS server.

## DNS Records

Common record types:

| Record | Purpose                       |
| ------ | ----------------------------- |
| A      | Maps hostname to IPv4 address |
| AAAA   | Maps hostname to IPv6 address |
| CNAME  | Alias record                  |
| MX     | Mail server record            |
| NS     | Name server record            |
| PTR    | Reverse lookup                |
| TXT    | Text information              |

---

# Install DNS Server (BIND)

BIND (Berkeley Internet Name Domain) is the most widely used DNS server on Linux.

## RHEL / CentOS / Rocky Linux

```bash
sudo dnf install bind bind-utils -y
```

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install bind9 bind9-utils -y
```

Verify Installation:

```bash
named -v
```

---

# Important DNS Packages

| Package    | Purpose              |
| ---------- | -------------------- |
| bind       | DNS server           |
| bind-utils | DNS tools            |
| bind9      | DNS service (Ubuntu) |
| dnsutils   | DNS utilities        |

---

# DNS Configuration Files

## Main Configuration File

```text
/etc/named.conf
```

Ubuntu:

```text
/etc/bind/named.conf
```

---

# Start DNS Service

Enable service:

```bash
sudo systemctl enable named
```

Start service:

```bash
sudo systemctl start named
```

Check status:

```bash
sudo systemctl status named
```

---

# Configure Forward Lookup Zone

Edit:

```bash
sudo vi /etc/named.conf
```

Add:

```text
zone "example.com" IN {
    type master;
    file "forward.zone";
};
```

---

# Create Forward Zone File

```bash
sudo vi /var/named/forward.zone
```

Example:

```text
$TTL 86400
@   IN  SOA dns.example.com. root.example.com. (
        2026062301
        3600
        1800
        604800
        86400 )

@       IN NS dns.example.com.
dns     IN A 192.168.1.10
www     IN A 192.168.1.20
mail    IN A 192.168.1.30
```

---

# Configure Reverse Lookup Zone

Add:

```text
zone "1.168.192.in-addr.arpa" IN {
    type master;
    file "reverse.zone";
};
```

Create file:

```bash
sudo vi /var/named/reverse.zone
```

Example:

```text
$TTL 86400
@   IN SOA dns.example.com. root.example.com. (
        2026062301
        3600
        1800
        604800
        86400 )

@ IN NS dns.example.com.
10 IN PTR dns.example.com.
20 IN PTR www.example.com.
```

---

# Verify DNS Configuration

Check syntax:

```bash
sudo named-checkconf
```

Check zone:

```bash
sudo named-checkzone example.com /var/named/forward.zone
```

Expected Output:

```text
OK
```

---

# Restart DNS Service

```bash
sudo systemctl restart named
```

---

# Configure Firewall

Allow DNS traffic:

```bash
sudo firewall-cmd --permanent --add-service=dns
sudo firewall-cmd --reload
```

Verify:

```bash
sudo firewall-cmd --list-services
```

---

# DNS Testing Commands

## nslookup

```bash
nslookup google.com
```

## dig

```bash
dig google.com
```

## host

```bash
host google.com
```

## Reverse Lookup

```bash
dig -x 8.8.8.8
```

---

# Configure Client DNS

Edit:

```bash
sudo vi /etc/resolv.conf
```

Example:

```text
nameserver 192.168.1.10
nameserver 8.8.8.8
```

Verify:

```bash
cat /etc/resolv.conf
```

---

# DNS Troubleshooting

Check service:

```bash
systemctl status named
```

Check listening port:

```bash
ss -tulpn | grep :53
```

Check logs:

```bash
journalctl -u named
```

Test DNS query:

```bash
dig @192.168.1.10 example.com
```

---

# Production Best Practices

1. Use primary and secondary DNS servers.
2. Enable firewall rules for port 53.
3. Keep BIND updated.
4. Backup zone files regularly.
5. Use access control lists (ACLs).
6. Monitor DNS logs.
7. Restrict zone transfers.

---

# Important Commands Summary

```bash
dnf install bind bind-utils

systemctl start named
systemctl enable named
systemctl status named

named-checkconf
named-checkzone

dig google.com
nslookup google.com
host google.com

ss -tulpn | grep :53

journalctl -u named
```

## One-Line Definition

**DNS (Domain Name System) is a service that translates domain names such as google.com into IP addresses so computers can communicate on a network.**
