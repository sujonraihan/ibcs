# Linux Filesystems and the VFS

## 💾 Overview

Linux Filesystems এবং Virtual File System (VFS) হল Linux operating system এর একটি গুরুত্বপূর্ণ অংশ যা file operations, storage management, এবং system performance নিয়ন্ত্রণ করে।

## 🎯 Filesystem Concepts

### What is a Filesystem?
- **Definition**: A filesystem is a method of storing and organizing files on storage devices
- **Purpose**: Provides a logical structure for data storage and retrieval
- **Components**: Directories, files, metadata, and access control

### Filesystem Types
- **Local Filesystems**: ext4, XFS, Btrfs, ZFS
- **Network Filesystems**: NFS, CIFS, SMB
- **Special Filesystems**: proc, sysfs, tmpfs, devtmpfs
- **Pseudo Filesystems**: Virtual filesystems for system information

## 🔧 Linux Filesystem Types

### 1. ext4 (Fourth Extended Filesystem)
```bash
# Check filesystem type
df -T
lsblk -f
file -s /dev/sda1

# Create ext4 filesystem
sudo mkfs.ext4 /dev/sda1
sudo mkfs.ext4 -L "MyData" /dev/sda1

# Mount ext4 filesystem
sudo mount /dev/sda1 /mnt/data
sudo umount /mnt/data

# Check filesystem
sudo fsck.ext4 /dev/sda1
sudo e2fsck -f /dev/sda1
```

### 2. XFS (High Performance Filesystem)
```bash
# Create XFS filesystem
sudo mkfs.xfs /dev/sda1
sudo mkfs.xfs -L "MyData" /dev/sda1

# Mount XFS filesystem
sudo mount /dev/sda1 /mnt/data

# XFS maintenance
sudo xfs_repair /dev/sda1
sudo xfs_fsr /mnt/data
```

### 3. Btrfs (B-tree Filesystem)
```bash
# Create Btrfs filesystem
sudo mkfs.btrfs /dev/sda1
sudo mkfs.btrfs -L "MyData" /dev/sda1

# Mount Btrfs filesystem
sudo mount /dev/sda1 /mnt/data

# Btrfs features
sudo btrfs subvolume create /mnt/data/snapshots
sudo btrfs subvolume snapshot /mnt/data /mnt/data/snapshots/backup
sudo btrfs balance start /mnt/data
```

### 4. ZFS (Zettabyte Filesystem)
```bash
# Install ZFS
sudo apt install zfsutils-linux    # Ubuntu/Debian
sudo yum install zfs               # CentOS/RHEL

# Create ZFS pool
sudo zpool create mypool /dev/sda1
sudo zpool create -f mypool mirror /dev/sda1 /dev/sdb1

# ZFS operations
sudo zfs create mypool/data
sudo zfs snapshot mypool/data@backup
sudo zfs clone mypool/data@backup mypool/restore
```

## 🔍 Virtual File System (VFS)

### VFS Concepts
- **Purpose**: Provides a uniform interface for different filesystems
- **Abstraction**: Hides filesystem-specific details from applications
- **Layers**: System calls → VFS → Filesystem drivers → Storage devices

### VFS Components
```bash
# VFS information
cat /proc/filesystems
cat /proc/mounts
cat /proc/self/mountinfo

# Filesystem statistics
cat /proc/stat
cat /proc/diskstats
```

### VFS Operations
```bash
# File operations
open()     # Open file
read()     # Read from file
write()    # Write to file
close()    # Close file
lseek()    # Seek in file

# Directory operations
opendir()  # Open directory
readdir()  # Read directory entries
closedir() # Close directory

# Filesystem operations
mount()    # Mount filesystem
umount()   # Unmount filesystem
statfs()   # Get filesystem statistics
```

## 📊 Filesystem Information

### Filesystem Statistics
```bash
# Disk usage
df -h
df -T
df -i                    # Inode usage

# Directory usage
du -h /path/to/directory
du -sh /path/to/directory
du -h --max-depth=1 /path

# Filesystem information
lsblk
lsblk -f
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT
```

### Mount Information
```bash
# Mounted filesystems
mount
cat /proc/mounts
cat /etc/fstab

# Mount options
mount -o remount,rw /dev/sda1
mount -o remount,ro /dev/sda1
mount -o noatime /dev/sda1 /mnt/data
```

### Filesystem Health
```bash
# Check filesystem
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
sudo xfs_repair /dev/sda1

# Filesystem errors
dmesg | grep -i error
journalctl -k | grep -i error
```

## 🔧 Filesystem Operations

### Mounting Filesystems
```bash
# Basic mount
sudo mount /dev/sda1 /mnt/data
sudo umount /mnt/data

# Mount with options
sudo mount -o rw,noatime /dev/sda1 /mnt/data
sudo mount -o remount,ro /dev/sda1

# Mount network filesystems
sudo mount -t nfs server:/path /mnt/nfs
sudo mount -t cifs //server/share /mnt/cifs -o username=user

# Mount ISO files
sudo mount -o loop image.iso /mnt/iso
```

### Filesystem Creation
```bash
# Create filesystem
sudo mkfs.ext4 /dev/sda1
sudo mkfs.xfs /dev/sda1
sudo mkfs.btrfs /dev/sda1

# Create with label
sudo mkfs.ext4 -L "MyData" /dev/sda1
sudo mkfs.xfs -L "MyData" /dev/sda1

# Create with specific options
sudo mkfs.ext4 -b 4096 -i 4096 /dev/sda1
sudo mkfs.xfs -d agcount=4 /dev/sda1
```

### Filesystem Maintenance
```bash
# Check filesystem
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
sudo xfs_repair /dev/sda1

# Defragment filesystem
sudo e4defrag /dev/sda1
sudo xfs_fsr /mnt/data

# Resize filesystem
sudo resize2fs /dev/sda1
sudo xfs_growfs /mnt/data
```

## 📈 Performance Optimization

### Filesystem Performance
```bash
# Mount options for performance
sudo mount -o noatime,nodiratime /dev/sda1 /mnt/data
sudo mount -o remount,noatime /dev/sda1

# I/O scheduler
echo noop > /sys/block/sda/queue/scheduler
echo deadline > /sys/block/sda/queue/scheduler
echo cfq > /sys/block/sda/queue/scheduler

# Read-ahead
echo 256 > /sys/block/sda/queue/read_ahead_kb
```

### Filesystem Tuning
```bash
# ext4 tuning
sudo tune2fs -o journal_data_writeback /dev/sda1
sudo tune2fs -O ^has_journal /dev/sda1

# XFS tuning
sudo mount -o noatime,nodiratime /dev/sda1 /mnt/data
sudo xfs_admin -l /dev/sda1
```

## 🔒 Security Considerations

### Filesystem Security
```bash
# File permissions
chmod 755 /path/to/file
chmod 644 /path/to/file
chmod 600 /path/to/file

# Directory permissions
chmod 755 /path/to/directory
chmod 700 /path/to/directory

# Ownership
chown user:group /path/to/file
chown -R user:group /path/to/directory
```

### Filesystem Encryption
```bash
# LUKS encryption
sudo cryptsetup luksFormat /dev/sda1
sudo cryptsetup luksOpen /dev/sda1 encrypted
sudo mkfs.ext4 /dev/mapper/encrypted
sudo mount /dev/mapper/encrypted /mnt/encrypted

# LUKS operations
sudo cryptsetup luksClose encrypted
sudo cryptsetup luksAddKey /dev/sda1
sudo cryptsetup luksRemoveKey /dev/sda1
```

## 🔍 Troubleshooting

### Common Issues
```bash
# Filesystem errors
sudo fsck /dev/sda1
sudo e2fsck -f /dev/sda1
sudo xfs_repair /dev/sda1

# Mount errors
dmesg | grep -i mount
journalctl -k | grep -i mount

# Permission errors
ls -la /path/to/file
stat /path/to/file
```

### Recovery Operations
```bash
# Force filesystem check
sudo fsck -f /dev/sda1
sudo e2fsck -f /dev/sda1

# Recover deleted files
sudo extundelete /dev/sda1 --restore-file /path/to/file
sudo extundelete /dev/sda1 --restore-directory /path/to/directory

# Backup and restore
sudo dd if=/dev/sda1 of=/backup/disk.img bs=4M
sudo dd if=/backup/disk.img of=/dev/sda1 bs=4M
```

## 📊 Filesystem Monitoring

### Performance Monitoring
```bash
# I/O statistics
iostat -x 1 5
iotop
sudo iotop -o

# Filesystem usage
df -h
du -h /path/to/directory

# Inode usage
df -i
find /path -type f | wc -l
```

### Health Monitoring
```bash
# Filesystem health
sudo fsck -n /dev/sda1
sudo e2fsck -n /dev/sda1

# Disk health
sudo smartctl -a /dev/sda
sudo badblocks -v /dev/sda1
```

## 🎯 Best Practices

### 1. Filesystem Selection
- **ext4**: General purpose, stable
- **XFS**: High performance, large files
- **Btrfs**: Advanced features, snapshots
- **ZFS**: Enterprise features, data integrity

### 2. Mount Options
- Use appropriate mount options for performance
- Consider security requirements
- Plan for backup and recovery

### 3. Maintenance
- Regular filesystem checks
- Monitor disk usage
- Plan for capacity growth

### 4. Security
- Implement proper file permissions
- Use encryption for sensitive data
- Regular security audits

## 🎯 Summary

Linux Filesystems এবং VFS হল Linux system এর একটি গুরুত্বপূর্ণ অংশ যা:

- ✅ Data storage এবং retrieval নিশ্চিত করে
- ✅ Performance optimization করে
- ✅ Security এবং access control প্রদান করে
- ✅ System stability নিশ্চিত করে
- ✅ Advanced features যেমন snapshots, compression, encryption প্রদান করে

সঠিক filesystem selection এবং configuration এর মাধ্যমে একটি efficient এবং reliable Linux system পরিচালনা করা যায়।
