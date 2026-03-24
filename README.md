# LINUX_TOOLKIT
I built out this linux toolkit after spending 36 hours learning linux fundamentals and bash programming language. This toolkit displays my understanding of the fundamental and I hope can be my foundation to becoming a great engineer.

# Linux System Automation Toolkit Project

## Project Overview

Build a Linux-based automation toolkit to monitor, maintain, and troubleshoot server environments using Bash scripting and system tools.

---

# Project Structure

Create project directory:

```bash
mkdir linux-system-toolkit
cd linux-system-toolkit

mkdir scripts logs backups
touch README.md
```

Final Structure:

```
linux-system-toolkit/
 ├── scripts/
 ├── logs/
 ├── backups/
 ├── README.md
```

---

# Step 1 — System Health Script

File: scripts/system_health.sh

```bash
#!/bin/bash

echo "===== SYSTEM HEALTH REPORT ====="
echo "Date: $(date)"
echo ""

echo "Uptime:"
uptime
echo ""

echo "CPU Load:"
top -bn1 | grep "load average"
echo ""

echo "Memory Usage:"
free -m
echo ""

echo "Disk Usage:"
df -h
```

Make executable:

```bash
chmod +x scripts/system_health.sh
./scripts/system_health.sh
```

---

# Step 2 — Disk Usage Monitor

File: scripts/disk_monitor.sh

```bash
#!/bin/bash

THRESHOLD=80

df -h | grep -vE '^Filesystem|tmpfs' | while read line; do
  usage=$(echo $line | awk '{print $5}' | sed 's/%//')
  partition=$(echo $line | awk '{print $1}')

  if [ $usage -ge $THRESHOLD ]; then
    echo "WARNING: $partition is ${usage}% full" >> logs/disk_alert.log
  fi
done
```

Run:

```bash
chmod +x scripts/disk_monitor.sh
./scripts/disk_monitor.sh
```

---

# Step 3 — Log Analyzer

File: scripts/log_analyzer.sh

```bash
#!/bin/bash

LOG_FILE="/var/log/syslog"

echo "===== ERROR LOG REPORT ====="
grep -i "error" $LOG_FILE | tail -n 20
```

---

# Step 4 — Cleanup Script

File: scripts/cleanup.sh

```bash
#!/bin/bash

echo "Cleaning temp files..."
rm -rf /tmp/*

echo "Cleaning old logs..."
find logs/ -type f -mtime +7 -delete

echo "Cleanup completed."
```

---

# Step 5 — Backup Script

File: scripts/backup.sh

```bash
#!/bin/bash

tar -czf backups/backup_$(date +%F).tar.gz scripts/
echo "Backup completed"
```

---

# Step 6 — Cron Automation

Open cron:

```bash
crontab -e
```

Add:

```bash
* * * * * /root/linux-system-toolkit/scripts/system_health.sh >> /root/linux-system-toolkit/logs/health.log
```

---

# Step 7 — NGINX Real World Task

Install Nginx:

```bash
sudo apt update
sudo apt install nginx -y
```

Start nginx:

```bash
sudo systemctl start nginx
```

Check status:

```bash
sudo systemctl status nginx
```

Access localhost:

```bash
curl http://localhost
```

Check port:

```bash
ss -tuln | grep 80
```

---

# Break Nginx Intentionally

Edit config:

```bash
sudo nano /etc/nginx/sites-enabled/default
```

Change:

```
listen 80;
```

To:

```
listen 9999;
```

Restart nginx:

```bash
sudo systemctl restart nginx
```

Debug:

```bash
curl http://localhost
journalctl -u nginx
ss -tuln
```

Fix back to port 80 and restart.

---

# README Template

```markdown
# Linux System Automation Toolkit

## Overview
A Linux-based toolkit built using Bash scripting to automate system monitoring, log analysis, disk usage alerts, and backups.

## Features
- System health monitoring
- Disk usage alerts
- Log analysis
- Automated cleanup
- Cron automation
- Nginx debugging

## Tech Stack
- Linux
- Bash
- systemd
- cron

## Learnings
- Linux administration
- Automation
- Debugging services
- Monitoring systems

```

--

# Final Deliverables

* GitHub Repository
* Bash Scripts
* Documentation
* LinkedIn Post

This project demonstrates real-world Linux and DevOps skills.
