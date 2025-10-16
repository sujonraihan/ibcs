# Firewalls

## 🔥 Overview

Firewalls হল Linux system এ network security এবং access control নিশ্চিত করার জন্য ব্যবহৃত security tools। এটি unauthorized access prevent করে এবং network traffic control করে।

## 🎯 Firewall Objectives

- **Access Control**: Control incoming and outgoing network traffic
- **Security**: Protect system from unauthorized access
- **Traffic Filtering**: Filter network packets based on rules
- **Network Protection**: Secure network infrastructure
- **Service Protection**: Protect network services

## 🔧 Firewall Types

### 1. iptables - Traditional Firewall
- **Purpose**: Packet filtering and NAT
- **Features**: Stateful inspection, connection tracking
- **Usage**: Advanced firewall configuration

### 2. ufw - Uncomplicated Firewall
- **Purpose**: Simple firewall management
- **Features**: Easy configuration, user-friendly
- **Usage**: Basic firewall setup

### 3. firewalld - Dynamic Firewall
- **Purpose**: Dynamic firewall management
- **Features**: Zone-based configuration, runtime changes
- **Usage**: Modern firewall management

## 🛠️ iptables Firewall

### Basic iptables Commands
```bash
# Show current rules
sudo iptables -L
sudo iptables -L -n
sudo iptables -L -v

# Show rules with line numbers
sudo iptables -L --line-numbers

# Clear all rules
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X
```

### iptables Rule Management
```bash
# Add rules
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Insert rules
sudo iptables -I INPUT 1 -p tcp --dport 22 -j ACCEPT

# Delete rules
sudo iptables -D INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -D INPUT 1

# Replace rules
sudo iptables -R INPUT 1 -p tcp --dport 22 -j ACCEPT
```

### iptables Rule Examples
```bash
# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow specific IP
sudo iptables -A INPUT -s 192.168.1.100 -j ACCEPT

# Block specific IP
sudo iptables -A INPUT -s 192.168.1.200 -j DROP

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Default policy
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

### iptables Advanced Rules
```bash
# Rate limiting
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min --limit-burst 3 -j ACCEPT

# Connection limiting
sudo iptables -A INPUT -p tcp --dport 80 -m connlimit --connlimit-above 10 -j DROP

# Time-based rules
sudo iptables -A INPUT -p tcp --dport 22 -m time --timestart 09:00 --timestop 17:00 -j ACCEPT

# Logging
sudo iptables -A INPUT -p tcp --dport 22 -j LOG --log-prefix "SSH: "
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

## 🔧 ufw Firewall

### Basic ufw Commands
```bash
# Enable ufw
sudo ufw enable

# Disable ufw
sudo ufw disable

# Show status
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered

# Reset ufw
sudo ufw --force reset
```

### ufw Rule Management
```bash
# Allow services
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow from specific IP
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.0/24

# Deny services
sudo ufw deny 23/tcp
sudo ufw deny from 192.168.1.200

# Allow specific ports
sudo ufw allow 3000:3010/tcp
sudo ufw allow 8080/udp
```

### ufw Advanced Configuration
```bash
# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow specific protocols
sudo ufw allow 22/tcp
sudo ufw allow 53/udp
sudo ufw allow 67/udp

# Rate limiting
sudo ufw limit 22/tcp

# Logging
sudo ufw logging on
sudo ufw logging off
```

## 🔧 firewalld Firewall

### Basic firewalld Commands
```bash
# Start firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld

# Stop firewalld
sudo systemctl stop firewalld
sudo systemctl disable firewalld

# Check status
sudo systemctl status firewalld
sudo firewall-cmd --state
```

### firewalld Zone Management
```bash
# List zones
sudo firewall-cmd --get-zones
sudo firewall-cmd --list-all-zones

# Set default zone
sudo firewall-cmd --set-default-zone=public

# Get active zone
sudo firewall-cmd --get-active-zones

# List zone configuration
sudo firewall-cmd --zone=public --list-all
```

### firewalld Service Management
```bash
# List services
sudo firewall-cmd --get-services

# Allow services
sudo firewall-cmd --zone=public --add-service=ssh
sudo firewall-cmd --zone=public --add-service=http
sudo firewall-cmd --zone=public --add-service=https

# Remove services
sudo firewall-cmd --zone=public --remove-service=ssh

# Permanent changes
sudo firewall-cmd --zone=public --add-service=ssh --permanent
sudo firewall-cmd --reload
```

### firewalld Port Management
```bash
# Allow ports
sudo firewall-cmd --zone=public --add-port=22/tcp
sudo firewall-cmd --zone=public --add-port=80/tcp
sudo firewall-cmd --zone=public --add-port=443/tcp

# Remove ports
sudo firewall-cmd --zone=public --remove-port=22/tcp

# Allow port ranges
sudo firewall-cmd --zone=public --add-port=3000-3010/tcp

# Permanent changes
sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
sudo firewall-cmd --reload
```

## 📊 Firewall Monitoring

### Firewall Status
```bash
# iptables status
sudo iptables -L -v
sudo iptables -L -n --line-numbers

# ufw status
sudo ufw status verbose
sudo ufw status numbered

# firewalld status
sudo firewall-cmd --state
sudo firewall-cmd --list-all
```

### Firewall Logs
```bash
# iptables logs
sudo tail -f /var/log/kern.log
sudo tail -f /var/log/syslog

# ufw logs
sudo tail -f /var/log/ufw.log

# firewalld logs
sudo journalctl -u firewalld
sudo tail -f /var/log/firewalld
```

### Network Connections
```bash
# Active connections
ss -tuln
netstat -tuln

# Connection states
ss -tuln | grep LISTEN
netstat -tuln | grep LISTEN

# Established connections
ss -tuln | grep ESTABLISHED
netstat -tuln | grep ESTABLISHED
```

## 🔒 Firewall Security

### Security Best Practices
```bash
# Default deny policy
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow specific services
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

### Advanced Security Rules
```bash
# Rate limiting
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min --limit-burst 3 -j ACCEPT

# Connection limiting
sudo iptables -A INPUT -p tcp --dport 80 -m connlimit --connlimit-above 10 -j DROP

# Block suspicious IPs
sudo iptables -A INPUT -s 192.168.1.200 -j DROP

# Allow specific networks
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
```

## 🔧 Firewall Configuration Scripts

### iptables Configuration Script
```bash
#!/bin/bash
# iptables configuration script

# Clear existing rules
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X

# Set default policies
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow loopback
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allow SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow DNS
sudo iptables -A INPUT -p udp --dport 53 -j ACCEPT

# Allow DHCP
sudo iptables -A INPUT -p udp --dport 67 -j ACCEPT

# Log dropped packets
sudo iptables -A INPUT -j LOG --log-prefix "DROPPED: "

# Save rules
sudo iptables-save > /etc/iptables/rules.v4
```

### ufw Configuration Script
```bash
#!/bin/bash
# ufw configuration script

# Reset ufw
sudo ufw --force reset

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH
sudo ufw allow 22/tcp

# Allow HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Allow DNS
sudo ufw allow 53/udp

# Allow DHCP
sudo ufw allow 67/udp

# Enable ufw
sudo ufw enable

echo "UFW configuration completed"
```

## 🔍 Firewall Troubleshooting

### Common Issues
```bash
# Firewall not working
sudo systemctl status iptables
sudo systemctl status ufw
sudo systemctl status firewalld

# Rules not applied
sudo iptables -L
sudo ufw status
sudo firewall-cmd --list-all

# Connection blocked
sudo iptables -L -v
sudo ufw status verbose
sudo firewall-cmd --list-all
```

### Firewall Recovery
```bash
# Reset iptables
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X

# Reset ufw
sudo ufw --force reset

# Reset firewalld
sudo firewall-cmd --reload
sudo systemctl restart firewalld
```

## 📈 Firewall Best Practices

### 1. Security
- Implement default deny policy
- Allow only necessary services
- Use rate limiting
- Monitor firewall logs

### 2. Configuration
- Document firewall rules
- Use consistent naming
- Test rules before applying
- Regular rule reviews

### 3. Maintenance
- Regular security audits
- Update firewall rules
- Monitor firewall performance
- Backup configurations

### 4. Monitoring
- Monitor firewall logs
- Track blocked connections
- Monitor network traffic
- Regular security assessments

## 🎯 Summary

Firewalls হল Linux system security এর একটি গুরুত্বপূর্ণ অংশ যা:

- ✅ Network access control নিশ্চিত করে
- ✅ Unauthorized access prevent করে
- ✅ Network traffic filter করে
- ✅ System security maintain করে
- ✅ Network services protect করে

সঠিক firewall configuration এবং management practices ব্যবহার করে একটি secure এবং protected network infrastructure তৈরি করা যায়।
