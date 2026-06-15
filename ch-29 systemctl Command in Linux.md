# `systemctl` Command in Linux

## What is `systemctl`?

`systemctl` is a command-line utility used to manage the **systemd** system and service manager in Linux.

It is used to:

* Start services
* Stop services
* Restart services
* Enable services at boot
* Disable services
* Check service status
* View logs and boot information

Most modern Linux distributions use `systemd`, including:

* Ubuntu
* Debian
* RHEL
* CentOS
* Rocky Linux
* AlmaLinux
* Fedora

---

# Syntax

```bash
systemctl [OPTIONS] COMMAND SERVICE_NAME
```

Example:

```bash
systemctl status ssh
```

---

# Why is `systemctl` Important?

In production environments, almost every application runs as a service.

Examples:

| Application | Service Name  |
| ----------- | ------------- |
| SSH         | ssh           |
| Apache      | apache2/httpd |
| Nginx       | nginx         |
| Docker      | docker        |
| MySQL       | mysql/mysqld  |
| PostgreSQL  | postgresql    |
| Jenkins     | jenkins       |
| Redis       | redis         |

System administrators use `systemctl` to manage these services.

---

# 1. Check Service Status

## Syntax

```bash
systemctl status service_name
```

Example:

```bash
systemctl status ssh
```

Output:

```
● ssh.service - OpenBSD Secure Shell server
Loaded: loaded
Active: active (running)
```

## Production Use

* Check if service is running.
* Troubleshoot application failures.
* Verify service health.

---

# 2. Start a Service

## Syntax

```bash
systemctl start service_name
```

Example:

```bash
sudo systemctl start nginx
```

## Production Use

* Start web servers.
* Start database services.
* Start Docker.

---

# 3. Stop a Service

## Syntax

```bash
systemctl stop service_name
```

Example:

```bash
sudo systemctl stop nginx
```

## Production Use

* Server maintenance.
* Application upgrades.
* Emergency shutdown.

---

# 4. Restart a Service

## Syntax

```bash
systemctl restart service_name
```

Example:

```bash
sudo systemctl restart nginx
```

## Production Use

After changing:

* Configuration files
* SSL certificates
* Application settings

---

# 5. Reload Service Configuration

Reload does not stop the service.

```bash
sudo systemctl reload nginx
```

## Difference

| Command | Effect                     |
| ------- | -------------------------- |
| restart | Stops and starts service   |
| reload  | Reloads configuration only |

---

# 6. Enable Service

Starts automatically after reboot.

```bash
sudo systemctl enable nginx
```

Output:

```
Created symlink...
```

## Production Use

Enable:

* SSH
* Docker
* Nginx
* Database servers

---

# 7. Disable Service

Prevent automatic startup.

```bash
sudo systemctl disable nginx
```

---

# 8. Check if Service is Enabled

```bash
systemctl is-enabled nginx
```

Output:

```
enabled
```

or

```
disabled
```

---

# 9. Check if Service is Running

```bash
systemctl is-active nginx
```

Output:

```
active
```

or

```
inactive
```

---

# 10. List Running Services

```bash
systemctl list-units --type=service
```

Shows active services.

---

# 11. List Failed Services

```bash
systemctl --failed
```

## Production Use

Quickly identify failed services.

---

# 12. View All Services

```bash
systemctl list-unit-files --type=service
```

---

# 13. Reboot the System

```bash
sudo systemctl reboot
```

---

# 14. Shutdown the System

```bash
sudo systemctl poweroff
```

---

# 15. Suspend the System

```bash
sudo systemctl suspend
```

---

# 16. Check Boot Time

```bash
systemctl status
```

Shows:

* System status
* Boot time
* Running services

---

# Service Unit Files

Systemd stores service definitions in:

```
/usr/lib/systemd/system/
/etc/systemd/system/
```

Example:

```
nginx.service
docker.service
ssh.service
mysql.service
```

View service file:

```bash
systemctl cat nginx
```

---

# Reload systemd After Service File Changes

If you modify a service file:

```bash
sudo systemctl daemon-reload
```

Then:

```bash
sudo systemctl restart nginx
```

---

# Production Scenario 1

## Nginx Website Down

Check status:

```bash
systemctl status nginx
```

Start service:

```bash
sudo systemctl start nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

Restart after config changes:

```bash
sudo systemctl restart nginx
```

---

# Production Scenario 2

## Docker Service Failed

Check:

```bash
systemctl status docker
```

Restart:

```bash
sudo systemctl restart docker
```

Verify:

```bash
systemctl is-active docker
```

---

# Production Scenario 3

## SSH Connection Failed

Check:

```bash
systemctl status ssh
```

Start:

```bash
sudo systemctl start ssh
```

Enable:

```bash
sudo systemctl enable ssh
```

---

# Common Service Names

| Service    | Name       |
| ---------- | ---------- |
| SSH        | ssh        |
| Nginx      | nginx      |
| Apache     | apache2    |
| Docker     | docker     |
| MySQL      | mysql      |
| PostgreSQL | postgresql |
| Redis      | redis      |
| Jenkins    | jenkins    |
| Cron       | cron       |

---

# Interview Questions

## Q1. What is systemctl?

A command to manage systemd services.

---

## Q2. Check service status.

```bash
systemctl status service_name
```

---

## Q3. Start a service.

```bash
systemctl start service_name
```

---

## Q4. Stop a service.

```bash
systemctl stop service_name
```

---

## Q5. Restart a service.

```bash
systemctl restart service_name
```

---

## Q6. Enable service at boot.

```bash
systemctl enable service_name
```

---

## Q7. Check failed services.

```bash
systemctl --failed
```

---

## Q8. Reload systemd configuration.

```bash
systemctl daemon-reload
```

---

# Most Used Production Commands

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl reload nginx
systemctl enable nginx
systemctl disable nginx
systemctl is-active nginx
systemctl is-enabled nginx
systemctl --failed
systemctl list-units --type=service
systemctl list-unit-files --type=service
systemctl cat nginx
systemctl daemon-reload
systemctl reboot
systemctl poweroff
```

---

# Quick Revision Table

| Command                   | Purpose                    |
| ------------------------- | -------------------------- |
| `systemctl status`        | Check service status       |
| `systemctl start`         | Start service              |
| `systemctl stop`          | Stop service               |
| `systemctl restart`       | Restart service            |
| `systemctl reload`        | Reload configuration       |
| `systemctl enable`        | Start service on boot      |
| `systemctl disable`       | Disable boot startup       |
| `systemctl is-active`     | Check running state        |
| `systemctl is-enabled`    | Check boot status          |
| `systemctl --failed`      | Show failed services       |
| `systemctl daemon-reload` | Reload service definitions |
| `systemctl reboot`        | Reboot system              |
| `systemctl poweroff`      | Shutdown system            |

## Daily Practice

```bash
systemctl status ssh
systemctl status nginx
systemctl is-active ssh
systemctl is-enabled ssh
systemctl --failed
systemctl list-units --type=service
systemctl list-unit-files --type=service
systemctl cat ssh
sudo systemctl restart ssh
sudo systemctl daemon-reload
```

**Production Tip:** Before restarting any production service, always check its status and validate configuration files. For example, test an Nginx configuration with `nginx -t` before running `systemctl restart nginx` to avoid service downtime.
