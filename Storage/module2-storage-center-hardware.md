# Module 2 - Storage Center Hardware

## Table of Contents
1. [Controller Components](#controller-components)
2. [Enclosure Components](#enclosure-components)
3. [Communication Links](#communication-links)

---

## Controller Components

### Storage Controller Overview
Storage Controller হল Storage Center এর brain যা সব data processing এবং management operations handle করে।

### Controller Types:

#### 1. SC4020 Controller
**বৈশিষ্ট্য:**
- Entry-level controller
- 2U form factor
- Dual controller support
- Up to 12 drives per controller

**Specifications:**
- CPU: Intel Xeon processor
- Memory: 16GB-64GB RAM
- Cache: Up to 1TB
- Ports: 4x 10GbE, 4x 1GbE

#### 2. SC5020 Controller
**বৈশিষ্ট্য:**
- Mid-range controller
- 2U form factor
- Enhanced performance
- Up to 24 drives per controller

**Specifications:**
- CPU: Intel Xeon processor
- Memory: 32GB-128GB RAM
- Cache: Up to 2TB
- Ports: 8x 10GbE, 4x 1GbE

#### 3. SC7020 Controller
**বৈশিষ্ট্য:**
- High-end controller
- 2U form factor
- Maximum performance
- Up to 48 drives per controller

**Specifications:**
- CPU: Intel Xeon processor
- Memory: 64GB-256GB RAM
- Cache: Up to 4TB
- Ports: 16x 10GbE, 8x 1GbE

### Controller Architecture:

#### CPU Subsystem
- **Primary CPU**: Main processing unit
- **Secondary CPU**: Backup processing
- **Cache Controller**: Memory management
- **I/O Processor**: Data transfer handling

#### Memory Subsystem
- **System Memory**: Operating system
- **Cache Memory**: Data caching
- **Metadata Memory**: Storage metadata
- **Buffer Memory**: I/O buffering

#### I/O Subsystem
- **Front-end Ports**: Server connectivity
- **Back-end Ports**: Drive connectivity
- **Management Ports**: Administrative access
- **Service Ports**: Maintenance access

### Controller Redundancy:

#### Active-Passive Configuration
- Primary controller active
- Secondary controller standby
- Automatic failover
- Data consistency maintained

#### Active-Active Configuration
- Both controllers active
- Load balancing
- Higher performance
- Complex configuration

---

## Enclosure Components

### Storage Enclosure Types:

#### 1. SC200 Enclosure
**বৈশিষ্ট্য:**
- 2U form factor
- 12 drive bays
- SAS connectivity
- Hot-swappable drives

**Drive Support:**
- 2.5" SAS drives
- 2.5" SATA drives
- SSD support
- Up to 12TB per drive

#### 2. SC220 Enclosure
**বৈশিষ্ট্য:**
- 2U form factor
- 24 drive bays
- SAS connectivity
- High density

**Drive Support:**
- 2.5" SAS drives
- 2.5" SATA drives
- SSD support
- Up to 12TB per drive

#### 3. SC280 Enclosure
**বৈশিষ্ট্য:**
- 2U form factor
- 24 drive bays
- SAS connectivity
- Enterprise features

**Drive Support:**
- 2.5" SAS drives
- 2.5" SATA drives
- SSD support
- Up to 12TB per drive

### Enclosure Architecture:

#### Power Supply Units (PSUs)
- **Redundant PSUs**: N+1 configuration
- **Hot-swappable**: Online replacement
- **Power monitoring**: Real-time status
- **Efficiency**: 80+ Gold certified

#### Cooling System
- **Redundant fans**: N+1 configuration
- **Variable speed**: Temperature controlled
- **Hot-swappable**: Online replacement
- **Noise reduction**: Quiet operation

#### Drive Bays
- **Hot-swappable**: Online replacement
- **LED indicators**: Status indication
- **Locking mechanism**: Secure mounting
- **Cable management**: Neat organization

### Drive Types and Specifications:

#### SAS Drives
- **Interface**: Serial Attached SCSI
- **Speed**: 12 Gbps
- **Capacity**: 300GB - 12TB
- **Use case**: High performance

#### SATA Drives
- **Interface**: Serial ATA
- **Speed**: 6 Gbps
- **Capacity**: 500GB - 12TB
- **Use case**: Cost-effective storage

#### SSD Drives
- **Interface**: SAS/SATA
- **Speed**: Up to 12 Gbps
- **Capacity**: 100GB - 3.84TB
- **Use case**: High performance, low latency

### Drive Tiering:

#### Tier 1 (Performance)
- SSD drives
- High cost per GB
- Low latency
- High IOPS

#### Tier 2 (Capacity)
- SAS drives
- Medium cost per GB
- Medium latency
- Medium IOPS

#### Tier 3 (Archive)
- SATA drives
- Low cost per GB
- High latency
- Low IOPS

---

## Communication Links

### Front-end Connectivity:

#### Ethernet Ports
- **10 Gigabit Ethernet**: High performance
- **1 Gigabit Ethernet**: Management
- **Port bonding**: Load balancing
- **VLAN support**: Network segmentation

#### Fibre Channel Ports
- **8 Gbps FC**: Legacy support
- **16 Gbps FC**: Current standard
- **32 Gbps FC**: High performance
- **Port trunking**: Bandwidth aggregation

#### iSCSI Connectivity
- **TCP/IP based**: Network storage
- **CHAP authentication**: Security
- **MPIO support**: Multipathing
- **Jumbo frames**: Performance optimization

### Back-end Connectivity:

#### SAS Connectivity
- **12 Gbps SAS**: Drive connection
- **SAS expanders**: Port multiplication
- **Redundant paths**: High availability
- **Cable management**: Organization

#### SATA Connectivity
- **6 Gbps SATA**: Drive connection
- **SAS to SATA**: Compatibility
- **Port multipliers**: Drive expansion
- **Hot-swap support**: Online replacement

### Network Topologies:

#### Direct Connection
- **Point-to-point**: Direct connection
- **Low latency**: Minimal overhead
- **Limited scalability**: Few connections
- **Simple management**: Easy configuration

#### Switched Network
- **Fabric topology**: Multiple paths
- **High scalability**: Many connections
- **Complex management**: Advanced configuration
- **Load balancing**: Traffic distribution

#### Hybrid Network
- **Mixed topologies**: Best of both
- **Flexible configuration**: Various options
- **Complex setup**: Advanced planning
- **Optimal performance**: Balanced approach

### Communication Protocols:

#### SCSI Protocol
- **Command set**: Standard commands
- **Error handling**: Robust error recovery
- **Queue management**: Command queuing
- **Status reporting**: Detailed feedback

#### ATA Protocol
- **Command set**: ATA commands
- **Error handling**: Basic error recovery
- **Queue management**: Simple queuing
- **Status reporting**: Basic feedback

#### NVMe Protocol
- **Command set**: NVMe commands
- **Error handling**: Advanced error recovery
- **Queue management**: Multiple queues
- **Status reporting**: Detailed feedback

---

## Hardware Monitoring and Management

### Environmental Monitoring:
- **Temperature sensors**: Real-time monitoring
- **Humidity sensors**: Environmental control
- **Power monitoring**: Consumption tracking
- **Fan speed monitoring**: Cooling status

### Health Monitoring:
- **Drive health**: SMART data
- **Controller health**: System status
- **Network health**: Connectivity status
- **Performance health**: Metrics tracking

### Alert Management:
- **Threshold-based**: Configurable limits
- **Event-driven**: Real-time alerts
- **Escalation**: Multiple levels
- **Integration**: Third-party tools

---

## Best Practices

### 1. Hardware Selection
- Choose appropriate controller for workload
- Select right drive types for performance needs
- Plan for future growth and expansion
- Consider redundancy requirements

### 2. Installation
- Follow vendor guidelines
- Ensure proper cooling
- Plan cable management
- Document configurations

### 3. Maintenance
- Regular health checks
- Proactive replacement
- Performance monitoring
- Capacity planning

### 4. Troubleshooting
- Use diagnostic tools
- Check environmental conditions
- Verify connectivity
- Review performance metrics

---

## Summary

Storage Center Hardware হল একটি comprehensive solution যা বিভিন্ন performance এবং capacity requirements meet করতে পারে। Proper hardware selection এবং configuration এর মাধ্যমে optimal performance achieve করা যায়।

**Key Takeaways:**
- Choose appropriate controller based on requirements
- Select right drive types for performance needs
- Plan for redundancy and high availability
- Implement proper monitoring and maintenance procedures
