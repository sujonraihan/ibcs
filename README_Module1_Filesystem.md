# Module 1: Introduction & Linux Filesystem Tree Layout

## Overview
This training module is part of the **Linux Server Administration with Storage & Backup System** program.  
It provides a detailed understanding of the **Linux filesystem structure**, basic architecture, and practical navigation commands.

---

## Objectives
By the end of this module, learners will be able to:
- Understand Linux architecture and its components.
- Explore and describe the Linux Filesystem Hierarchy (FHS).
- Identify the purpose of key system directories.
- Perform hands-on navigation and monitoring of filesystems.

---

## Duration
**4 Hours (1 Day)**  
Includes theory, demonstrations, and practical lab sessions.

---

## Topics Covered
1. **Introduction to Linux**
   - What is Linux
   - Linux distributions (RHEL, Ubuntu, CentOS, Debian, etc.)
   - Key features and enterprise uses

2. **Linux Architecture Overview**
   - Kernel, Shell, File System, User Space
   - Role of each layer in OS functionality

3. **Linux Filesystem Tree Layout**
   - Detailed explanation of `/`, `/bin`, `/etc`, `/home`, `/root`, `/boot`, `/dev`, `/var`, `/proc`, `/sys`, etc.
   - Filesystem Hierarchy Standard (FHS)
   - Virtual filesystems (`/proc`, `/sys`)

4. **Hands-on Exercises**
   - Navigating Linux directories (`pwd`, `cd`, `ls`)
   - Viewing filesystem usage (`df`, `du`)
   - Exploring `/proc` and `/sys` directories

5. **Summary and Assessment**
   - Review of key directories and commands
   - Quiz to test knowledge retention

---

## Lab Commands

### Navigation
```bash
pwd
cd /etc
ls -l
tree /
```

### Disk and Directory Usage
```bash
df -h
du -sh /*
```

### System Info from /proc
```bash
cat /proc/cpuinfo
cat /proc/meminfo
ls /sys/class/net
```

---

## Assessment Questions
1. What is the purpose of `/etc/fstab`?
2. Which directory holds kernel and bootloader files?
3. What’s the difference between `/home` and `/root`?
4. Which directory contains device files?
5. Which directory is used for temporary mounts?

---

## Resources
- **Training Slide:** [Linux_Module1_Filesystem_Training.pptx](Linux_Module1_Filesystem_Training.pptx)
- **Recommended Distro:** CentOS Stream / Ubuntu Server
- **Virtualization Tools:** VMware Workstation or VirtualBox

---

## Author
**K M Sujon Raihan**  
Lead Support Engineer – Systems, Virtualization, SAN Storage & Backup  
Training Material Prepared for: *Linux Server Administration with Storage & Backup System*

---
