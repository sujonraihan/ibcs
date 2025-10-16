# Package Management Systems

## 📦 Overview

Package Management Systems are tools used for installing, updating, and removing software in Linux systems. Different Linux distributions use different package managers.

## 🎯 Package Manager Types

### 1. APT (Advanced Package Tool) - Debian/Ubuntu
- **Distributions**: Ubuntu, Debian, Linux Mint
- **Package Format**: .deb
- **Database**: /var/lib/dpkg/

### 2. YUM/DNF - Red Hat/CentOS/Fedora
- **Distributions**: Red Hat, CentOS, Fedora
- **Package Format**: .rpm
- **Database**: /var/lib/rpm/

### 3. Pacman - Arch Linux
- **Distributions**: Arch Linux, Manjaro
- **Package Format**: .pkg.tar.xz
- **Database**: /var/lib/pacman/

### 4. Zypper - openSUSE
- **Distributions**: openSUSE, SUSE
- **Package Format**: .rpm
- **Database**: /var/lib/zypp/

## 🔧 APT Package Management (Debian/Ubuntu)

### Basic Commands
```bash
# Update package list
sudo apt update

# Upgrade all packages
sudo apt upgrade

# Full system upgrade
sudo apt full-upgrade

# Install package
sudo apt install package_name

# Remove package
sudo apt remove package_name

# Purge package (remove with config)
sudo apt purge package_name

# Search packages
apt search keyword

# Show package info
apt show package_name

# List installed packages
apt list --installed

# List available updates
apt list --upgradable
```

### Advanced APT Commands
```bash
# Clean package cache
sudo apt clean
sudo apt autoclean

# Remove unused packages
sudo apt autoremove

# Fix broken dependencies
sudo apt --fix-broken install

# Download package without installing
apt download package_name

# Install from .deb file
sudo dpkg -i package.deb

# Fix package issues
sudo dpkg --configure -a
```

### Repository Management
```bash
# List repositories
cat /etc/apt/sources.list
ls /etc/apt/sources.list.d/

# Add repository
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu focal main"

# Remove repository
sudo add-apt-repository --remove "repository_url"

# Update GPG keys
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys KEY_ID
```

## 🔧 YUM/DNF Package Management (Red Hat/CentOS/Fedora)

### YUM Commands (CentOS 7)
```bash
# Update package list
sudo yum update

# Install package
sudo yum install package_name

# Remove package
sudo yum remove package_name

# Search packages
yum search keyword

# Show package info
yum info package_name

# List installed packages
yum list installed

# List available packages
yum list available

# Clean cache
sudo yum clean all
```

### DNF Commands (CentOS 8+/Fedora)
```bash
# Update package list
sudo dnf update

# Install package
sudo dnf install package_name

# Remove package
sudo dnf remove package_name

# Search packages
dnf search keyword

# Show package info
dnf info package_name

# List installed packages
dnf list installed

# Clean cache
sudo dnf clean all

# Install from RPM file
sudo dnf install package.rpm
```

### Repository Management
```bash
# List repositories
yum repolist
dnf repolist

# Add repository
sudo yum-config-manager --add-repo repository_url
sudo dnf config-manager --add-repo repository_url

# Enable/disable repository
sudo yum-config-manager --enable repository_name
sudo yum-config-manager --disable repository_name
```

## 🔧 Pacman Package Management (Arch Linux)

### Basic Commands
```bash
# Update package database
sudo pacman -Sy

# Update all packages
sudo pacman -Syu

# Install package
sudo pacman -S package_name

# Remove package
sudo pacman -R package_name

# Remove with dependencies
sudo pacman -Rs package_name

# Search packages
pacman -Ss keyword

# Show package info
pacman -Si package_name

# List installed packages
pacman -Q

# List orphaned packages
pacman -Qdt
```

### Advanced Pacman Commands
```bash
# Clean package cache
sudo pacman -Sc

# Clean all cache
sudo pacman -Scc

# Remove orphaned packages
sudo pacman -Rns $(pacman -Qdtq)

# Install from AUR
yay -S package_name

# Update AUR packages
yay -Syu
```

## 📊 Package Information Commands

### Check Package Status
```bash
# APT - Check if package is installed
dpkg -l | grep package_name
apt list --installed | grep package_name

# YUM/DNF - Check package status
rpm -qa | grep package_name
yum list installed | grep package_name

# Pacman - Check package
pacman -Q package_name
```

### Package Dependencies
```bash
# APT - Show dependencies
apt-cache depends package_name
apt-cache rdepends package_name

# RPM - Show dependencies
rpm -qR package_name
rpm -q --whatrequires package_name

# Pacman - Show dependencies
pacman -Qi package_name
```

### Package Files
```bash
# APT - List package files
dpkg -L package_name

# RPM - List package files
rpm -ql package_name

# Pacman - List package files
pacman -Ql package_name
```

## 🔍 Package Troubleshooting

### Common Issues and Solutions

#### 1. Broken Dependencies
```bash
# APT - Fix broken dependencies
sudo apt --fix-broken install
sudo apt autoremove
sudo apt autoclean

# YUM/DNF - Fix dependencies
sudo yum clean all
sudo yum makecache
sudo dnf clean all
sudo dnf makecache
```

#### 2. Package Conflicts
```bash
# Check for conflicts
apt-cache policy package_name
yum check
dnf check

# Force reinstall
sudo apt install --reinstall package_name
sudo yum reinstall package_name
```

#### 3. Repository Issues
```bash
# Update repository keys
sudo apt-key update
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-*

# Check repository status
apt-cache policy
yum repolist
```

## 🛠️ Package Building and Compilation

### Compile from Source
```bash
# Download source
wget source_url.tar.gz
tar -xzf source.tar.gz
cd source_directory

# Configure and compile
./configure
make
sudo make install

# Check dependencies
./configure --help
```

### Create Packages
```bash
# Debian package
dpkg-deb --build package_directory

# RPM package
rpmbuild -ba package.spec

# Arch package
makepkg -s
```

## 📈 Package Management Best Practices

### 1. Regular Updates
```bash
# Schedule automatic updates
sudo crontab -e
# Add: 0 2 * * * apt update && apt upgrade -y
```

### 2. Security Updates
```bash
# APT - Security updates only
sudo apt update
sudo apt upgrade -s

# YUM/DNF - Security updates
sudo yum update --security
sudo dnf update --security
```

### 3. Package Cleanup
```bash
# APT - Clean unused packages
sudo apt autoremove
sudo apt autoclean

# YUM/DNF - Clean cache
sudo yum clean all
sudo dnf clean all

# Pacman - Clean cache
sudo pacman -Sc
```

### 4. Backup Package Lists
```bash
# APT - Backup installed packages
dpkg --get-selections > installed_packages.txt

# YUM/DNF - Backup packages
rpm -qa > installed_packages.txt

# Pacman - Backup packages
pacman -Qqe > installed_packages.txt
```

## 🔒 Security Considerations

### Package Verification
```bash
# Verify package integrity
dpkg --verify package_name
rpm -V package_name

# Check package signatures
apt-key list
rpm -qa --queryformat '%{NAME}-%{VERSION}-%{RELEASE} %{SIGPGP:pgpsig}\n'
```

### Secure Installation
```bash
# Install from trusted sources only
sudo apt install --allow-unauthenticated package_name  # Avoid this
sudo yum install --nogpgcheck package_name  # Avoid this

# Use official repositories
sudo apt install package_name  # Recommended
sudo yum install package_name  # Recommended
```

## 📊 Monitoring Package Management

### System Information
```bash
# Check package manager status
systemctl status packagekit
systemctl status apt-daily

# Monitor package installations
tail -f /var/log/apt/history.log
tail -f /var/log/yum.log
```

### Disk Usage
```bash
# Check package cache size
du -sh /var/cache/apt/
du -sh /var/cache/yum/
du -sh /var/cache/pacman/

# Clean old packages
sudo apt autoremove
sudo yum clean all
```

## 🎯 Summary

Package Management Systems are an important part of Linux administration. Through proper package management:

- ✅ Software installation and updates
- ✅ Dependency management
- ✅ Security updates
- ✅ System stability
- ✅ Resource optimization

Different package managers are used for different distributions, but the core principles remain the same.
