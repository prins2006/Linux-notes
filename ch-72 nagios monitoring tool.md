# Installing, Configuring and Managing Nagios

## What is Nagios?

Nagios is an open-source monitoring tool used to monitor:

- Servers
- Network devices
- Services
- Applications
- Databases

It helps administrators detect problems before they affect users.

---

# Why Use Nagios?

- Monitor server health
- Monitor network devices
- Monitor services (HTTP, SSH, DNS, FTP)
- Receive alerts for failures
- Generate reports
- Reduce downtime

---

# Nagios Architecture

```text
                 +----------------+
                 | Nagios Server  |
                 +----------------+
                         |
        ----------------------------------
        |              |                |
        v              v                v
   Linux Server   Windows Server    Router/Switch
      (NRPE)          (NSClient)       (SNMP)

```

Nagios Server collects information from remote hosts and services.

---

# Nagios Components

## 1. Nagios Core

Main monitoring engine.

Responsibilities:

- Execute checks
- Process results
- Generate alerts
- Create reports

---

## 2. Plugins

Programs used to perform checks.

Examples:

```bash
check_ping
check_http
check_ssh
check_disk
check_load
```

---

## 3. NRPE

Nagios Remote Plugin Executor

Used to monitor Linux systems remotely.

---

## 4. SNMP

Simple Network Management Protocol

Used for:

- Routers
- Switches
- Printers
- Firewalls

---

# Installation of Nagios (Ubuntu)

## Update System

```bash
sudo apt update
```

---

## Install Dependencies

```bash
sudo apt install -y apache2 php gcc make unzip wget
```

---

## Create Nagios User

```bash
sudo useradd nagios
sudo groupadd nagcmd

sudo usermod -aG nagcmd nagios
sudo usermod -aG nagcmd www-data
```

---

## Download Nagios

```bash
wget https://github.com/NagiosEnterprises/nagioscore/releases/download/nagios-4.5.9/nagios-4.5.9.tar.gz
```

Extract:

```bash
tar -xzf nagios-4.5.9.tar.gz
cd nagios-4.5.9
```

---

## Compile Nagios

```bash
./configure --with-command-group=nagcmd

make all
```

---

## Install Nagios

```bash
sudo make install
```

Install configuration:

```bash
sudo make install-config
```

Install command mode:

```bash
sudo make install-commandmode
```

Install Apache configuration:

```bash
sudo make install-webconf
```

---

# Install Plugins

Download:

```bash
wget https://nagios-plugins.org/download/nagios-plugins-2.4.12.tar.gz
```

Extract:

```bash
tar -xzf nagios-plugins-2.4.12.tar.gz
cd nagios-plugins-2.4.12
```

Compile:

```bash
./configure
make
sudo make install
```

---

# Create Web Login

```bash
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

Set password.

---

# Start Services

```bash
sudo systemctl restart apache2
sudo systemctl start nagios
```

Enable startup:

```bash
sudo systemctl enable nagios
```

---

# Access Nagios Web Interface

Open browser:

```text
http://SERVER-IP/nagios
```

Login:

```text
Username: nagiosadmin
Password: your-password
```

---

# Important Directories

| Directory | Purpose |
|------------|---------|
| /usr/local/nagios/etc | Configuration |
| /usr/local/nagios/libexec | Plugins |
| /usr/local/nagios/var | Logs |
| /usr/local/nagios/share | Web files |

---

# Configuration Files

## Main Configuration

```bash
/usr/local/nagios/etc/nagios.cfg
```

Controls overall Nagios operation.

---

## Host Configuration

```bash
/usr/local/nagios/etc/objects/hosts.cfg
```

Defines monitored hosts.

---

## Service Configuration

```bash
/usr/local/nagios/etc/objects/services.cfg
```

Defines monitored services.

---

# Adding a Host

Example:

```bash
define host {
    use             linux-server
    host_name       webserver
    alias           Apache Server
    address         192.168.1.100
}
```

Meaning:

| Parameter | Description |
|------------|-------------|
| host_name | Host identifier |
| alias | Friendly name |
| address | IP Address |

---

# Adding a Service

Example:

```bash
define service {
    use                     generic-service
    host_name               webserver
    service_description     HTTP
    check_command           check_http
}
```

Checks HTTP service availability.

---

# Monitoring Disk Usage

```bash
define service {
    use                     generic-service
    host_name               webserver
    service_description     Disk Usage
    check_command           check_disk
}
```

---

# Monitoring SSH

```bash
define service {
    use                     generic-service
    host_name               webserver
    service_description     SSH
    check_command           check_ssh
}
```

---

# Verify Configuration

Before restarting Nagios:

```bash
sudo /usr/local/nagios/bin/nagios \
-v /usr/local/nagios/etc/nagios.cfg
```

Expected Output:

```text
Things look okay - No serious problems found.
```

---

# Restart Nagios

```bash
sudo systemctl restart nagios
```

---

# Managing Nagios

## Check Status

```bash
sudo systemctl status nagios
```

---

## Start Nagios

```bash
sudo systemctl start nagios
```

---

## Stop Nagios

```bash
sudo systemctl stop nagios
```

---

## Restart Nagios

```bash
sudo systemctl restart nagios
```

---

# Nagios Service States

| Status | Meaning |
|----------|----------|
| OK | Service working |
| WARNING | Threshold exceeded |
| CRITICAL | Service failure |
| UNKNOWN | Unable to determine |

---

# Host States

| State | Meaning |
|---------|---------|
| UP | Host reachable |
| DOWN | Host unreachable |
| UNREACHABLE | Network path unavailable |

---

# Alerts and Notifications

Nagios can send alerts using:

- Email
- SMS
- Slack
- Teams
- Telegram

Example Email Alert:

```text
HOST DOWN ALERT

Host: webserver
IP: 192.168.1.100
Status: DOWN
```

---

# Monitoring Remote Linux Servers Using NRPE

Install NRPE:

```bash
sudo apt install nagios-nrpe-server
```

Configure:

```bash
sudo vi /etc/nagios/nrpe.cfg
```

Allow Nagios server:

```bash
allowed_hosts=192.168.1.10
```

Restart:

```bash
sudo systemctl restart nagios-nrpe-server
```

Test:

```bash
check_nrpe -H 192.168.1.100
```

Output:

```text
NRPE v4.x
```

---

# Log Files

Nagios Log:

```bash
/usr/local/nagios/var/nagios.log
```

View logs:

```bash
tail -f /usr/local/nagios/var/nagios.log
```

---

# Common Troubleshooting

## Nagios Not Starting

Check:

```bash
systemctl status nagios
```

---

## Configuration Error

Validate:

```bash
nagios -v /usr/local/nagios/etc/nagios.cfg
```

---

## Plugins Not Working

Check:

```bash
ls /usr/local/nagios/libexec
```

Verify plugin permissions.

---

## Web Page Not Opening

Check Apache:

```bash
systemctl status apache2
```

---

# Real-World Example

Company Requirements:

- Monitor Web Server
- Monitor SSH
- Monitor Disk Usage
- Receive Email Alerts

Configuration:

```text
Host:
webserver (192.168.1.100)

Services:
HTTP
SSH
Disk Usage
```

Result:

✓ Automatic monitoring

✓ Instant alerts

✓ Service health tracking

✓ Reduced downtime

---

# Interview Questions

### What is Nagios?

An open-source infrastructure monitoring tool.

### What is NRPE?

Nagios Remote Plugin Executor for monitoring Linux servers remotely.

### Default Nagios Web User?

```text
nagiosadmin
```

### How to Verify Configuration?

```bash
nagios -v /usr/local/nagios/etc/nagios.cfg
```

### What are Nagios Plugins?

Programs used to perform service and host checks.

### Difference Between Host and Service?

Host = Device/System

Service = Resource running on host

Example:

Host:
Server

Service:
HTTP, SSH, DNS

---

# Summary

- Nagios is used for infrastructure monitoring.
- Nagios Core performs monitoring tasks.
- Plugins perform actual checks.
- NRPE monitors remote Linux servers.
- Supports alerts, reports, and dashboards.
- Helps detect issues before users are affected.