# System Monitoring

## 📊 Overview

System Monitoring হল Linux সিস্টেমের পারফরমেন্স, রিসোর্স ব্যবহার, এবং সিস্টেম হেলথ ট্র্যাক করার প্রক্রিয়া। এটি সিস্টেম অ্যাডমিনিস্ট্রেটরদের জন্য অত্যন্ত গুরুত্বপূর্ণ।

## 🎯 Monitoring Objectives

- **Performance Tracking**: CPU, Memory, Disk, Network usage
- **Resource Management**: Identify bottlenecks and optimize resources
- **System Health**: Monitor system stability and availability
- **Capacity Planning**: Plan for future resource requirements
- **Troubleshooting**: Identify and resolve system issues

## 🔧 Core Monitoring Tools

### 1. top - Real-time Process Viewer
```bash
# Basic top command
top

# Customize display
top -u username          # Show processes for specific user
top -p PID1,PID2         # Show specific processes
top -n 1                 # Run once and exit
top -b                   # Batch mode
top -d 5                 # Update every 5 seconds
```

### 2. htop - Enhanced Process Viewer
```bash
# Install htop
sudo apt install htop    # Ubuntu/Debian
sudo yum install htop    # CentOS/RHEL

# Run htop
htop

# htop options
htop -u username          # Filter by user
htop -s CPU               # Sort by CPU
htop -s MEM               # Sort by memory
```

### 3. iotop - I/O Monitoring
```bash
# Install iotop
sudo apt install iotop    # Ubuntu/Debian
sudo yum install iotop    # CentOS/RHEL

# Run iotop
sudo iotop

# iotop options
sudo iotop -o             # Only show processes doing I/O
sudo iotop -a             # Show accumulated I/O
sudo iotop -d 2           # Update every 2 seconds
```

## 📈 System Resource Monitoring

### CPU Monitoring
```bash
# CPU information
cat /proc/cpuinfo
lscpu

# CPU usage
top
htop
sar -u 1 5                # CPU usage every 1 second for 5 times

# CPU load average
uptime
cat /proc/loadavg

# CPU temperature (if supported)
sensors
cat /sys/class/thermal/thermal_zone*/temp
```

### Memory Monitoring
```bash
# Memory information
free -h
cat /proc/meminfo

# Memory usage by process
ps aux --sort=-%mem | head -10
top -o %MEM

# Memory usage over time
sar -r 1 5                # Memory usage every 1 second for 5 times

# Swap usage
swapon -s
cat /proc/swaps
```

### Disk Monitoring
```bash
# Disk usage
df -h
du -h /path/to/directory

# Disk I/O
iostat -x 1 5             # Disk I/O every 1 second for 5 times
iotop

# Disk space by directory
du -h --max-depth=1 / | sort -hr

# Inode usage
df -i
```

### Network Monitoring
```bash
# Network interfaces
ip addr show
ifconfig

# Network statistics
cat /proc/net/dev
ss -tuln                   # Network connections
netstat -tuln              # Alternative to ss

# Network traffic
iftop                      # Install: sudo apt install iftop
nethogs                   # Install: sudo apt install nethogs

# Network I/O
sar -n DEV 1 5            # Network device statistics
```

## 🔍 Advanced Monitoring Tools

### 1. sar - System Activity Reporter
```bash
# Install sysstat
sudo apt install sysstat   # Ubuntu/Debian
sudo yum install sysstat   # CentOS/RHEL

# Enable sar collection
sudo systemctl enable sysstat
sudo systemctl start sysstat

# View sar data
sar -u                    # CPU usage
sar -r                    # Memory usage
sar -d                    # Disk activity
sar -n DEV                # Network statistics
sar -A                    # All statistics

# Real-time monitoring
sar -u 1 10               # CPU every 1 second for 10 times
```

### 2. vmstat - Virtual Memory Statistics
```bash
# Basic vmstat
vmstat

# Continuous monitoring
vmstat 1 10               # Every 1 second for 10 times
vmstat 5                  # Every 5 seconds indefinitely

# Detailed output
vmstat -a                 # Active/inactive memory
vmstat -s                 # Summary statistics
```

### 3. iostat - I/O Statistics
```bash
# Install sysstat for iostat
sudo apt install sysstat

# Basic iostat
iostat

# Continuous monitoring
iostat 1 5                # Every 1 second for 5 times
iostat -x                  # Extended statistics
iostat -d 1 5             # Disk statistics only
```

### 4. netstat/ss - Network Statistics
```bash
# Network connections
netstat -tuln              # TCP/UDP listening ports
netstat -tulnp             # With process information
ss -tuln                   # Modern alternative to netstat
ss -tulnp                  # With process information

# Network routing
ip route show
netstat -rn

# Network interface statistics
cat /proc/net/dev
ip -s link show
```

## 📊 Log Monitoring

### System Logs
```bash
# System log
tail -f /var/log/syslog
journalctl -f              # systemd journal

# Authentication log
tail -f /var/log/auth.log
journalctl -f _COMM=sshd   # SSH log

# Kernel log
dmesg
journalctl -k              # Kernel messages

# Boot log
journalctl -b              # Current boot
journalctl -b -1           # Previous boot
```

### Application Logs
```bash
# Web server logs
tail -f /var/log/apache2/access.log
tail -f /var/log/nginx/access.log

# Database logs
tail -f /var/log/mysql/error.log
tail -f /var/log/postgresql/postgresql.log

# Custom application logs
tail -f /var/log/application.log
```

## 🚨 Alerting and Notifications

### Simple Alert Scripts
```bash
#!/bin/bash
# CPU usage alert
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d'%' -f1)
if [ $CPU_USAGE -gt 80 ]; then
    echo "High CPU usage: $CPU_USAGE%" | mail -s "CPU Alert" admin@example.com
fi

# Memory usage alert
MEM_USAGE=$(free | grep Mem | awk '{printf("%.2f", $3/$2 * 100.0)}')
if [ $(echo "$MEM_USAGE > 80" | bc) -eq 1 ]; then
    echo "High memory usage: $MEM_USAGE%" | mail -s "Memory Alert" admin@example.com
fi

# Disk space alert
DISK_USAGE=$(df / | awk 'NR==2 {print $5}' | cut -d'%' -f1)
if [ $DISK_USAGE -gt 80 ]; then
    echo "High disk usage: $DISK_USAGE%" | mail -s "Disk Alert" admin@example.com
fi
```

### Monitoring Scripts
```bash
#!/bin/bash
# System health check
echo "=== System Health Report ==="
echo "Date: $(date)"
echo "Uptime: $(uptime)"
echo "Load Average: $(cat /proc/loadavg)"
echo "Memory Usage:"
free -h
echo "Disk Usage:"
df -h
echo "Top Processes:"
ps aux --sort=-%cpu | head -5
```

## 📈 Performance Analysis

### CPU Performance
```bash
# CPU bottlenecks
top -o %CPU
htop -s CPU

# CPU frequency
cat /proc/cpuinfo | grep MHz
cpufreq-info

# CPU cores utilization
mpstat -P ALL 1 5
```

### Memory Performance
```bash
# Memory bottlenecks
top -o %MEM
htop -s MEM

# Memory leaks
ps aux --sort=-%mem | head -10
cat /proc/meminfo | grep -E "(MemTotal|MemFree|MemAvailable)"

# Swap usage
swapon -s
cat /proc/swaps
```

### Disk Performance
```bash
# Disk bottlenecks
iostat -x 1 5
iotop

# Disk queue
iostat -x | grep -E "(Device|sd)"
cat /proc/diskstats
```

## 🔧 Monitoring Configuration

### Cron Jobs for Monitoring
```bash
# Add to crontab
crontab -e

# Every 5 minutes
*/5 * * * * /path/to/monitoring_script.sh

# Every hour
0 * * * * /path/to/hourly_check.sh

# Daily report
0 9 * * * /path/to/daily_report.sh
```

### Log Rotation
```bash
# Configure logrotate
sudo nano /etc/logrotate.d/monitoring

# Example configuration
/var/log/monitoring.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
}
```

## 📊 Monitoring Dashboards

### Simple Dashboard Script
```bash
#!/bin/bash
# Simple monitoring dashboard
clear
echo "=== System Monitoring Dashboard ==="
echo "Date: $(date)"
echo "Uptime: $(uptime)"
echo ""
echo "=== CPU Usage ==="
top -bn1 | grep "Cpu(s)"
echo ""
echo "=== Memory Usage ==="
free -h
echo ""
echo "=== Disk Usage ==="
df -h
echo ""
echo "=== Top Processes ==="
ps aux --sort=-%cpu | head -5
echo ""
echo "=== Network Connections ==="
ss -tuln | wc -l
echo "Active connections: $(ss -tuln | wc -l)"
```

## 🛡️ Security Monitoring

### Security Logs
```bash
# Failed login attempts
grep "Failed password" /var/log/auth.log
grep "Invalid user" /var/log/auth.log

# SSH connections
grep "Accepted" /var/log/auth.log
grep "Connection closed" /var/log/auth.log

# System access
last
lastb
w
who
```

### Intrusion Detection
```bash
# Check for suspicious processes
ps aux | grep -E "(nc|netcat|nmap|masscan)"

# Check network connections
ss -tuln | grep -E ":(22|80|443|3306|5432)"

# Check file modifications
find /etc -type f -mtime -1 -ls
```

## 📈 Capacity Planning

### Resource Trends
```bash
# Historical data
sar -u -f /var/log/sa/sa01    # CPU usage from specific date
sar -r -f /var/log/sa/sa01    # Memory usage from specific date
sar -d -f /var/log/sa/sa01    # Disk usage from specific date

# Generate reports
sar -A -f /var/log/sa/sa01 > system_report.txt
```

### Growth Projections
```bash
# Disk space growth
du -h /var/log | sort -hr
du -h /home | sort -hr

# Log file sizes
find /var/log -name "*.log" -exec ls -lh {} \; | sort -k5 -hr
```

## 🎯 Best Practices

### 1. Regular Monitoring
- Set up automated monitoring scripts
- Monitor critical resources continuously
- Set up alerts for threshold breaches

### 2. Log Management
- Implement log rotation
- Monitor log file sizes
- Archive old logs

### 3. Performance Baseline
- Establish performance baselines
- Monitor trends over time
- Plan for capacity growth

### 4. Security Monitoring
- Monitor authentication logs
- Check for suspicious activities
- Implement intrusion detection

## 🎯 Summary

System Monitoring হল Linux administration এর একটি গুরুত্বপূর্ণ অংশ যা:

- ✅ সিস্টেম পারফরমেন্স ট্র্যাক করে
- ✅ রিসোর্স ব্যবহার মনিটর করে
- ✅ সমস্যা শনাক্ত করে
- ✅ সিকিউরিটি নিশ্চিত করে
- ✅ ক্যাপাসিটি প্ল্যানিং করে

সঠিক monitoring tools এবং techniques ব্যবহার করে একটি stable এবং efficient Linux system পরিচালনা করা যায়।
