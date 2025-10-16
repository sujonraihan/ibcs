# Disk Partitioning

## 💾 Overview

Disk Partitioning হল storage device কে logical sections এ ভাগ করার প্রক্রিয়া। এটি data organization, security, এবং system management এর জন্য অত্যন্ত গুরুত্বপূর্ণ।

## 🎯 Partitioning Objectives

- **Data Organization**: Separate different types of data
- **Security**: Isolate sensitive data
- **Performance**: Optimize disk performance
- **Backup**: Facilitate backup operations
- **System Management**: Simplify system administration

## 🔧 Partition Types

### 1. Primary Partitions
- **Maximum**: 4 primary partitions per disk
- **Purpose**: Bootable partitions, main data storage
- **Limitation**: MBR (Master Boot Record) limitation

### 2. Extended Partitions
- **Purpose**: Container for logical partitions
- **Maximum**: 1 extended partition per disk
- **Usage**: When more than 4 partitions needed

### 3. Logical Partitions
- **Purpose**: Partitions inside extended partition
- **Maximum**: Unlimited (practical limit)
- **Usage**: Additional data storage

## 🛠️ Partitioning Tools

### 1. fdisk - Traditional Partitioning Tool
```bash
# Start fdisk
sudo fdisk /dev/sda

# fdisk commands
# n - Create new partition
# d - Delete partition
# p - Print partition table
# w - Write changes and exit
# q - Quit without saving
# t - Change partition type
# l - List partition types

# Create partition
sudo fdisk /dev/sda
# n (new partition)
# p (primary partition)
# 1 (partition number)
# +10G (size)
# w (write and exit)
```

### 2. parted - Advanced Partitioning Tool
```bash
# Start parted
sudo parted /dev/sda

# parted commands
# print - Show partition table
# mkpart - Create partition
# rm - Remove partition
# resizepart - Resize partition
# set - Set partition flags
# quit - Exit parted

# Create partition with parted
sudo parted /dev/sda
# mkpart primary ext4 0% 50%
# mkpart primary ext4 50% 100%
# print
# quit
```

### 3. gdisk - GPT Partitioning Tool
```bash
# Start gdisk
sudo gdisk /dev/sda

# gdisk commands
# n - Create new partition
# d - Delete partition
# p - Print partition table
# w - Write changes and exit
# q - Quit without saving
# t - Change partition type
# l - List partition types

# Create GPT partition
sudo gdisk /dev/sda
# n (new partition)
# 1 (partition number)
# +10G (size)
# 8300 (Linux filesystem type)
# w (write and exit)
```

## 📊 Partition Table Types

### 1. MBR (Master Boot Record)
```bash
# Check MBR
sudo fdisk -l /dev/sda
sudo parted /dev/sda print

# MBR limitations
# - Maximum 4 primary partitions
# - Maximum 2TB disk size
# - 32-bit partition table
```

### 2. GPT (GUID Partition Table)
```bash
# Check GPT
sudo gdisk -l /dev/sda
sudo parted /dev/sda print

# GPT advantages
# - Unlimited partitions
# - Maximum 9.4ZB disk size
# - 64-bit partition table
# - Better error detection
```

## 🔧 Partitioning Operations

### Create Partitions
```bash
# Using fdisk
sudo fdisk /dev/sda
# n (new partition)
# p (primary partition)
# 1 (partition number)
# +10G (size)
# w (write and exit)

# Using parted
sudo parted /dev/sda
# mkpart primary ext4 0% 50%
# mkpart primary ext4 50% 100%
# quit

# Using gdisk
sudo gdisk /dev/sda
# n (new partition)
# 1 (partition number)
# +10G (size)
# 8300 (Linux filesystem type)
# w (write and exit)
```

### Delete Partitions
```bash
# Using fdisk
sudo fdisk /dev/sda
# d (delete partition)
# 1 (partition number)
# w (write and exit)

# Using parted
sudo parted /dev/sda
# rm 1 (remove partition 1)
# quit

# Using gdisk
sudo gdisk /dev/sda
# d (delete partition)
# 1 (partition number)
# w (write and exit)
```

### Resize Partitions
```bash
# Using parted
sudo parted /dev/sda
# resizepart 1 20G (resize partition 1 to 20GB)
# quit

# Using gparted (GUI tool)
sudo gparted
```

## 📈 Partition Information

### View Partition Table
```bash
# List all partitions
sudo fdisk -l
sudo parted -l
sudo gdisk -l /dev/sda

# Show specific disk
sudo fdisk -l /dev/sda
sudo parted /dev/sda print
sudo gdisk -l /dev/sda

# Show partition details
lsblk
lsblk -f
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT
```

### Partition Information
```bash
# Partition size
sudo fdisk -l /dev/sda1
sudo parted /dev/sda print

# Partition type
sudo fdisk -l /dev/sda
sudo gdisk -l /dev/sda

# Partition usage
df -h
df -T
df -i
```

## 🔍 Partition Management

### Format Partitions
```bash
# Format as ext4
sudo mkfs.ext4 /dev/sda1
sudo mkfs.ext4 -L "MyData" /dev/sda1

# Format as XFS
sudo mkfs.xfs /dev/sda1
sudo mkfs.xfs -L "MyData" /dev/sda1

# Format as Btrfs
sudo mkfs.btrfs /dev/sda1
sudo mkfs.btrfs -L "MyData" /dev/sda1
```

### Mount Partitions
```bash
# Create mount point
sudo mkdir /mnt/data

# Mount partition
sudo mount /dev/sda1 /mnt/data

# Mount with options
sudo mount -o rw,noatime /dev/sda1 /mnt/data

# Unmount partition
sudo umount /mnt/data
```

### Auto-mount Partitions
```bash
# Edit fstab
sudo nano /etc/fstab

# Add entry
/dev/sda1 /mnt/data ext4 defaults 0 2

# Test fstab
sudo mount -a

# Check mounts
mount | grep sda1
```

## 🔧 Advanced Partitioning

### Logical Volume Management (LVM)
```bash
# Create physical volume
sudo pvcreate /dev/sda1
sudo pvcreate /dev/sdb1

# Create volume group
sudo vgcreate myvg /dev/sda1 /dev/sdb1

# Create logical volume
sudo lvcreate -L 10G -n mylv myvg

# Format logical volume
sudo mkfs.ext4 /dev/myvg/mylv

# Mount logical volume
sudo mount /dev/myvg/mylv /mnt/data
```

### RAID Partitioning
```bash
# Create RAID array
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1

# Format RAID array
sudo mkfs.ext4 /dev/md0

# Mount RAID array
sudo mount /dev/md0 /mnt/raid
```

## 📊 Partition Monitoring

### Disk Usage
```bash
# Disk usage by partition
df -h
df -T
df -i

# Directory usage
du -h /path/to/directory
du -sh /path/to/directory
du -h --max-depth=1 /path

# Inode usage
df -i
find /path -type f | wc -l
```

### Partition Health
```bash
# Check partition
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
sudo xfs_repair /dev/sda1

# Disk health
sudo smartctl -a /dev/sda
sudo badblocks -v /dev/sda1
```

## 🔒 Security Considerations

### Partition Security
```bash
# Set partition permissions
sudo chmod 755 /mnt/data
sudo chown root:root /mnt/data

# Encrypt partition
sudo cryptsetup luksFormat /dev/sda1
sudo cryptsetup luksOpen /dev/sda1 encrypted
sudo mkfs.ext4 /dev/mapper/encrypted
sudo mount /dev/mapper/encrypted /mnt/encrypted
```

### Backup Partitions
```bash
# Backup partition
sudo dd if=/dev/sda1 of=/backup/partition.img bs=4M

# Restore partition
sudo dd if=/backup/partition.img of=/dev/sda1 bs=4M

# Clone partition
sudo dd if=/dev/sda1 of=/dev/sdb1 bs=4M
```

## 🔍 Troubleshooting

### Common Issues
```bash
# Partition not recognized
sudo partprobe /dev/sda
sudo kpartx -a /dev/sda

# Mount errors
dmesg | grep -i mount
journalctl -k | grep -i mount

# Filesystem errors
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
```

### Recovery Operations
```bash
# Recover partition table
sudo testdisk
sudo gpart

# Recover deleted partition
sudo fdisk /dev/sda
# n (new partition)
# p (primary partition)
# 1 (partition number)
# +10G (size)
# w (write and exit)

# Force filesystem check
sudo fsck -f /dev/sda1
```

## 📈 Performance Optimization

### Partition Alignment
```bash
# Check partition alignment
sudo parted /dev/sda print
sudo fdisk -l /dev/sda

# Align partitions
sudo parted /dev/sda
# mklabel gpt
# mkpart primary ext4 0% 50%
# mkpart primary ext4 50% 100%
# quit
```

### I/O Optimization
```bash
# Set I/O scheduler
echo noop > /sys/block/sda/queue/scheduler
echo deadline > /sys/block/sda/queue/scheduler
echo cfq > /sys/block/sda/queue/scheduler

# Set read-ahead
echo 256 > /sys/block/sda/queue/read_ahead_kb
```

## 🎯 Best Practices

### 1. Partition Planning
- Plan partition layout before installation
- Consider future growth requirements
- Separate system and data partitions
- Use appropriate filesystem types

### 2. Security
- Encrypt sensitive partitions
- Set proper permissions
- Regular backup of partition table
- Monitor partition health

### 3. Performance
- Align partitions properly
- Use appropriate filesystem types
- Optimize I/O settings
- Monitor disk usage

### 4. Maintenance
- Regular filesystem checks
- Monitor disk health
- Plan for capacity growth
- Document partition layout

## 🎯 Summary

Disk Partitioning হল Linux system administration এর একটি গুরুত্বপূর্ণ অংশ যা:

- ✅ Data organization এবং management করে
- ✅ Security এবং isolation প্রদান করে
- ✅ Performance optimization করে
- ✅ Backup এবং recovery সহজ করে
- ✅ System administration সহজ করে

সঠিক partitioning strategy এবং tools ব্যবহার করে একটি efficient এবং reliable Linux system পরিচালনা করা যায়।
