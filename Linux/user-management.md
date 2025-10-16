# User Account Management

## 👤 Overview

User Account Management হল Linux system এ user accounts তৈরি, পরিচালনা, এবং security নিশ্চিত করার প্রক্রিয়া। এটি system administration এর একটি গুরুত্বপূর্ণ অংশ।

## 🎯 User Management Objectives

- **User Creation**: Create new user accounts
- **User Modification**: Modify existing user accounts
- **User Deletion**: Remove user accounts
- **Security**: Implement user security policies
- **Access Control**: Manage user permissions and access

## 🔧 User Account Types

### 1. Root User
- **UID**: 0
- **Purpose**: System administrator
- **Privileges**: Full system access
- **Home Directory**: /root

### 2. System Users
- **UID**: 1-999 (Red Hat/CentOS), 1-999 (Ubuntu/Debian)
- **Purpose**: Service accounts
- **Privileges**: Limited system access
- **Home Directory**: Usually /var or /srv

### 3. Regular Users
- **UID**: 1000+ (Red Hat/CentOS), 1000+ (Ubuntu/Debian)
- **Purpose**: Human users
- **Privileges**: Limited system access
- **Home Directory**: /home/username

## 🛠️ User Management Commands

### 1. useradd - Create User
```bash
# Basic user creation
sudo useradd username

# Create user with specific options
sudo useradd -m -s /bin/bash -c "Full Name" username

# Create user with specific UID
sudo useradd -u 1001 username

# Create user with specific GID
sudo useradd -g groupname username

# Create user with multiple groups
sudo useradd -G group1,group2 username

# Create user with home directory
sudo useradd -m -d /home/username username

# Create user with specific shell
sudo useradd -s /bin/bash username
```

### 2. usermod - Modify User
```bash
# Change user's home directory
sudo usermod -d /new/home/username username

# Change user's shell
sudo usermod -s /bin/zsh username

# Change user's UID
sudo usermod -u 1002 username

# Change user's GID
sudo usermod -g newgroup username

# Add user to additional groups
sudo usermod -a -G group1,group2 username

# Lock user account
sudo usermod -L username

# Unlock user account
sudo usermod -U username

# Change user's comment
sudo usermod -c "New Full Name" username
```

### 3. userdel - Delete User
```bash
# Delete user (keep home directory)
sudo userdel username

# Delete user and home directory
sudo userdel -r username

# Force delete user
sudo userdel -f username

# Delete user and remove mail spool
sudo userdel -r -f username
```

### 4. passwd - Password Management
```bash
# Change user's password
sudo passwd username

# Change current user's password
passwd

# Lock user's password
sudo passwd -l username

# Unlock user's password
sudo passwd -u username

# Set password expiration
sudo passwd -e username

# Set password to never expire
sudo passwd -x -1 username
```

## 📊 User Information Commands

### View User Information
```bash
# Show user information
id username
id -u username          # Show UID
id -g username          # Show GID
id -G username          # Show all groups

# Show user details
finger username
getent passwd username
cat /etc/passwd | grep username
```

### User Status
```bash
# Show logged in users
who
w
users

# Show user sessions
loginctl list-sessions
loginctl show-session SESSION_ID

# Show user processes
ps aux | grep username
pgrep -u username
```

### User Groups
```bash
# Show user's groups
groups username
id -Gn username

# Show group members
getent group groupname
cat /etc/group | grep groupname
```

## 🔧 User Configuration Files

### /etc/passwd
```bash
# View passwd file
cat /etc/passwd

# Format: username:x:UID:GID:comment:home:shell
# Example: john:x:1001:1001:John Doe:/home/john:/bin/bash

# Edit passwd file (use vipw)
sudo vipw
```

### /etc/shadow
```bash
# View shadow file
sudo cat /etc/shadow

# Format: username:password:lastchange:min:max:warn:inactive:expire:reserved
# Example: john:$6$salt$hash:19000:0:99999:7:::

# Edit shadow file (use vipw)
sudo vipw -s
```

### /etc/group
```bash
# View group file
cat /etc/group

# Format: groupname:x:GID:members
# Example: developers:x:1001:john,jane,bob

# Edit group file (use vigr)
sudo vigr
```

## 🔍 User Security

### Password Policies
```bash
# Set password aging
sudo chage -M 90 username        # Maximum days
sudo chage -m 7 username         # Minimum days
sudo chage -W 7 username         # Warning days
sudo chage -I 30 username        # Inactive days
sudo chage -E 2024-12-31 username  # Expiration date

# Show password aging
sudo chage -l username
```

### Account Locking
```bash
# Lock account
sudo usermod -L username
sudo passwd -l username

# Unlock account
sudo usermod -U username
sudo passwd -u username

# Check account status
sudo passwd -S username
```

### Login Restrictions
```bash
# Set login restrictions
sudo usermod -s /bin/false username    # Disable login
sudo usermod -s /sbin/nologin username # Disable login with message

# Set account expiration
sudo usermod -e 2024-12-31 username
sudo chage -E 2024-12-31 username
```

## 🔧 User Environment

### User Home Directory
```bash
# Create home directory
sudo mkdir /home/username
sudo chown username:username /home/username
sudo chmod 755 /home/username

# Copy skeleton files
sudo cp -r /etc/skel/* /home/username/
sudo chown -R username:username /home/username

# Set home directory permissions
sudo chmod 700 /home/username
```

### User Shell Configuration
```bash
# Set default shell
sudo usermod -s /bin/bash username

# Available shells
cat /etc/shells

# Change shell
chsh -s /bin/zsh
sudo usermod -s /bin/zsh username
```

### User Environment Variables
```bash
# Set user environment
sudo usermod -e 2024-12-31 username

# User environment files
~/.bashrc
~/.bash_profile
~/.profile
~/.bash_logout
```

## 📊 User Monitoring

### User Activity
```bash
# Show user login history
last username
last -n 10 username

# Show user processes
ps aux | grep username
pgrep -u username

# Show user file usage
sudo du -h /home/username
sudo find /home/username -type f -exec ls -la {} \;
```

### User Sessions
```bash
# Show active sessions
who
w
users

# Show session details
loginctl list-sessions
loginctl show-session SESSION_ID

# Kill user session
sudo pkill -u username
sudo killall -u username
```

## 🔒 User Security Best Practices

### Password Security
```bash
# Enforce strong passwords
sudo nano /etc/pam.d/common-password
# Add: password requisite pam_pwquality.so retry=3 minlen=8 difok=3

# Set password complexity
sudo nano /etc/security/pwquality.conf
# Set: minlen=8, dcredit=-1, ucredit=-1, lcredit=-1, ocredit=-1
```

### Account Security
```bash
# Set account limits
sudo nano /etc/security/limits.conf
# Add: username hard nproc 100
# Add: username hard fsize 100000

# Set login restrictions
sudo nano /etc/security/access.conf
# Add: -:username:ALL
```

### Audit User Activity
```bash
# Enable audit logging
sudo nano /etc/audit/auditd.conf
sudo systemctl enable auditd
sudo systemctl start auditd

# Monitor user activity
sudo ausearch -ua username
sudo ausearch -ua username -ts today
```

## 🔧 User Management Scripts

### User Creation Script
```bash
#!/bin/bash
# User creation script

USERNAME=$1
FULLNAME=$2
GROUP=$3

if [ -z "$USERNAME" ]; then
    echo "Usage: $0 username fullname group"
    exit 1
fi

# Create user
sudo useradd -m -s /bin/bash -c "$FULLNAME" $USERNAME

# Set password
sudo passwd $USERNAME

# Add to group
if [ ! -z "$GROUP" ]; then
    sudo usermod -a -G $GROUP $USERNAME
fi

# Set permissions
sudo chmod 700 /home/$USERNAME

echo "User $USERNAME created successfully"
```

### User Monitoring Script
```bash
#!/bin/bash
# User monitoring script

echo "=== User Activity Report ==="
echo "Date: $(date)"
echo ""

echo "=== Currently Logged In Users ==="
who
echo ""

echo "=== User Processes ==="
ps aux --sort=-%cpu | head -10
echo ""

echo "=== User Disk Usage ==="
sudo du -h /home | sort -hr | head -10
echo ""

echo "=== Recent Logins ==="
last -n 10
echo ""

echo "=== Failed Login Attempts ==="
sudo grep "Failed password" /var/log/auth.log | tail -10
```

## 🔍 User Troubleshooting

### Common Issues
```bash
# User cannot login
sudo passwd -S username
sudo chage -l username

# User home directory issues
ls -la /home/username
sudo chown -R username:username /home/username

# User permission issues
id username
groups username
```

### User Recovery
```bash
# Reset user password
sudo passwd username

# Unlock user account
sudo usermod -U username
sudo passwd -u username

# Restore user home directory
sudo mkdir /home/username
sudo chown username:username /home/username
sudo chmod 700 /home/username
```

## 📈 User Management Best Practices

### 1. User Creation
- Use descriptive usernames
- Set strong passwords
- Assign appropriate groups
- Set proper permissions

### 2. Security
- Implement password policies
- Set account expiration
- Monitor user activity
- Regular security audits

### 3. Maintenance
- Regular user cleanup
- Monitor disk usage
- Update user information
- Document user policies

### 4. Backup
- Backup user data
- Document user configurations
- Plan for user migration
- Regular user audits

## 🎯 Summary

User Account Management হল Linux system administration এর একটি গুরুত্বপূর্ণ অংশ যা:

- ✅ User accounts তৈরি এবং পরিচালনা করে
- ✅ Security policies implement করে
- ✅ Access control নিশ্চিত করে
- ✅ User activity monitor করে
- ✅ System security maintain করে

সঠিক user management practices এবং tools ব্যবহার করে একটি secure এবং efficient Linux system পরিচালনা করা যায়।
