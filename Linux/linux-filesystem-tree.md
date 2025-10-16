# Linux Filesystem Tree Layout

## 📁 Overview

The Linux filesystem is a hierarchical structure that starts from a root directory (`/`). This structure follows the Filesystem Hierarchy Standard (FHS).

## 🗂️ Root Directory Structure

```
/
├── bin/          # Essential user binaries
├── boot/         # Boot loader files
├── dev/          # Device files
├── etc/          # System configuration files
├── home/         # User home directories
├── lib/          # Essential shared libraries
├── media/        # Removable media mount points
├── mnt/          # Temporary mount points
├── opt/          # Optional software packages
├── proc/         # Process information pseudo-filesystem
├── root/         # Root user's home directory
├── run/          # Runtime variable data
├── sbin/         # Essential system binaries
├── srv/          # Service data
├── sys/          # System information pseudo-filesystem
├── tmp/          # Temporary files
├── usr/          # User programs and data
└── var/          # Variable data files
```

## 📋 Key Directories Explained

### `/bin` - Essential User Binaries
- **Purpose**: Contains essential command binaries for all users
- **Examples**: `ls`, `cp`, `mv`, `rm`, `cat`, `echo`
- **Permissions**: Executable by all users

```bash
# View contents of /bin
ls -la /bin

# Check specific binary
which ls
file /bin/ls
```

### `/sbin` - System Binaries
- **Purpose**: Contains system administration binaries
- **Examples**: `fdisk`, `mkfs`, `mount`, `umount`, `ifconfig`
- **Permissions**: Usually requires root privileges

```bash
# View system binaries
ls -la /sbin

# Check if command exists
which fdisk
```

### `/etc` - System Configuration
- **Purpose**: Contains system-wide configuration files
- **Examples**: `/etc/passwd`, `/etc/hosts`, `/etc/fstab`
- **Permissions**: Usually readable by all, writable by root

```bash
# Important configuration files
ls -la /etc/
cat /etc/passwd
cat /etc/hosts
cat /etc/fstab
```

### `/dev` - Device Files
- **Purpose**: Contains device files representing hardware
- **Examples**: `/dev/sda`, `/dev/tty1`, `/dev/null`
- **Types**: Block devices, character devices, pseudo-devices

```bash
# List all devices
ls -la /dev/

# Check disk devices
ls -la /dev/sd*
ls -la /dev/hd*

# Check terminal devices
ls -la /dev/tty*
```

### `/proc` - Process Information
- **Purpose**: Virtual filesystem providing process and system information
- **Examples**: `/proc/cpuinfo`, `/proc/meminfo`, `/proc/version`

```bash
# System information
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/version

# Process information
ls /proc/
cat /proc/1/status  # Process 1 status
```

### `/sys` - System Information
- **Purpose**: Virtual filesystem for kernel and hardware information
- **Examples**: `/sys/class/`, `/sys/devices/`

```bash
# System information
ls /sys/
ls /sys/class/
ls /sys/devices/
```

### `/var` - Variable Data
- **Purpose**: Contains variable data files
- **Subdirectories**:
  - `/var/log/` - Log files
  - `/var/spool/` - Spool directories
  - `/var/tmp/` - Temporary files
  - `/var/cache/` - Cache files

```bash
# Log files
ls -la /var/log/
tail -f /var/log/syslog

# Spool directories
ls -la /var/spool/
```

### `/usr` - User Programs
- **Purpose**: Contains user programs and data
- **Subdirectories**:
  - `/usr/bin/` - User binaries
  - `/usr/sbin/` - System binaries
  - `/usr/lib/` - Libraries
  - `/usr/share/` - Shared data
  - `/usr/local/` - Local software

```bash
# User programs
ls /usr/bin/ | head -20
ls /usr/share/

# Local software
ls /usr/local/
```

### `/home` - User Home Directories
- **Purpose**: Contains user home directories
- **Structure**: `/home/username/`
- **Permissions**: User owns their home directory

```bash
# List home directories
ls -la /home/

# Check user home
echo $HOME
ls -la $HOME
```

### `/tmp` - Temporary Files
- **Purpose**: Temporary files that may be deleted on reboot
- **Permissions**: All users can create files
- **Cleanup**: Usually cleaned on system restart

```bash
# Temporary files
ls -la /tmp/
touch /tmp/test_file
rm /tmp/test_file
```

## 🔧 Important Commands

### Navigation Commands
```bash
# Change directory
cd /path/to/directory
cd ..              # Parent directory
cd ~               # Home directory
cd /               # Root directory

# List directory contents
ls -la             # Detailed listing
ls -lh             # Human readable sizes
ls -R              # Recursive listing
```

### Directory Operations
```bash
# Create directories
mkdir directory_name
mkdir -p /path/to/nested/directory

# Remove directories
rmdir empty_directory
rm -rf directory_with_contents

# Copy directories
cp -r source destination
```

### File Operations
```bash
# Create files
touch filename
echo "content" > filename

# View files
cat filename
less filename
head filename
tail filename

# Copy files
cp source destination
cp -r source_dir destination_dir

# Move/rename files
mv old_name new_name
mv file destination/

# Remove files
rm filename
rm -f filename    # Force remove
rm -rf directory  # Recursive force remove
```

## 📊 Filesystem Information

### Check Filesystem Usage
```bash
# Disk usage
df -h              # Human readable
df -T              # With filesystem type

# Directory usage
du -h directory
du -sh directory   # Summary only
du -h --max-depth=1 /path
```

### Mount Information
```bash
# Mounted filesystems
mount
cat /proc/mounts
cat /etc/fstab

# Mount new filesystem
mount /dev/sda1 /mnt/point
umount /mnt/point
```

## 🛡️ Security Considerations

### File Permissions
```bash
# Check permissions
ls -la filename
stat filename

# Change permissions
chmod 755 filename
chmod u+x filename
chmod g-w filename
chmod o-r filename

# Change ownership
chown user:group filename
chown -R user:group directory
```

### Special Directories
- **`/root`**: Root user's home (not `/home/root`)
- **`/tmp`**: World-writable, sticky bit set
- **`/var/tmp`**: Persistent temporary files
- **`/proc`**: Virtual filesystem, no actual files

## 🔍 Troubleshooting

### Common Issues
```bash
# Permission denied
ls -la /path/to/file
sudo ls -la /path/to/file

# Directory not found
pwd
ls -la current_directory

# Disk full
df -h
du -sh /var/log/
```

### Useful Commands
```bash
# Find files
find /path -name "filename"
find /path -type f -name "*.txt"
find /path -size +100M

# Locate files
locate filename
updatedb

# Which command
which command_name
whereis command_name
```

## 📚 Best Practices

1. **Never delete system files** from `/bin`, `/sbin`, `/lib`
2. **Use `/tmp`** for temporary files
3. **Store user data** in `/home` directory
4. **Configuration files** go in `/etc`
5. **Log files** are in `/var/log`
6. **Use absolute paths** in scripts
7. **Check permissions** before accessing files
8. **Regular cleanup** of `/tmp` and `/var/tmp`

## 🎯 Summary

Linux filesystem tree layout provides a standardized structure for organizing files and directories. Understanding this hierarchy is essential for:

- System administration
- File organization
- Security management
- Troubleshooting
- Script development

The hierarchical structure ensures consistency across different Linux distributions while providing flexibility for system customization.
