# File Permissions and Ownership

## 🔐 Overview

File Permissions and Ownership is the process of ensuring file access control and security in Linux systems. It is an important part of system security and data protection.

## 🎯 Permission Concepts

### Permission Types
- **Read (r)**: View file contents, list directory contents
- **Write (w)**: Modify file contents, create/delete files in directory
- **Execute (x)**: Run file as program, access directory

### Permission Categories
- **Owner (u)**: File owner
- **Group (g)**: Group members
- **Others (o)**: All other users

### Special Permissions
- **Setuid (s)**: Execute with owner's privileges
- **Setgid (s)**: Execute with group's privileges
- **Sticky Bit (t)**: Only owner can delete files

## 🔧 Permission Commands

### 1. chmod - Change Permissions
```bash
# Symbolic mode
chmod u+x filename          # Add execute for owner
chmod g-w filename          # Remove write for group
chmod o+r filename          # Add read for others
chmod a+x filename          # Add execute for all
chmod u=rwx,g=rx,o=r filename # Set specific permissions

# Numeric mode
chmod 755 filename          # rwxr-xr-x
chmod 644 filename          # rw-r--r--
chmod 600 filename          # rw-------
chmod 777 filename          # rwxrwxrwx

# Recursive permissions
chmod -R 755 directory
chmod -R u+x directory
```

### 2. chown - Change Ownership
```bash
# Change owner
sudo chown username filename
sudo chown username directory

# Change owner and group
sudo chown username:groupname filename
sudo chown username:groupname directory

# Change group only
sudo chown :groupname filename
sudo chown :groupname directory

# Recursive ownership
sudo chown -R username:groupname directory
sudo chown -R username directory
```

### 3. chgrp - Change Group
```bash
# Change group
sudo chgrp groupname filename
sudo chgrp groupname directory

# Recursive group change
sudo chgrp -R groupname directory
```

## 📊 Permission Information

### View Permissions
```bash
# List file permissions
ls -la filename
ls -la directory

# Detailed file information
stat filename
stat -c "%a %n" filename

# Permission in octal
ls -la filename | awk '{print $1}'
```

### Permission Analysis
```bash
# Check file permissions
ls -la /path/to/file
stat /path/to/file

# Check directory permissions
ls -ld /path/to/directory
stat /path/to/directory

# Check special permissions
ls -la /usr/bin/passwd
ls -la /tmp
ls -la /var/tmp
```

## 🔍 Special Permissions

### Setuid (Set User ID)
```bash
# Set setuid bit
sudo chmod u+s filename
sudo chmod 4755 filename

# Remove setuid bit
sudo chmod u-s filename
sudo chmod 0755 filename

# Check setuid files
find / -type f -perm -4000 2>/dev/null
```

### Setgid (Set Group ID)
```bash
# Set setgid bit
sudo chmod g+s filename
sudo chmod 2755 filename

# Remove setgid bit
sudo chmod g-s filename
sudo chmod 0755 filename

# Check setgid files
find / -type f -perm -2000 2>/dev/null
```

### Sticky Bit
```bash
# Set sticky bit
sudo chmod +t directory
sudo chmod 1777 directory

# Remove sticky bit
sudo chmod -t directory
sudo chmod 0777 directory

# Check sticky bit directories
find / -type d -perm -1000 2>/dev/null
```

## 🔒 Security Permissions

### Secure File Permissions
```bash
# Secure file permissions
chmod 600 filename          # Owner read/write only
chmod 640 filename          # Owner read/write, group read
chmod 644 filename          # Owner read/write, others read

# Secure directory permissions
chmod 700 directory         # Owner full access only
chmod 750 directory         # Owner full, group read/execute
chmod 755 directory         # Owner full, others read/execute
```

### System File Permissions
```bash
# System files
sudo chmod 644 /etc/passwd
sudo chmod 600 /etc/shadow
sudo chmod 644 /etc/group
sudo chmod 600 /etc/gshadow

# System directories
sudo chmod 755 /etc
sudo chmod 700 /root
sudo chmod 755 /home
```

## 📊 Permission Monitoring

### Permission Auditing
```bash
# Find world-writable files
find / -type f -perm -002 2>/dev/null

# Find setuid files
find / -type f -perm -4000 2>/dev/null

# Find setgid files
find / -type f -perm -2000 2>/dev/null

# Find sticky bit directories
find / -type d -perm -1000 2>/dev/null

# Find files with no owner
find / -nouser 2>/dev/null

# Find files with no group
find / -nogroup 2>/dev/null
```

### Permission Changes
```bash
# Monitor permission changes
sudo auditctl -w /path/to/file -p wa
sudo ausearch -f /path/to/file

# Check file integrity
sudo md5sum /path/to/file
sudo sha256sum /path/to/file
```

## 🔧 Permission Management Scripts

### Permission Audit Script
```bash
#!/bin/bash
# Permission audit script

echo "=== File Permission Audit ==="
echo "Date: $(date)"
echo ""

echo "=== World-writable Files ==="
find / -type f -perm -002 2>/dev/null | head -10
echo ""

echo "=== Setuid Files ==="
find / -type f -perm -4000 2>/dev/null | head -10
echo ""

echo "=== Setgid Files ==="
find / -type f -perm -2000 2>/dev/null | head -10
echo ""

echo "=== Sticky Bit Directories ==="
find / -type d -perm -1000 2>/dev/null | head -10
echo ""

echo "=== Files with No Owner ==="
find / -nouser 2>/dev/null | head -10
echo ""

echo "=== Files with No Group ==="
find / -nogroup 2>/dev/null | head -10
```

### Permission Fix Script
```bash
#!/bin/bash
# Permission fix script

# Fix world-writable files
find /home -type f -perm -002 -exec chmod 644 {} \;

# Fix world-writable directories
find /home -type d -perm -002 -exec chmod 755 {} \;

# Fix setuid files
find /home -type f -perm -4000 -exec chmod 755 {} \;

# Fix setgid files
find /home -type f -perm -2000 -exec chmod 755 {} \;

# Fix sticky bit directories
find /home -type d -perm -1000 -exec chmod 755 {} \;

echo "Permission fixes completed"
```

## 🔍 Permission Troubleshooting

### Common Issues
```bash
# Permission denied
ls -la filename
sudo chmod 644 filename

# Cannot access directory
ls -ld directory
sudo chmod 755 directory

# Cannot execute file
ls -la filename
chmod +x filename
```

### Permission Recovery
```bash
# Reset file permissions
sudo chmod 644 filename
sudo chown root:root filename

# Reset directory permissions
sudo chmod 755 directory
sudo chown root:root directory

# Reset recursive permissions
sudo chmod -R 755 directory
sudo chown -R root:root directory
```

## 📈 Permission Best Practices

### 1. File Permissions
- Use minimal required permissions
- Avoid world-writable files
- Set appropriate ownership
- Regular permission audits

### 2. Directory Permissions
- Use appropriate directory permissions
- Set sticky bit for shared directories
- Monitor directory access
- Regular permission reviews

### 3. Security
- Implement least privilege principle
- Monitor permission changes
- Regular security audits
- Document permission policies

### 4. Maintenance
- Regular permission cleanup
- Monitor file ownership
- Update permission policies
- Backup permission configurations

## 🎯 Permission Commands Summary

### Basic Permissions
```bash
chmod 755 filename          # rwxr-xr-x
chmod 644 filename          # rw-r--r--
chmod 600 filename          # rw-------
chmod 777 filename          # rwxrwxrwx
```

### Ownership Commands
```bash
chown username filename     # Change owner
chown username:group filename # Change owner and group
chgrp groupname filename    # Change group
```

### Special Permissions
```bash
chmod u+s filename          # Setuid
chmod g+s filename          # Setgid
chmod +t directory          # Sticky bit
```

## 🎯 Summary

File Permissions and Ownership is an important part of Linux system security that:

- ✅ Ensures file access control
- ✅ Implements security policies
- ✅ Provides data protection
- ✅ Maintains system integrity
- ✅ Manages user access

With proper permission management practices and tools, a secure and efficient Linux system can be managed.
