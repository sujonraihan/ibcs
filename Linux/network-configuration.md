# Network Devices and Configuration

## 🌐 Overview

Network Devices and Configuration is the process of managing network connectivity, routing, and network services in Linux systems. It is an important part of system administration.

## 🎯 Network Configuration Objectives

- **Network Connectivity**: Establish and maintain network connections
- **IP Configuration**: Configure IP addresses, subnets, and routing
- **Network Services**: Manage network services and daemons
- **Security**: Implement network security policies
- **Monitoring**: Monitor network performance and connectivity

## 🔧 Network Interface Types

### 1. Physical Interfaces
- **Ethernet**: Wired network connections
- **WiFi**: Wireless network connections
- **Fiber**: High-speed fiber connections

### 2. Virtual Interfaces
- **Loopback**: Local system communication
- **Bridge**: Virtual network bridges
- **VLAN**: Virtual LAN interfaces
- **Tunnel**: VPN and tunneling interfaces

## 🛠️ Network Configuration Commands

### 1. ip - Modern Network Configuration
```bash
# Show network interfaces
ip addr show
ip link show

# Show routing table
ip route show
ip route list

# Show network statistics
ip -s link show
ip -s addr show

# Configure IP address
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip addr del 192.168.1.100/24 dev eth0

# Configure interface
sudo ip link set eth0 up
sudo ip link set eth0 down
sudo ip link set eth0 mtu 1500

# Configure routing
sudo ip route add 192.168.2.0/24 via 192.168.1.1
sudo ip route del 192.168.2.0/24
sudo ip route add default via 192.168.1.1
```

### 2. ifconfig - Traditional Network Configuration
```bash
# Show network interfaces
ifconfig
ifconfig -a

# Configure IP address
sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0
sudo ifconfig eth0 up
sudo ifconfig eth0 down

# Configure interface
sudo ifconfig eth0 mtu 1500
sudo ifconfig eth0 promisc
```

### 3. route - Routing Configuration
```bash
# Show routing table
route -n
netstat -rn

# Add route
sudo route add -net 192.168.2.0/24 gw 192.168.1.1
sudo route add default gw 192.168.1.1

# Delete route
sudo route del -net 192.168.2.0/24
sudo route del default
```

## 📊 Network Information Commands

### Network Interface Information
```bash
# Show all interfaces
ip addr show
ifconfig -a

# Show specific interface
ip addr show eth0
ifconfig eth0

# Show interface statistics
ip -s link show eth0
cat /proc/net/dev
```

### Routing Information
```bash
# Show routing table
ip route show
route -n
netstat -rn

# Show ARP table
ip neigh show
arp -a

# Show network connections
ss -tuln
netstat -tuln
```

### Network Statistics
```bash
# Network statistics
cat /proc/net/dev
cat /proc/net/snmp
cat /proc/net/netstat

# Interface statistics
ip -s link show
ifconfig -s
```

## 🔧 Network Configuration Files

### /etc/network/interfaces (Debian/Ubuntu)
```bash
# Static IP configuration
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4

# DHCP configuration
auto eth0
iface eth0 inet dhcp

# Bridge configuration
auto br0
iface br0 inet static
    bridge_ports eth0
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
```

### /etc/sysconfig/network-scripts/ (Red Hat/CentOS)
```bash
# Static IP configuration
# /etc/sysconfig/network-scripts/ifcfg-eth0
TYPE=Ethernet
BOOTPROTO=static
NAME=eth0
DEVICE=eth0
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
DNS2=8.8.4.4

# DHCP configuration
TYPE=Ethernet
BOOTPROTO=dhcp
NAME=eth0
DEVICE=eth0
ONBOOT=yes
```

### /etc/resolv.conf - DNS Configuration
```bash
# DNS configuration
nameserver 8.8.8.8
nameserver 8.8.4.4
nameserver 1.1.1.1

# Search domains
search example.com
domain example.com
```

## 🔍 Network Services

### Network Service Management
```bash
# Network service status
sudo systemctl status networking
sudo systemctl status NetworkManager

# Start/stop network services
sudo systemctl start networking
sudo systemctl stop networking
sudo systemctl restart networking

# Enable/disable network services
sudo systemctl enable networking
sudo systemctl disable networking
```

### Network Service Configuration
```bash
# NetworkManager configuration
sudo nmcli connection show
sudo nmcli connection add type ethernet con-name "MyConnection"
sudo nmcli connection modify "MyConnection" ipv4.addresses 192.168.1.100/24
sudo nmcli connection modify "MyConnection" ipv4.gateway 192.168.1.1
sudo nmcli connection modify "MyConnection" ipv4.dns "8.8.8.8,8.8.4.4"
sudo nmcli connection up "MyConnection"
```

## 🔧 Network Troubleshooting

### Connectivity Testing
```bash
# Ping test
ping 8.8.8.8
ping google.com
ping -c 4 8.8.8.8

# Traceroute
traceroute 8.8.8.8
tracepath 8.8.8.8

# DNS resolution
nslookup google.com
dig google.com
host google.com
```

### Network Diagnostics
```bash
# Check network connectivity
ip route get 8.8.8.8
traceroute 8.8.8.8

# Check DNS resolution
nslookup google.com
dig @8.8.8.8 google.com

# Check network interfaces
ip addr show
ip link show
```

### Common Network Issues
```bash
# Interface not up
sudo ip link set eth0 up
sudo ifconfig eth0 up

# No IP address
sudo dhclient eth0
sudo ip addr add 192.168.1.100/24 dev eth0

# No routing
sudo ip route add default via 192.168.1.1
sudo route add default gw 192.168.1.1
```

## 📊 Network Monitoring

### Network Performance
```bash
# Network statistics
cat /proc/net/dev
ip -s link show

# Network traffic
iftop
nethogs
nload

# Network connections
ss -tuln
netstat -tuln
```

### Network Health
```bash
# Interface status
ip link show
ifconfig -a

# Routing table
ip route show
route -n

# ARP table
ip neigh show
arp -a
```

## 🔒 Network Security

### Network Security Configuration
```bash
# Disable IP forwarding
echo 0 > /proc/sys/net/ipv4/ip_forward
sudo sysctl -w net.ipv4.ip_forward=0

# Enable IP forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward
sudo sysctl -w net.ipv4.ip_forward=1

# Configure firewall
sudo ufw enable
sudo iptables -L
```

### Network Access Control
```bash
# Restrict network access
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
sudo iptables -A INPUT -j DROP

# Allow specific services
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

## 🔧 Network Configuration Scripts

### Network Configuration Script
```bash
#!/bin/bash
# Network configuration script

INTERFACE=$1
IP_ADDRESS=$2
NETMASK=$3
GATEWAY=$4
DNS=$5

if [ -z "$INTERFACE" ]; then
    echo "Usage: $0 interface ip_address netmask gateway dns"
    exit 1
fi

# Configure interface
sudo ip addr add $IP_ADDRESS/$NETMASK dev $INTERFACE
sudo ip link set $INTERFACE up

# Configure routing
sudo ip route add default via $GATEWAY

# Configure DNS
echo "nameserver $DNS" | sudo tee /etc/resolv.conf

echo "Network configuration completed"
```

### Network Monitoring Script
```bash
#!/bin/bash
# Network monitoring script

echo "=== Network Status Report ==="
echo "Date: $(date)"
echo ""

echo "=== Network Interfaces ==="
ip addr show
echo ""

echo "=== Routing Table ==="
ip route show
echo ""

echo "=== Network Statistics ==="
ip -s link show
echo ""

echo "=== Active Connections ==="
ss -tuln | head -10
echo ""

echo "=== DNS Resolution ==="
nslookup google.com
```

## 🔍 Network Troubleshooting

### Common Network Issues
```bash
# Interface not found
sudo ip link show
sudo lspci | grep -i network

# No IP address
sudo dhclient eth0
sudo ip addr add 192.168.1.100/24 dev eth0

# No internet connectivity
ping 8.8.8.8
traceroute 8.8.8.8
nslookup google.com
```

### Network Recovery
```bash
# Reset network configuration
sudo systemctl restart networking
sudo systemctl restart NetworkManager

# Reset network interfaces
sudo ip link set eth0 down
sudo ip link set eth0 up

# Clear routing table
sudo ip route flush all
sudo ip route add default via 192.168.1.1
```

## 📈 Network Best Practices

### 1. Network Planning
- Plan network topology
- Use appropriate IP addressing
- Implement network segmentation
- Plan for network growth

### 2. Security
- Implement network security policies
- Use firewall rules
- Monitor network traffic
- Regular security audits

### 3. Performance
- Optimize network settings
- Monitor network performance
- Use appropriate network hardware
- Implement network redundancy

### 4. Maintenance
- Regular network monitoring
- Update network configurations
- Monitor network health
- Document network topology

## 🎯 Summary

Network Devices and Configuration is an important part of Linux system administration that:

- ✅ Ensures network connectivity
- ✅ Manages IP configuration and routing
- ✅ Manages network services
- ✅ Implements network security
- ✅ Monitors network performance

With proper network configuration and management practices, a reliable and secure network infrastructure can be created.
