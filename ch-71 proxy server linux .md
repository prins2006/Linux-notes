# Proxy Server in Linux (Squid)

## What is a Proxy Server?

A Proxy Server acts as an intermediary between client computers and the Internet.

Instead of directly accessing websites, users send requests to the proxy server. The proxy server then fetches the requested content and returns it to the user.

### Without Proxy

Client → Internet

### With Proxy

Client → Proxy Server → Internet

---

# Why Use a Proxy Server?

- Control internet access.
- Monitor user activity.
- Cache web pages to improve speed.
- Block unwanted websites.
- Save bandwidth.
- Increase security.
- Hide internal network information.

---

# What is Squid?

Squid is an open-source caching and forwarding proxy server.

It supports:

- HTTP
- HTTPS
- FTP
- Caching
- Access Control Lists (ACLs)
- Authentication

---

# Squid Architecture

```

+-----------+
| Client PC |
+-----------+
|
v
+---------------+
| Squid Proxy |
| Port 3128 |
+---------------+
|
v
+------------+
| Internet |
+------------+

```

---

# Install Squid

## Ubuntu / Debian

```bash
sudo apt update
sudo apt install squid -y
```

## RHEL / Rocky / AlmaLinux

```bash
sudo dnf install squid -y
```

Verify installation:

```bash
squid -v
```

Example Output:

```bash
Squid Cache: Version 5.x
```

---

# Important Files

| File | Purpose |
|--------|----------|
| /etc/squid/squid.conf | Main configuration |
| /var/log/squid/access.log | Access logs |
| /var/log/squid/cache.log | Cache logs |
| /var/spool/squid | Cache storage |

---

# Start Squid Service

```bash
sudo systemctl start squid
```

Enable on boot:

```bash
sudo systemctl enable squid
```

Check status:

```bash
sudo systemctl status squid
```

Example:

```bash
active (running)
```

---

# Verify Listening Port

Squid uses Port 3128 by default.

```bash
sudo ss -tulpn | grep 3128
```

Example Output:

```bash
tcp LISTEN 0 128 *:3128 *:* users:(("squid",pid=1234))
```

---

# Basic Configuration

Open configuration file:

```bash
sudo vi /etc/squid/squid.conf
```

---

# Allow Local Network

Example network:

```bash
192.168.1.0/24
```

Add:

```bash
acl localnet src 192.168.1.0/24
http_access allow localnet
```

Meaning:

- ACL = Access Control List
- Allow clients from 192.168.1.0 network

---

# Restart Squid

After changes:

```bash
sudo systemctl restart squid
```

---

# Configure Client Browser

Proxy IP:

```text
192.168.1.10
```

Port:

```text
3128
```

Browser Settings:

```text
HTTP Proxy:
192.168.1.10

Port:
3128
```

Now all browser traffic passes through Squid.

---

# Testing Proxy

Client Machine:

```bash
curl -x http://192.168.1.10:3128 http://example.com
```

Example:

```bash
curl -x http://192.168.1.10:3128 http://google.com
```

---

# View Access Logs

Monitor requests:

```bash
sudo tail -f /var/log/squid/access.log
```

Example Output:

```bash
172.16.1.20 TCP_MISS/200 GET http://google.com
```

Meaning:

| Field | Meaning |
|---------|---------|
| 172.16.1.20 | Client IP |
| TCP_MISS | Not in cache |
| 200 | HTTP Status |
| GET | Request Type |

---

# Squid Caching

When a webpage is requested:

### First Request

```text
Client → Squid → Internet
```

Result:

```text
TCP_MISS
```

Page stored in cache.

### Second Request

```text
Client → Squid Cache
```

Result:

```text
TCP_HIT
```

Faster response.

---

# Block Websites

Create ACL:

```bash
acl blocked_sites dstdomain .facebook.com
http_access deny blocked_sites
```

Restart:

```bash
sudo systemctl restart squid
```

Now Facebook is blocked.

---

# Block Multiple Websites

```bash
acl blocked_sites dstdomain .facebook.com .youtube.com .instagram.com
http_access deny blocked_sites
```

---

# Allow Only Specific Websites

```bash
acl allowed_sites dstdomain .google.com .github.com

http_access allow allowed_sites
http_access deny all
```

Result:

Only Google and GitHub accessible.

---

# Block Specific IP

```bash
acl baduser src 192.168.1.50
http_access deny baduser
```

---

# Allow Specific IP

```bash
acl managerpc src 192.168.1.100
http_access allow managerpc
```

---

# Time-Based Restrictions

Office hours only:

```bash
acl officehours time MTWHF 09:00-18:00
http_access allow officehours
http_access deny all
```

Meaning:

Monday-Friday

09:00 AM to 06:00 PM

---

# Proxy Authentication

Install utilities:

```bash
sudo apt install apache2-utils
```

Create password file:

```bash
sudo htpasswd -c /etc/squid/passwd user1
```

Configure:

```bash
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

acl authenticated proxy_auth REQUIRED

http_access allow authenticated
```

Restart:

```bash
sudo systemctl restart squid
```

Users must authenticate before browsing.

---

# Transparent Proxy

Users do not configure proxy manually.

Traffic automatically redirected using iptables.

Example:

```bash
sudo iptables -t nat -A PREROUTING \
-p tcp --dport 80 \
-j REDIRECT --to-port 3128
```

Flow:

```text
Client → Router → Squid → Internet
```

---

# Squid Log Files

## Access Log

```bash
/var/log/squid/access.log
```

Contains:

- Client IP
- Requested URL
- Status Code
- Access Time

---

## Cache Log

```bash
/var/log/squid/cache.log
```

Contains:

- Startup information
- Errors
- Warnings

View:

```bash
tail -f /var/log/squid/cache.log
```

---

# Check Configuration

Before restarting:

```bash
sudo squid -k parse
```

Output:

```bash
Configuration file is valid
```

---

# Reload Configuration

Without stopping service:

```bash
sudo squid -k reconfigure
```

---

# Common Troubleshooting

## Service Not Starting

Check:

```bash
sudo systemctl status squid
```

---

## Syntax Errors

Check:

```bash
sudo squid -k parse
```

---

## Firewall Blocking

Allow port:

```bash
sudo firewall-cmd --permanent --add-port=3128/tcp
sudo firewall-cmd --reload
```

Ubuntu:

```bash
sudo ufw allow 3128/tcp
```

---

# Useful Commands

| Command | Purpose |
|-----------|---------|
| squid -v | Version |
| systemctl start squid | Start Service |
| systemctl stop squid | Stop Service |
| systemctl restart squid | Restart Service |
| systemctl status squid | Status |
| squid -k parse | Verify Config |
| squid -k reconfigure | Reload Config |
| tail -f access.log | Monitor Requests |
| ss -tulpn \| grep 3128 | Verify Port |

---

# Real-World Example

Company Network:

```text
192.168.1.0/24
```

Requirements:

- Allow internet access.
- Block Facebook.
- Block YouTube.
- Enable caching.
- Monitor usage.

Configuration:

```bash
acl localnet src 192.168.1.0/24

acl socialmedia dstdomain \
.facebook.com \
.youtube.com

http_access deny socialmedia
http_access allow localnet
```

Result:

✓ Employees can browse websites.

✓ Facebook blocked.

✓ YouTube blocked.

✓ Web pages cached.

✓ Activity logged.

---

# Interview Questions

### What is Squid?

An open-source caching and forwarding proxy server.

### Default Squid Port?

```text
3128
```

### What is ACL?

Access Control List used to define access rules.

### Difference Between TCP_HIT and TCP_MISS?

TCP_HIT:
Data served from cache.

TCP_MISS:
Data fetched from Internet.

### Configuration File Location?

```bash
/etc/squid/squid.conf
```

### Log File Location?

```bash
/var/log/squid/access.log
```

### Check Configuration Syntax?

```bash
squid -k parse
```

---

# Summary

- Squid is a Linux proxy server.
- Default port is 3128.
- Supports caching, filtering, authentication, and monitoring.
- ACLs control who can access the Internet.
- Websites can be blocked or allowed.
- Logs help monitor user activity.
- Caching improves speed and saves bandwidth.