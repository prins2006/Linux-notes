# Hostname or IP Lookup (nslookup and dig)

## What are nslookup and dig?

`nslookup` and `dig` are DNS troubleshooting tools used to query DNS servers and obtain information about domain names and IP addresses.

They help administrators:

* Verify DNS records
* Troubleshoot name resolution issues
* Perform forward lookups
* Perform reverse lookups
* Check DNS server responses

---

# 1. nslookup Command

## What is nslookup?

`nslookup` (Name Server Lookup) is a command-line tool used to query DNS servers and resolve hostnames to IP addresses.

### Syntax

```bash
nslookup hostname
```

or

```bash
nslookup IP-address
```

---

## Forward Lookup

Find the IP address of a domain:

```bash
nslookup google.com
```

Example Output:

```text
Server:     8.8.8.8
Address:    8.8.8.8#53

Non-authoritative answer:
Name:       google.com
Address:    142.250.183.78
```

### Explanation

| Field   | Meaning             |
| ------- | ------------------- |
| Server  | DNS server used     |
| Address | DNS server IP       |
| Name    | Domain queried      |
| Address | Returned IP address |

---

## Reverse Lookup

Find the hostname of an IP address:

```bash
nslookup 8.8.8.8
```

Example Output:

```text
8.8.8.8.in-addr.arpa name = dns.google
```

---

## Query a Specific DNS Server

```bash
nslookup google.com 8.8.8.8
```

Uses Google's DNS server directly.

---

## Interactive Mode

Start interactive mode:

```bash
nslookup
```

Example:

```text
> google.com
> facebook.com
> exit
```

---

## Check MX Records

```bash
nslookup -type=mx gmail.com
```

Shows mail servers responsible for receiving email.

---

## Check NS Records

```bash
nslookup -type=ns google.com
```

Displays authoritative name servers.

---

## Check TXT Records

```bash
nslookup -type=txt google.com
```

Used for SPF, DKIM, and domain verification records.

---

# 2. dig Command

## What is dig?

`dig` (Domain Information Groper) is an advanced DNS lookup tool that provides detailed DNS query information.

### Syntax

```bash
dig domain-name
```

---

## Basic Lookup

```bash
dig google.com
```

Example Output:

```text
;; QUESTION SECTION:
;google.com.    IN  A

;; ANSWER SECTION:
google.com. 300 IN A 142.250.183.78
```

---

## Important Sections

| Section    | Purpose           |
| ---------- | ----------------- |
| HEADER     | Query status      |
| QUESTION   | DNS query         |
| ANSWER     | Returned records  |
| AUTHORITY  | Authoritative DNS |
| ADDITIONAL | Extra information |

---

## Short Output

```bash
dig +short google.com
```

Output:

```text
142.250.183.78
```

Useful in scripts.

---

## Reverse Lookup

```bash
dig -x 8.8.8.8
```

Output:

```text
dns.google.
```

---

## Query Specific DNS Server

```bash
dig @8.8.8.8 google.com
```

Queries Google's DNS server.

---

## Check MX Records

```bash
dig gmail.com MX
```

---

## Check NS Records

```bash
dig google.com NS
```

---

## Check TXT Records

```bash
dig google.com TXT
```

---

## Check AAAA Record (IPv6)

```bash
dig google.com AAAA
```

---

## Trace DNS Resolution

```bash
dig +trace google.com
```

Shows the complete DNS lookup path from root servers to the authoritative server.

---

# Install DNS Utilities

## RHEL / CentOS / Rocky Linux

```bash
sudo dnf install bind-utils -y
```

---

## Ubuntu / Debian

```bash
sudo apt install dnsutils -y
```

---

# Common DNS Record Types

| Record | Purpose        |
| ------ | -------------- |
| A      | IPv4 Address   |
| AAAA   | IPv6 Address   |
| MX     | Mail Server    |
| NS     | Name Server    |
| PTR    | Reverse Lookup |
| TXT    | Text Record    |
| CNAME  | Alias          |

---

# Production Troubleshooting Examples

## Verify DNS Resolution

```bash
dig google.com
```

---

## Check if DNS Server is Reachable

```bash
nslookup google.com 8.8.8.8
```

---

## Verify Reverse DNS

```bash
dig -x 8.8.8.8
```

---

## Test Internal DNS Server

```bash
dig @192.168.1.10 company.local
```

---

## Check DNS Port

```bash
ss -tulpn | grep :53
```

---

## Verify Resolver Configuration

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 8.8.8.8
nameserver 1.1.1.1
```

---

# nslookup vs dig

| Feature               | nslookup | dig       |
| --------------------- | -------- | --------- |
| Easy to Use           | Yes      | Moderate  |
| Detailed Output       | Limited  | Extensive |
| Reverse Lookup        | Yes      | Yes       |
| Query Specific Server | Yes      | Yes       |
| Script Friendly       | Limited  | Excellent |
| DNS Troubleshooting   | Basic    | Advanced  |

---

# Important Commands Summary

```bash
nslookup google.com
nslookup 8.8.8.8
nslookup -type=mx gmail.com
nslookup -type=ns google.com

dig google.com
dig +short google.com
dig -x 8.8.8.8
dig google.com MX
dig google.com NS
dig +trace google.com

cat /etc/resolv.conf
ss -tulpn | grep :53
```

## Interview Question

### Q: What is the difference between nslookup and dig?

**Answer:**
`nslookup` is a simple DNS query tool for basic hostname and IP lookups, while `dig` provides detailed DNS information, supports advanced troubleshooting, and is preferred by Linux administrators for DNS diagnostics.

## One-Line Definition

**nslookup and dig are DNS query tools used to resolve hostnames, IP addresses, and troubleshoot DNS-related issues in Linux environments.**
