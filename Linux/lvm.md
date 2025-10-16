# Logical Volume Management (LVM)

## 💾 Overview

Logical Volume Management (LVM) is a powerful system for advanced storage management in Linux that provides flexible, scalable, and efficient storage solutions.

## 🎯 LVM Concepts

### LVM Components
- **Physical Volume (PV)**: Physical storage devices (disks, partitions)
- **Volume Group (VG)**: Collection of physical volumes
- **Logical Volume (LV)**: Virtual partitions created from volume groups
- **Physical Extent (PE)**: Smallest unit of allocation in LVM

### LVM Advantages
- **Flexibility**: Resize volumes without downtime
- **Snapshots**: Create point-in-time copies
- **Striping**: Improve performance across multiple disks
- **Mirroring**: Data redundancy and protection
- **Migration**: Move data between physical devices

## 🔧 LVM Components

### 1. Physical Volumes (PV)
```bash
# Create physical volume
sudo pvcreate /dev/sda1
sudo pvcreate /dev/sdb1
sudo pvcreate /dev/sdc1

# Display physical volumes
sudo pvdisplay
sudo pvs
sudo pvscan

# Remove physical volume
sudo pvremove /dev/sda1

# Extend physical volume
sudo pvextend /dev/sda1
```

### 2. Volume Groups (VG)
```bash
# Create volume group
sudo vgcreate myvg /dev/sda1 /dev/sdb1
sudo vgcreate -s 4M myvg /dev/sda1 /dev/sdb1  # 4MB extent size

# Display volume groups
sudo vgdisplay
sudo vgs
sudo vgscan

# Extend volume group
sudo vgextend myvg /dev/sdc1

# Reduce volume group
sudo vgreduce myvg /dev/sdc1

# Remove volume group
sudo vgremove myvg
```

### 3. Logical Volumes (LV)
```bash
# Create logical volume
sudo lvcreate -L 10G -n mylv myvg
sudo lvcreate -l 100%FREE -n mylv myvg  # Use all free space
sudo lvcreate -l 50 -n mylv myvg  # 50 extents

# Display logical volumes
sudo lvdisplay
sudo lvs
sudo lvscan

# Extend logical volume
sudo lvextend -L +5G /dev/myvg/mylv
sudo lvextend -l +100 /dev/myvg/mylv  # Add 100 extents

# Reduce logical volume
sudo lvreduce -L -5G /dev/myvg/mylv
sudo lvreduce -l -100 /dev/myvg/mylv

# Remove logical volume
sudo lvremove /dev/myvg/mylv
```

## 🔧 LVM Operations

### Create LVM Setup
```bash
# Step 1: Create physical volumes
sudo pvcreate /dev/sda1 /dev/sdb1 /dev/sdc1

# Step 2: Create volume group
sudo vgcreate myvg /dev/sda1 /dev/sdb1 /dev/sdc1

# Step 3: Create logical volume
sudo lvcreate -L 20G -n mylv myvg

# Step 4: Format logical volume
sudo mkfs.ext4 /dev/myvg/mylv

# Step 5: Mount logical volume
sudo mkdir /mnt/lvm
sudo mount /dev/myvg/mylv /mnt/lvm
```

### Extend LVM
```bash
# Extend logical volume
sudo lvextend -L +10G /dev/myvg/mylv

# Resize filesystem
sudo resize2fs /dev/myvg/mylv

# Check new size
df -h /mnt/lvm
```

### Reduce LVM
```bash
# Unmount logical volume
sudo umount /mnt/lvm

# Check filesystem
sudo fsck /dev/myvg/mylv

# Resize filesystem
sudo resize2fs /dev/myvg/mylv 10G

# Reduce logical volume
sudo lvreduce -L 10G /dev/myvg/mylv

# Remount logical volume
sudo mount /dev/myvg/mylv /mnt/lvm
```

## 📊 LVM Information

### Display LVM Information
```bash
# Physical volume information
sudo pvdisplay
sudo pvs
sudo pvscan

# Volume group information
sudo vgdisplay
sudo vgs
sudo vgscan

# Logical volume information
sudo lvdisplay
sudo lvs
sudo lvscan

# LVM configuration
sudo vgcfgbackup
sudo vgcfgrestore
```

### LVM Statistics
```bash
# LVM status
sudo lvm lvs
sudo lvm vgs
sudo lvm pvs

# LVM metadata
sudo vgcfgbackup
sudo vgcfgrestore

# LVM logs
sudo journalctl -u lvm2-monitor
sudo tail -f /var/log/lvm.log
```

## 🔍 LVM Advanced Features

### 1. LVM Snapshots
```bash
# Create snapshot
sudo lvcreate -L 5G -s -n mysnap /dev/myvg/mylv

# Display snapshots
sudo lvdisplay /dev/myvg/mysnap

# Mount snapshot
sudo mkdir /mnt/snapshot
sudo mount /dev/myvg/mysnap /mnt/snapshot

# Remove snapshot
sudo lvremove /dev/myvg/mysnap
```

### 2. LVM Striping
```bash
# Create striped logical volume
sudo lvcreate -L 20G -i 2 -I 64k -n mystripe myvg

# Display striping information
sudo lvdisplay /dev/myvg/mystripe

# Check striping
sudo lvs -o +stripes,stripesize /dev/myvg/mystripe
```

### 3. LVM Mirroring
```bash
# Create mirrored logical volume
sudo lvcreate -L 20G -m 1 -n mymirror myvg

# Display mirror information
sudo lvdisplay /dev/myvg/mymirror

# Check mirror status
sudo lvs -o +mirrors /dev/myvg/mymirror
```

## 🔧 LVM Management

### LVM Configuration
```bash
# LVM configuration file
sudo nano /etc/lvm/lvm.conf

# LVM tags
sudo lvchange --addtag backup /dev/myvg/mylv
sudo lvchange --deltag backup /dev/myvg/mylv

# LVM activation
sudo lvchange -ay /dev/myvg/mylv
sudo lvchange -an /dev/myvg/mylv
```

### LVM Backup
```bash
# Backup LVM metadata
sudo vgcfgbackup
sudo vgcfgbackup -f /backup/vg_backup

# Restore LVM metadata
sudo vgcfgrestore -f /backup/vg_backup

# Backup logical volume
sudo dd if=/dev/myvg/mylv of=/backup/lv_backup.img bs=4M
```

### LVM Migration
```bash
# Move logical volume
sudo pvmove /dev/sda1 /dev/sdb1

# Move specific logical volume
sudo pvmove -n mylv /dev/sda1 /dev/sdb1

# Check migration status
sudo pvmove --abort
```

## 🔒 LVM Security

### LVM Encryption
```bash
# Create encrypted logical volume
sudo lvcreate -L 20G -n myencrypted myvg
sudo cryptsetup luksFormat /dev/myvg/myencrypted
sudo cryptsetup luksOpen /dev/myvg/myencrypted encrypted
sudo mkfs.ext4 /dev/mapper/encrypted
sudo mount /dev/mapper/encrypted /mnt/encrypted
```

### LVM Permissions
```bash
# Set LVM permissions
sudo chmod 600 /dev/myvg/mylv
sudo chown root:root /dev/myvg/mylv

# LVM access control
sudo nano /etc/lvm/lvm.conf
# Set access control rules
```

## 📈 LVM Performance

### LVM Performance Tuning
```bash
# Set LVM cache
sudo lvcreate -L 20G -n mycached myvg
sudo lvconvert --type cache-pool /dev/myvg/mycached
sudo lvconvert --type cache /dev/myvg/mylv /dev/myvg/mycached

# LVM I/O optimization
sudo lvchange --cachemode writethrough /dev/myvg/mylv
sudo lvchange --cachemode writeback /dev/myvg/mylv
```

### LVM Monitoring
```bash
# LVM status
sudo lvm lvs
sudo lvm vgs
sudo lvm pvs

# LVM performance
sudo iostat -x 1 5
sudo iotop

# LVM health
sudo lvm lvs -o +health_status
```

## 🔍 LVM Troubleshooting

### Common Issues
```bash
# LVM not recognized
sudo vgscan
sudo pvscan
sudo lvscan

# LVM activation issues
sudo lvchange -ay /dev/myvg/mylv
sudo vgchange -ay myvg

# LVM metadata corruption
sudo vgcfgrestore -f /backup/vg_backup
sudo vgcfgbackup
```

### LVM Recovery
```bash
# Recover LVM metadata
sudo vgcfgrestore -f /backup/vg_backup

# Recover logical volume
sudo lvchange -ay /dev/myvg/mylv

# Check LVM status
sudo lvm lvs
sudo lvm vgs
sudo lvm pvs
```

## 📊 LVM Best Practices

### 1. Planning
- Plan LVM layout before implementation
- Consider future growth requirements
- Use appropriate extent sizes
- Plan for backup and recovery

### 2. Performance
- Use appropriate striping for performance
- Monitor LVM performance
- Optimize I/O settings
- Use LVM cache for frequently accessed data

### 3. Security
- Implement LVM encryption for sensitive data
- Set proper permissions
- Regular backup of LVM metadata
- Monitor LVM health

### 4. Maintenance
- Regular LVM health checks
- Monitor disk usage
- Plan for capacity growth
- Document LVM configuration

## 🎯 LVM Commands Summary

### Physical Volume Commands
```bash
pvcreate /dev/sda1          # Create physical volume
pvdisplay                   # Display physical volumes
pvs                         # List physical volumes
pvscan                      # Scan for physical volumes
pvextend /dev/sda1          # Extend physical volume
pvremove /dev/sda1          # Remove physical volume
```

### Volume Group Commands
```bash
vgcreate myvg /dev/sda1     # Create volume group
vgdisplay                   # Display volume groups
vgs                         # List volume groups
vgscan                      # Scan for volume groups
vgextend myvg /dev/sdb1     # Extend volume group
vgreduce myvg /dev/sdb1     # Reduce volume group
vgremove myvg               # Remove volume group
```

### Logical Volume Commands
```bash
lvcreate -L 10G -n mylv myvg  # Create logical volume
lvdisplay                      # Display logical volumes
lvs                            # List logical volumes
lvscan                         # Scan for logical volumes
lvextend -L +5G /dev/myvg/mylv # Extend logical volume
lvreduce -L -5G /dev/myvg/mylv # Reduce logical volume
lvremove /dev/myvg/mylv        # Remove logical volume
```

## 🎯 Summary

Logical Volume Management (LVM) is a powerful tool for Linux storage management that:

- ✅ Provides flexible storage management
- ✅ Provides dynamic resizing capabilities
- ✅ Provides advanced features like snapshots, striping, mirroring
- ✅ Optimizes performance
- ✅ Simplifies data protection and backup

Using LVM, a scalable, flexible, and efficient storage solution can be created that is ideal for modern Linux systems.
