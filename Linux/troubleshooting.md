# Basic Troubleshooting

## 🔍 Overview

Basic Troubleshooting হল Linux system এ সমস্যা শনাক্ত, বিশ্লেষণ, এবং সমাধান করার প্রক্রিয়া। এটি system administration এর একটি গুরুত্বপূর্ণ দক্ষতা।

## 🎯 Troubleshooting Objectives

- **Problem Identification**: Identify system issues and problems
- **Root Cause Analysis**: Find the root cause of problems
- **Solution Implementation**: Implement appropriate solutions
- **System Recovery**: Restore system to normal operation
- **Prevention**: Prevent similar issues in the future

## 🔧 Troubleshooting Methodology

### 1. Problem Identification
- **Symptoms**: Observe and document symptoms
- **Timeline**: Determine when the problem started
- **Scope**: Identify affected systems and users
- **Impact**: Assess the impact of the problem

### 2. Information Gathering
- **System Logs**: Check relevant log files
- **System Status**: Check system resources and status
- **User Reports**: Gather information from users
- **Recent Changes**: Identify recent system changes

### 3. Root Cause Analysis
- **System Analysis**: Analyze system components
- **Log Analysis**: Review log files for errors
- **Resource Analysis**: Check system resources
- **Configuration Review**: Review system configurations

## 🛠️ System Information Commands

### System Status
```bash
# System information
uname -a
hostnamectl
lsb_release -a

# System uptime
uptime
w
who

# System resources
free -h
df -h
top
htop
```

### System Logs
```bash
# System logs
sudo journalctl -f
sudo tail -f /var/log/syslog
sudo tail -f /var/log/messages

# Boot logs
sudo journalctl -b
sudo dmesg
sudo journalctl -k

# Service logs
sudo journalctl -u service_name
sudo systemctl status service_name
```

### Process Information
```bash
# Running processes
ps aux
ps aux --sort=-%cpu
ps aux --sort=-%mem

# Process details
ps -ef | grep process_name
pgrep process_name
pidof process_name

# Process resources
top
htop
iotop
```

## 🔍 Common System Issues

### 1. Boot Issues
```bash
# Check boot logs
sudo journalctl -b
sudo dmesg | grep -i error
sudo dmesg | grep -i fail

# Check boot configuration
sudo systemctl list-unit-files
sudo systemctl list-dependencies

# Check filesystem
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
```

### 2. Network Issues
```bash
# Check network connectivity
ping 8.8.8.8
ping google.com
traceroute 8.8.8.8

# Check network configuration
ip addr show
ip route show
cat /etc/resolv.conf

# Check network services
sudo systemctl status networking
sudo systemctl status NetworkManager
```

### 3. Disk Issues
```bash
# Check disk space
df -h
du -h /path/to/directory

# Check disk health
sudo smartctl -a /dev/sda
sudo badblocks -v /dev/sda1

# Check filesystem
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
```

### 4. Memory Issues
```bash
# Check memory usage
free -h
cat /proc/meminfo

# Check memory processes
ps aux --sort=-%mem | head -10

# Check swap usage
swapon -s
cat /proc/swaps
```

## 🔧 Troubleshooting Tools

### System Monitoring
```bash
# System monitoring
top
htop
iotop
nethogs
iftop

# System statistics
sar -u 1 5
sar -r 1 5
sar -d 1 5
sar -n DEV 1 5
```

### Log Analysis
```bash
# Log analysis
sudo grep -i error /var/log/syslog
sudo grep -i fail /var/log/syslog
sudo grep -i warning /var/log/syslog

# Log rotation
sudo logrotate -f /etc/logrotate.conf
sudo systemctl restart logrotate
```

### Network Troubleshooting
```bash
# Network diagnostics
ip addr show
ip route show
ss -tuln
netstat -tuln

# Network testing
ping -c 4 8.8.8.8
traceroute 8.8.8.8
nslookup google.com
dig google.com
```

## 🔍 Service Troubleshooting

### Service Status
```bash
# Check service status
sudo systemctl status service_name
sudo systemctl is-active service_name
sudo systemctl is-enabled service_name

# Service logs
sudo journalctl -u service_name
sudo journalctl -u service_name -f
```

### Service Management
```bash
# Start/stop services
sudo systemctl start service_name
sudo systemctl stop service_name
sudo systemctl restart service_name
sudo systemctl reload service_name

# Enable/disable services
sudo systemctl enable service_name
sudo systemctl disable service_name
```

### Service Configuration
```bash
# Check service configuration
sudo systemctl show service_name
sudo systemctl cat service_name

# Edit service configuration
sudo systemctl edit service_name
sudo systemctl daemon-reload
```

## 🔧 Performance Troubleshooting

### CPU Issues
```bash
# CPU monitoring
top
htop
sar -u 1 5
mpstat -P ALL 1 5

# CPU information
cat /proc/cpuinfo
lscpu
```

### Memory Issues
```bash
# Memory monitoring
free -h
cat /proc/meminfo
sar -r 1 5

# Memory processes
ps aux --sort=-%mem | head -10
```

### Disk Issues
```bash
# Disk monitoring
df -h
du -h /path/to/directory
iostat -x 1 5
iotop

# Disk information
lsblk
fdisk -l
```

## 🔍 Security Troubleshooting

### Security Issues
```bash
# Check security logs
sudo grep -i "failed password" /var/log/auth.log
sudo grep -i "invalid user" /var/log/auth.log
sudo grep -i "authentication failure" /var/log/auth.log

# Check user activity
last
lastb
w
who
```

### Permission Issues
```bash
# Check file permissions
ls -la /path/to/file
stat /path/to/file

# Check directory permissions
ls -ld /path/to/directory
stat /path/to/directory

# Fix permissions
sudo chmod 755 /path/to/file
sudo chown user:group /path/to/file
```

## 🔧 Troubleshooting Scripts

### System Health Check Script
```bash
#!/bin/bash
# System health check script

echo "=== System Health Report ==="
echo "Date: $(date)"
echo ""

echo "=== System Information ==="
uname -a
echo ""

echo "=== System Uptime ==="
uptime
echo ""

echo "=== Memory Usage ==="
free -h
echo ""

echo "=== Disk Usage ==="
df -h
echo ""

echo "=== CPU Usage ==="
top -bn1 | grep "Cpu(s)"
echo ""

echo "=== Top Processes ==="
ps aux --sort=-%cpu | head -5
echo ""

echo "=== Network Status ==="
ip addr show
echo ""

echo "=== Service Status ==="
sudo systemctl list-units --failed
echo ""

echo "=== Recent Errors ==="
sudo journalctl -p err -n 10
echo ""
```

### Network Troubleshooting Script
```bash
#!/bin/bash
# Network troubleshooting script

echo "=== Network Troubleshooting Report ==="
echo "Date: $(date)"
echo ""

echo "=== Network Interfaces ==="
ip addr show
echo ""

echo "=== Routing Table ==="
ip route show
echo ""

echo "=== DNS Configuration ==="
cat /etc/resolv.conf
echo ""

echo "=== Network Connectivity ==="
ping -c 4 8.8.8.8
echo ""

echo "=== Network Services ==="
sudo systemctl status networking
sudo systemctl status NetworkManager
echo ""

echo "=== Active Connections ==="
ss -tuln | head -10
echo ""
```

## 🔍 Recovery Procedures

### System Recovery
```bash
# Boot recovery
sudo systemctl rescue
sudo systemctl emergency

# Filesystem recovery
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1

# Service recovery
sudo systemctl restart service_name
sudo systemctl reload service_name
```

### Data Recovery
```bash
# File recovery
sudo extundelete /dev/sda1 --restore-file /path/to/file
sudo extundelete /dev/sda1 --restore-directory /path/to/directory

# Backup restoration
sudo tar -xzf backup.tar.gz -C /path/to/restore
sudo rsync -av /backup/ /path/to/restore
```

## 📈 Troubleshooting Best Practices

### 1. Documentation
- Document all troubleshooting steps
- Keep logs of system changes
- Maintain system documentation
- Record solutions for future reference

### 2. Prevention
- Regular system maintenance
- Monitor system health
- Implement monitoring tools
- Regular security audits

### 3. Communication
- Communicate with users
- Provide status updates
- Document resolution steps
- Share knowledge with team

### 4. Learning
- Analyze root causes
- Learn from incidents
- Improve processes
- Share lessons learned

## 🎯 Troubleshooting Commands Summary

### System Information
```bash
uname -a              # System information
uptime                # System uptime
free -h               # Memory usage
df -h                 # Disk usage
top                   # Process monitoring
```

### Log Analysis
```bash
sudo journalctl -f    # System logs
sudo tail -f /var/log/syslog  # System log
sudo dmesg            # Kernel messages
```

### Network Troubleshooting
```bash
ping 8.8.8.8         # Network connectivity
traceroute 8.8.8.8   # Network path
ip addr show          # Network interfaces
ss -tuln              # Network connections
```

### Service Management
```bash
sudo systemctl status service_name  # Service status
sudo systemctl restart service_name  # Restart service
sudo journalctl -u service_name      # Service logs
```

## 🎯 Summary

Basic Troubleshooting হল Linux system administration এর একটি গুরুত্বপূর্ণ দক্ষতা যা:

- ✅ System problems identify করে
- ✅ Root cause analysis করে
- ✅ Appropriate solutions implement করে
- ✅ System recovery নিশ্চিত করে
- ✅ Future problems prevent করে

সঠিক troubleshooting methodology এবং tools ব্যবহার করে system issues efficiently resolve করা যায় এবং system stability maintain করা যায়।
