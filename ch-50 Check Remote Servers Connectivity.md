# Linux: Check Remote Servers Connectivity

## Introduction

Checking remote server connectivity is a common Linux system administration task. It helps determine whether a remote host is reachable, network services are available, and applications can communicate properly.

Common use cases:

* Verify server availability.
* Troubleshoot network issues.
* Check firewall rules.
* Test application ports.
* Monitor infrastructure.

---

# 1. ping Command

## Purpose

Checks whether a remote server is reachable using ICMP packets.

## Syntax

```bash
ping hostname
```

or

```bash
ping IP_Address
```

## Example

```bash
ping google.com
```

Output:

```
64 bytes from ...
64 bytes from ...
```

Stop:

```
Ctrl+C
```

## Send Limited Packets

```bash
ping -c 4 google.com
```

Send only four packets.

## Important Options

| Option | Meaning           |
| ------ | ----------------- |
| -c     | Number of packets |
| -i     | Interval          |
| -W     | Timeout           |
| -s     | Packet size       |

Example:

```bash
ping -c 5 8.8.8.8
```

---

# 2. traceroute Command

## Purpose

Shows the path packets take to reach the destination.

Install:

Ubuntu:

```bash
sudo apt install traceroute
```

RHEL:

```bash
sudo yum install traceroute
```

Example:

```bash
traceroute google.com
```

Output:

```
1 Router
2 ISP
3 Gateway
4 Google
```

Useful for:

* Routing problems
* High latency
* Network hops

---

# 3. tracepath Command

Works similarly to traceroute but usually requires no root privileges.

Example:

```bash
tracepath google.com
```

---

# 4. ssh Connectivity Test

Check whether SSH service is accessible.

Example:

```bash
ssh user@192.168.1.10
```

If successful:

```
password:
```

If failed:

```
Connection refused
```

or

```
Connection timed out
```

---

# 5. telnet Command

Test specific ports.

Install:

Ubuntu:

```bash
sudo apt install telnet
```

Example:

```bash
telnet google.com 80
```

Success:

```
Connected
```

Failure:

```
Connection refused
```

---

# 6. nc (Netcat)

Modern alternative to telnet.

## Check SSH Port

```bash
nc -zv server_ip 22
```

## Check HTTP

```bash
nc -zv server_ip 80
```

## Check HTTPS

```bash
nc -zv server_ip 443
```

Example:

```
Connection succeeded
```

---

# 7. curl Command

Checks web services.

Example:

```bash
curl http://google.com
```

Headers only:

```bash
curl -I google.com
```

Output:

```
HTTP/1.1 200 OK
```

---

# 8. wget Command

Test website accessibility.

```bash
wget google.com
```

Headers:

```bash
wget --spider google.com
```

---

# 9. nslookup

Checks DNS resolution.

Example:

```bash
nslookup google.com
```

Output:

```
Address: 142.x.x.x
```

---

# 10. dig Command

Advanced DNS lookup.

Install:

```bash
sudo apt install dnsutils
```

Example:

```bash
dig google.com
```

Short output:

```bash
dig +short google.com
```

---

# 11. host Command

Simple DNS query.

```bash
host google.com
```

Output:

```
google.com has address ...
```

---

# 12. nmap Command

Network scanning tool.

Install:

```bash
sudo apt install nmap
```

Scan host:

```bash
nmap 192.168.1.100
```

Scan SSH:

```bash
nmap -p 22 192.168.1.100
```

Scan multiple ports:

```bash
nmap -p 22,80,443 192.168.1.100
```

---

# Production Connectivity Checklist

## Step 1

Check IP reachability:

```bash
ping server_ip
```

## Step 2

Check DNS:

```bash
nslookup hostname
```

## Step 3

Check routing:

```bash
traceroute hostname
```

## Step 4

Check SSH:

```bash
nc -zv server_ip 22
```

## Step 5

Check HTTP:

```bash
curl -I website
```

## Step 6

Check open ports:

```bash
nmap server_ip
```

---

# Common Errors

## Request Timed Out

Possible causes:

* Server down
* Firewall
* Network issue

---

## Connection Refused

Possible causes:

* Service stopped
* Port blocked
* Wrong port

---

## No Route to Host

Possible causes:

* Routing problem
* Gateway issue

---

## Temporary Failure in Name Resolution

Possible causes:

* DNS issue
* Network configuration

---

# Shell Script Example

Check server availability.

```bash
#!/bin/bash

server=google.com

if ping -c 1 $server > /dev/null
then
    echo "$server is reachable"
else
    echo "$server is unreachable"
fi
```

---

# Production Server Health Check

```bash
#!/bin/bash

servers="google.com github.com"

for server in $servers
do
    ping -c 1 $server > /dev/null

    if [ $? -eq 0 ]
    then
        echo "$server UP"
    else
        echo "$server DOWN"
    fi
done
```

Output:

```
google.com UP
github.com UP
```

---

# Interview Questions

## Q1. Which command checks basic connectivity?

**Answer:**

```bash
ping
```

---

## Q2. Which command shows the packet route?

**Answer:**

```bash
traceroute
```

---

## Q3. Which command checks if SSH port 22 is open?

**Answer:**

```bash
nc -zv server_ip 22
```

---

## Q4. Which command tests a web server?

**Answer:**

```bash
curl -I website
```

---

## Q5. Which command checks DNS resolution?

**Answer:**

```bash
nslookup
```

or

```bash
dig
```

---

# Commands Every Linux Administrator Should Know

| Purpose            | Command                   |
| ------------------ | ------------------------- |
| Check reachability | `ping`                    |
| Check route        | `traceroute`              |
| Test ports         | `nc -zv`                  |
| Test SSH           | `ssh`                     |
| Test HTTP          | `curl -I`                 |
| Check DNS          | `nslookup`, `dig`, `host` |
| Scan ports         | `nmap`                    |
| Download test      | `wget`                    |
| Network path       | `tracepath`               |

## Production Troubleshooting Order

1. `ping`
2. `nslookup`
3. `traceroute`
4. `nc -zv`
5. `ssh`
6. `curl`
7. `nmap`

This sequence is commonly used by Linux and DevOps engineers to diagnose remote server connectivity problems efficiently.
