# The `ss` Command in Linux

# Introduction

The **ss (Socket Statistics)** command is a modern Linux utility used to display information about:

- Network connections
- Listening ports
- TCP connections
- UDP connections
- Unix sockets
- Network statistics

It is the replacement for the older:

```bash
netstat
```

command from the **net-tools** package.

---

# What Does ss Mean?

```text
ss = Socket Statistics
```

A socket is an endpoint used for communication between two machines or processes.

Example:

```text
Client -------- Server
      Socket
```

---

# Why Use ss?

Compared to `netstat`:

- Faster
- More detailed
- Uses kernel information directly
- Available by default on most Linux distributions

---

# Basic Syntax

```bash
ss [options]
```

---

# Display All Connections

```bash
ss
```

Output Example:

```text
Netid State   Recv-Q Send-Q Local Address:Port
tcp   ESTAB   0      0      192.168.1.10:ssh
```

Shows active network connections.

---

# Show Listening Ports

```bash
ss -l
```

Example:

```text
LISTEN
```

Displays services waiting for connections.

---

# Show TCP Connections

```bash
ss -t
```

Example:

```text
tcp ESTAB
```

Displays TCP sockets only.

---

# Show UDP Connections

```bash
ss -u
```

Example:

```text
udp UNCONN
```

Displays UDP sockets only.

---

# Show Listening TCP Ports

```bash
ss -lt
```

Example:

```text
tcp LISTEN
```

---

# Show Listening UDP Ports

```bash
ss -lu
```

---

# Show Listening TCP and UDP Ports

```bash
ss -tuln
```

Most commonly used command.

Example:

```bash
ss -tuln
```

Output:

```text
Netid State  Local Address:Port

tcp LISTEN 0.0.0.0:22
tcp LISTEN 0.0.0.0:80
udp UNCONN 0.0.0.0:68
```

---

# Meaning of Options

| Option | Meaning |
|----------|----------|
| -t | TCP |
| -u | UDP |
| -l | Listening |
| -n | Numeric output |
| -p | Process information |
| -a | All sockets |

Example:

```bash
ss -tulnp
```

---

# Show Process Using Port

```bash
ss -tulnp
```

Example:

```text
tcp LISTEN 0 128 *:22 *:* users:(("sshd",pid=1000))
```

Meaning:

```text
Port 22 is used by sshd
PID = 1000
```

Useful for troubleshooting.

---

# Check SSH Service Port

```bash
ss -tulnp | grep ssh
```

Example:

```text
tcp LISTEN *:22
```

---

# Check HTTP Service

```bash
ss -tulnp | grep :80
```

---

# Check HTTPS Service

```bash
ss -tulnp | grep :443
```

---

# Display Established Connections

```bash
ss -t state established
```

Example:

```text
ESTAB
```

Shows active TCP sessions.

---

# Show SSH Sessions

```bash
ss -tn state established '( sport = :22 )'
```

Example:

```text
192.168.1.20:22
```

Useful for administrators to see connected users.

---

# Show Connection Summary

```bash
ss -s
```

Example:

```text
Total: 250

TCP: 120

UDP: 30
```

Provides network statistics summary.

---

# Show IPv4 Connections

```bash
ss -4
```

---

# Show IPv6 Connections

```bash
ss -6
```

---

# Filter by Port

Show port 22:

```bash
ss -tulnp | grep :22
```

Show port 80:

```bash
ss -tulnp | grep :80
```

Show port 443:

```bash
ss -tulnp | grep :443
```

---

# Filter by Process

Show SSH daemon:

```bash
ss -tulnp | grep sshd
```

Show Apache:

```bash
ss -tulnp | grep httpd
```

Show Nginx:

```bash
ss -tulnp | grep nginx
```

---

# Display UNIX Sockets

```bash
ss -x
```

Used for local inter-process communication.

Example:

```text
u_str LISTEN
```

---

# Real Production Examples

## Verify SSH is Running

```bash
ss -tulnp | grep :22
```

Output:

```text
tcp LISTEN *:22
```

---

## Verify Web Server

```bash
ss -tulnp | grep :80
```

Output:

```text
tcp LISTEN *:80
```

---

## Verify HTTPS

```bash
ss -tulnp | grep :443
```

Output:

```text
tcp LISTEN *:443
```

---

## Find Process Occupying Port

```bash
ss -tulnp
```

Example:

```text
users:(("nginx",pid=1200))
```

Useful when:

```text
Port already in use
```

errors occur.

---

# Understanding Output

Example:

```text
tcp LISTEN 0 128 0.0.0.0:22
```

| Field | Meaning |
|---------|---------|
| tcp | Protocol |
| LISTEN | Connection State |
| 0 | Receive Queue |
| 128 | Send Queue |
| 0.0.0.0 | Listen on all interfaces |
| 22 | Port Number |

---

# Common TCP States

| State | Meaning |
|----------|----------|
| LISTEN | Waiting for connection |
| ESTAB | Established |
| TIME-WAIT | Closing connection |
| CLOSE-WAIT | Waiting to close |
| SYN-SENT | Connection request sent |
| SYN-RECV | Connection request received |

Example:

```bash
ss -ant
```

Displays all TCP states.

---

# ss vs netstat

| Feature | ss | netstat |
|-----------|------|---------|
| Speed | Faster | Slower |
| Modern Tool | Yes | No |
| Default Install | Usually | Not Always |
| Process Info | Yes | Yes |
| Detailed Statistics | Better | Limited |
| Recommended | Yes | Legacy |

---

# Interview Questions

### What does ss stand for?

```text
Socket Statistics
```

---

### Which command replaces netstat?

```bash
ss
```

---

### Show listening ports?

```bash
ss -l
```

---

### Show all listening TCP/UDP ports?

```bash
ss -tuln
```

---

### Show process using a port?

```bash
ss -tulnp
```

---

### Show network summary?

```bash
ss -s
```

---

### Show only TCP connections?

```bash
ss -t
```

---

### Show only UDP connections?

```bash
ss -u
```

---

# Most Important Commands for Linux Administrators

```bash
ss -tuln
```
Show listening ports

```bash
ss -tulnp
```
Show listening ports with processes

```bash
ss -s
```
Show connection summary

```bash
ss -ant
```
Show all TCP connections

```bash
ss -4
```
IPv4 connections

```bash
ss -6
```
IPv6 connections

```bash
ss -x
```
Unix sockets

```bash
ss -t state established
```
Established connections

---

# Quick Revision

| Command | Purpose |
|----------|----------|
| `ss` | Show socket statistics |
| `ss -l` | Listening sockets |
| `ss -t` | TCP connections |
| `ss -u` | UDP connections |
| `ss -tuln` | Listening TCP/UDP ports |
| `ss -tulnp` | Ports with process details |
| `ss -s` | Network summary |
| `ss -4` | IPv4 connections |
| `ss -6` | IPv6 connections |
| `ss -x` | Unix sockets |

## Exam Tip

The most frequently used command in troubleshooting is:

```bash
ss -tulnp
```

Because it shows:

- Open ports
- Listening services
- Process names
- PID numbers

and helps identify which application is using a specific network port.