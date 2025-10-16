# Process Monitoring

## 🔍 Overview

Process Monitoring হল Linux সিস্টেমে চলমান প্রসেসগুলো ট্র্যাক, বিশ্লেষণ, এবং পরিচালনা করার প্রক্রিয়া। এটি system administration এর একটি গুরুত্বপূর্ণ অংশ।

## 🎯 Process Monitoring Objectives

- **Process Tracking**: Identify running processes and their resources
- **Performance Analysis**: Monitor CPU, memory, and I/O usage per process
- **Resource Management**: Control process resource consumption
- **Troubleshooting**: Identify problematic processes
- **Security**: Monitor for suspicious or unauthorized processes

## 🔧 Core Process Monitoring Tools

### 1. ps - Process Status
```bash
# Basic ps command
ps

# Show all processes
ps aux

# Show processes in tree format
ps auxf

# Show processes for specific user
ps -u username

# Show processes with custom format
ps -eo pid,ppid,cmd,%mem,%cpu

# Show processes by PID
ps -p 1234,5678

# Show processes with full command line
ps -ef
```

### 2. top - Real-time Process Viewer
```bash
# Basic top command
top

# Interactive top commands
# Press 'P' - Sort by CPU usage
# Press 'M' - Sort by memory usage
# Press 'T' - Sort by time
# Press 'k' - Kill process
# Press 'r' - Renice process
# Press 'q' - Quit

# Top with specific options
top -u username          # Show processes for specific user
top -p PID1,PID2         # Show specific processes
top -n 1                 # Run once and exit
top -b                   # Batch mode
top -d 5                 # Update every 5 seconds
```

### 3. htop - Enhanced Process Viewer
```bash
# Install htop
sudo apt install htop    # Ubuntu/Debian
sudo yum install htop    # CentOS/RHEL

# Run htop
htop

# htop interactive commands
# F1 - Help
# F2 - Setup
# F3 - Search
# F4 - Filter
# F5 - Tree view
# F6 - Sort by
# F7 - Decrease priority
# F8 - Increase priority
# F9 - Kill process
# F10 - Quit

# htop options
htop -u username          # Filter by user
htop -s CPU               # Sort by CPU
htop -s MEM               # Sort by memory
htop -s TIME              # Sort by time
```

## 📊 Process Information Commands

### Process Details
```bash
# Show process information
ps -p PID -o pid,ppid,cmd,%mem,%cpu,etime,user

# Show process tree
pstree
pstree -p                # Show PIDs
pstree -u                # Show users
pstree -h                # Highlight current process

# Show process hierarchy
ps -eo pid,ppid,cmd --forest

# Show process with parent-child relationship
ps -eo pid,ppid,cmd | grep -E "(PID|1234)"
```

### Process Resources
```bash
# Show process memory usage
ps -eo pid,cmd,%mem --sort=-%mem | head -10

# Show process CPU usage
ps -eo pid,cmd,%cpu --sort=-%cpu | head -10

# Show process I/O
sudo iotop -p PID

# Show process network usage
sudo nethogs

# Show process file usage
lsof -p PID
```

## 🔍 Advanced Process Monitoring

### 1. iotop - I/O Monitoring
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
sudo iotop -p PID         # Show specific process
```

### 2. nethogs - Network Usage by Process
```bash
# Install nethogs
sudo apt install nethogs  # Ubuntu/Debian
sudo yum install nethogs  # CentOS/RHEL

# Run nethogs
sudo nethogs

# nethogs options
sudo nethogs -d 2         # Update every 2 seconds
sudo nethogs -t           # Trace mode
sudo nethogs -c 5         # Refresh 5 times then exit
```

### 3. lsof - List Open Files
```bash
# Show files opened by process
lsof -p PID

# Show files opened by user
lsof -u username

# Show network connections
lsof -i

# Show specific port
lsof -i :80
lsof -i :22

# Show files in directory
lsof +D /path/to/directory

# Show files by name
lsof /path/to/file
```

## 📈 Process Performance Analysis

### CPU Usage Analysis
```bash
# Top CPU consuming processes
ps aux --sort=-%cpu | head -10

# CPU usage over time
top -bn1 | grep "Cpu(s)"
sar -u 1 5

# Per-process CPU usage
ps -eo pid,cmd,%cpu --sort=-%cpu

# CPU usage by user
ps -eo user,%cpu --sort=-%cpu | awk '{cpu[$1]+=$2} END {for (user in cpu) print user, cpu[user]}'
```

### Memory Usage Analysis
```bash
# Top memory consuming processes
ps aux --sort=-%mem | head -10

# Memory usage over time
free -h
sar -r 1 5

# Per-process memory usage
ps -eo pid,cmd,%mem --sort=-%mem

# Memory usage by user
ps -eo user,%mem --sort=-%mem | awk '{mem[$1]+=$2} END {for (user in mem) print user, mem[user]}'
```

### I/O Usage Analysis
```bash
# I/O usage by process
sudo iotop -o

# I/O statistics
iostat -x 1 5

# Per-process I/O
sudo iotop -p PID

# I/O usage over time
sar -d 1 5
```

## 🔧 Process Management

### Process Control
```bash
# Kill process
kill PID
kill -9 PID              # Force kill
killall process_name     # Kill all processes with name

# Send signals
kill -HUP PID            # Hangup signal
kill -TERM PID           # Termination signal
kill -KILL PID           # Kill signal
kill -STOP PID           # Stop process
kill -CONT PID           # Continue process

# Process priority
nice -n 10 command       # Start with priority 10
renice -n 5 PID          # Change priority to 5
renice -n -5 PID         # Increase priority
```

### Process Scheduling
```bash
# Show process priority
ps -eo pid,cmd,ni        # ni = nice value

# Change process priority
renice -n 10 PID         # Lower priority
renice -n -10 PID        # Higher priority

# Show process scheduling policy
ps -eo pid,cmd,cls       # cls = scheduling class
chrt -p PID              # Show scheduling policy
chrt -f -p 50 PID        # Set FIFO scheduling
chrt -r -p 50 PID        # Set round-robin scheduling
```

## 🔍 Process Troubleshooting

### Common Issues
```bash
# Zombie processes
ps aux | grep -E "(Z|zombie)"
ps -eo pid,ppid,state,cmd | grep -E "(Z|zombie)"

# Orphaned processes
ps -eo pid,ppid,cmd | awk '$2 == 1 && $1 != 1'

# High CPU processes
ps aux --sort=-%cpu | head -5

# High memory processes
ps aux --sort=-%mem | head -5

# Processes consuming disk I/O
sudo iotop -o
```

### Process Analysis
```bash
# Process dependencies
pstree -p PID

# Process file usage
lsof -p PID

# Process network connections
lsof -p PID -i

# Process environment
cat /proc/PID/environ | tr '\0' '\n'

# Process command line
cat /proc/PID/cmdline | tr '\0' ' '

# Process status
cat /proc/PID/status
```

## 📊 Process Monitoring Scripts

### Process Health Check Script
```bash
#!/bin/bash
# Process health check script

echo "=== Process Health Report ==="
echo "Date: $(date)"
echo ""

echo "=== Top 5 CPU Consuming Processes ==="
ps aux --sort=-%cpu | head -6
echo ""

echo "=== Top 5 Memory Consuming Processes ==="
ps aux --sort=-%mem | head -6
echo ""

echo "=== Zombie Processes ==="
ps aux | grep -E "(Z|zombie)" | grep -v grep
echo ""

echo "=== Orphaned Processes ==="
ps -eo pid,ppid,cmd | awk '$2 == 1 && $1 != 1'
echo ""

echo "=== Process Count by User ==="
ps -eo user | sort | uniq -c | sort -nr
echo ""

echo "=== Total Processes ==="
ps aux | wc -l
```

### Process Alert Script
```bash
#!/bin/bash
# Process alert script

# Check for high CPU usage
HIGH_CPU=$(ps aux --sort=-%cpu | head -2 | tail -1 | awk '{print $3}')
if [ $(echo "$HIGH_CPU > 80" | bc) -eq 1 ]; then
    echo "High CPU usage detected: $HIGH_CPU%"
    ps aux --sort=-%cpu | head -5
fi

# Check for high memory usage
HIGH_MEM=$(ps aux --sort=-%mem | head -2 | tail -1 | awk '{print $4}')
if [ $(echo "$HIGH_MEM > 80" | bc) -eq 1 ]; then
    echo "High memory usage detected: $HIGH_MEM%"
    ps aux --sort=-%mem | head -5
fi

# Check for zombie processes
ZOMBIE_COUNT=$(ps aux | grep -E "(Z|zombie)" | grep -v grep | wc -l)
if [ $ZOMBIE_COUNT -gt 0 ]; then
    echo "Zombie processes detected: $ZOMBIE_COUNT"
    ps aux | grep -E "(Z|zombie)" | grep -v grep
fi
```

## 🔒 Security Process Monitoring

### Suspicious Process Detection
```bash
# Check for suspicious processes
ps aux | grep -E "(nc|netcat|nmap|masscan|nslookup|dig)"

# Check for processes running as root
ps aux | grep "^root"

# Check for processes with unusual names
ps aux | grep -E "(\.|sh|bash|python|perl)" | grep -v grep

# Check for processes with high privileges
ps aux | grep -E "(sudo|su|passwd|chmod|chown)"
```

### Process Security Analysis
```bash
# Check process file permissions
ls -la /proc/PID/

# Check process environment variables
cat /proc/PID/environ | tr '\0' '\n'

# Check process command line
cat /proc/PID/cmdline | tr '\0' ' '

# Check process working directory
ls -la /proc/PID/cwd

# Check process root directory
ls -la /proc/PID/root
```

## 📈 Process Performance Optimization

### Resource Optimization
```bash
# Identify resource-heavy processes
ps aux --sort=-%cpu | head -10
ps aux --sort=-%mem | head -10
sudo iotop -o

# Optimize process priorities
renice -n 10 PID         # Lower priority for non-critical processes
renice -n -10 PID        # Higher priority for critical processes

# Monitor process resource usage
top -p PID
htop -p PID
```

### Process Tuning
```bash
# Set process limits
ulimit -a                # Show current limits
ulimit -n 4096          # Set file descriptor limit
ulimit -u 100           # Set process limit

# Set process scheduling
chrt -f -p 50 PID       # Set FIFO scheduling
chrt -r -p 50 PID       # Set round-robin scheduling
```

## 🎯 Best Practices

### 1. Regular Monitoring
- Monitor critical processes continuously
- Set up alerts for process failures
- Track resource usage trends

### 2. Process Management
- Use appropriate process priorities
- Implement process limits
- Monitor for zombie processes

### 3. Security
- Monitor for suspicious processes
- Check process permissions
- Audit process activities

### 4. Performance
- Optimize resource usage
- Monitor process performance
- Plan for capacity growth

## 🎯 Summary

Process Monitoring হল Linux administration এর একটি গুরুত্বপূর্ণ অংশ যা:

- ✅ চলমান প্রসেস ট্র্যাক করে
- ✅ রিসোর্স ব্যবহার মনিটর করে
- ✅ পারফরমেন্স বিশ্লেষণ করে
- ✅ সিকিউরিটি নিশ্চিত করে
- ✅ সমস্যা শনাক্ত করে

সঠিক process monitoring tools এবং techniques ব্যবহার করে একটি efficient এবং secure Linux system পরিচালনা করা যায়।
