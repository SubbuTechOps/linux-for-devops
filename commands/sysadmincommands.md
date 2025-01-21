# Linux Commands Reference Guide for System Administrators

## Basic Commands

### File Navigation
- `pwd` - Print working directory
  ```bash
  pwd  # Output: /home/username
  ```
- `ls` - List directory contents
  ```bash
  ls -la  # List all files including hidden ones with details
  ls -lh  # List files with human-readable sizes
  ```
- `cd` - Change directory
  ```bash
  cd /path/to/directory  # Go to specific directory
  cd ..  # Go up one level
  cd ~   # Go to home directory
  ```

### File Operations
- `cp` - Copy files and directories
  ```bash
  cp -r source_dir destination_dir  # Copy directory recursively
  cp -p file1 file2  # Copy preserving permissions
  ```
- `mv` - Move/rename files
  ```bash
  mv old_name new_name  # Rename file
  mv file /new/location/  # Move file
  ```
- `rm` - Remove files/directories
  ```bash
  rm -rf directory  # Remove directory recursively
  rm -i file  # Remove with confirmation
  ```
- `mkdir` - Create directory
  ```bash
  mkdir -p parent/child/grandchild  # Create nested directories
  ```

### File Viewing
- `cat` - View file contents
  ```bash
  cat file.txt  # Display entire file
  cat -n file.txt  # Display with line numbers
  ```
- `less` - View files page by page
  ```bash
  less +F /var/log/syslog  # Follow log file
  ```
- `head`/`tail` - View beginning/end of file
  ```bash
  head -n 20 file.txt  # View first 20 lines
  tail -f /var/log/auth.log  # Follow log file
  ```

## Intermediate Commands

### Text Processing
- `grep` - Search text patterns
  ```bash
  grep -r "pattern" /path  # Recursive search
  grep -i "pattern" file  # Case-insensitive search
  grep -v "pattern" file  # Inverse match
  ```
- `sed` - Stream editor
  ```bash
  sed -i 's/old/new/g' file  # Replace all occurrences
  sed '1d' file  # Delete first line
  ```
- `awk` - Pattern scanning and text processing
  ```bash
  awk '{print $1}' file  # Print first column
  awk -F: '{print $1,$3}' /etc/passwd  # Custom delimiter
  ```

### Process Management
- `ps` - Process status
  ```bash
  ps aux  # List all processes
  ps -ef | grep apache  # Find specific process
  ```
- `top`/`htop` - System monitoring
  ```bash
  top -u username  # Monitor user's processes
  htop -p $(pgrep apache2)  # Monitor specific process
  ```
- `kill`/`pkill` - Terminate processes
  ```bash
  kill -9 PID  # Force kill process
  pkill -f "process_name"  # Kill by name
  ```

### File Permissions
- `chmod` - Change file permissions
  ```bash
  chmod 755 file  # Set numeric permissions
  chmod u+x file  # Add execute permission for user
  ```
- `chown` - Change file owner
  ```bash
  chown user:group file  # Change owner and group
  chown -R user directory  # Recursive ownership change
  ```
- `umask` - Set default permissions
  ```bash
  umask 022  # Set default permissions
  ```

## Advanced Commands

### System Information
- `uname` - System information
  ```bash
  uname -a  # All system information
  ```
- `lscpu` - CPU information
  ```bash
  lscpu  # Detailed CPU information
  ```
- `free` - Memory usage
  ```bash
  free -h  # Human-readable memory stats
  ```
- `df`/`du` - Disk usage
  ```bash
  df -h  # Filesystem usage
  du -sh /*  # Directory sizes
  ```

### Network Management
- `ip` - IP configuration
  ```bash
  ip addr show  # Show IP addresses
  ip route show  # Show routing table
  ```
- `netstat`/`ss` - Network statistics
  ```bash
  netstat -tulpn  # Show listening ports
  ss -ant  # Show TCP connections
  ```
- `tcpdump` - Network packet analyzer
  ```bash
  tcpdump -i eth0 port 80  # Capture HTTP traffic
  ```
- `iptables` - Firewall management
  ```bash
  iptables -L  # List firewall rules
  iptables -A INPUT -p tcp --dport 22 -j ACCEPT  # Allow SSH
  ```

### Storage Management
- `fdisk` - Partition management
  ```bash
  fdisk -l  # List partitions
  fdisk /dev/sda  # Manage disk partitions
  ```
- `lvm` - Logical Volume Management
  ```bash
  lvextend -L +10G /dev/vg/lv  # Extend logical volume
  pvs; vgs; lvs  # Display LVM information
  ```
- `mount` - Mount filesystems
  ```bash
  mount -t nfs server:/share /mnt  # Mount NFS share
  mount -a  # Mount all in fstab
  ```

### System Services
- `systemctl` - Service management
  ```bash
  systemctl status service  # Check service status
  systemctl enable --now service  # Enable and start service
  ```
- `journalctl` - View system logs
  ```bash
  journalctl -u service  # View service logs
  journalctl -f  # Follow system logs
  ```

### Backup and Recovery
- `tar` - Archive management
  ```bash
  tar -czf backup.tar.gz /path  # Create compressed archive
  tar -xzf backup.tar.gz  # Extract archive
  ```
- `rsync` - File synchronization
  ```bash
  rsync -avz source/ dest/  # Sync directories
  rsync -avz -e ssh user@host:/source/ /dest/  # Remote sync
  ```

### Performance Monitoring
- `sar` - System activity reporter
  ```bash
  sar -u 1 5  # CPU usage every 1 second, 5 times
  sar -r  # Memory usage
  ```
- `iostat` - I/O statistics
  ```bash
  iostat -xz 1  # Extended I/O stats
  ```
- `vmstat` - Virtual memory statistics
  ```bash
  vmstat 1  # Memory stats every second
  ```

### Security Tools
- `auditd` - System auditing
  ```bash
  ausearch -m LOGIN --success no  # Failed logins
  auditctl -w /etc/passwd -p wa  # Watch file changes
  ```
- `fail2ban` - Intrusion prevention
  ```bash
  fail2ban-client status  # Show jail status
  fail2ban-client set sshd unbanip IP  # Unban IP
  ```

### Advanced Troubleshooting
- `strace` - Trace system calls
  ```bash
  strace -p PID  # Trace running process
  strace -e open,write command  # Trace specific calls
  ```
- `lsof` - List open files
  ```bash
  lsof -i :80  # Show process using port 80
  lsof -u user  # Show user's open files
  ```
- `perf` - Performance analysis
  ```bash
  perf top  # System profiling
  perf record -g command  # Record command profile
  ```

## Best Practices and Tips

### Security Best Practices
1. Regular system updates
   ```bash
   apt update && apt upgrade  # Debian/Ubuntu
   yum update  # RHEL/CentOS
   ```
2. Monitor auth logs
   ```bash
   tail -f /var/log/auth.log  # Debian/Ubuntu
   tail -f /var/log/secure    # RHEL/CentOS
   ```
3. Check for failed login attempts
   ```bash
   lastb  # Show bad login attempts
   ```

### Performance Optimization
1. Monitor system load
   ```bash
   uptime  # Load averages
   ```
2. Check memory usage
   ```bash
   free -m | grep Mem  # Memory usage
   ```
3. Monitor disk I/O
   ```bash
   iotop  # I/O monitoring
   ```

### Maintenance Tasks
1. Log rotation
   ```bash
   logrotate -f /etc/logrotate.conf  # Force log rotation
   ```
2. Clean old files
   ```bash
   find /tmp -type f -mtime +30 -delete  # Delete files older than 30 days
   ```
3. Check disk health
   ```bash
   smartctl -a /dev/sda  # SMART disk information
   ```

### Automation Tips
1. Use cron for scheduled tasks
   ```bash
   crontab -e  # Edit user crontab
   ```
2. Create aliases for common commands
   ```bash
   echo "alias ll='ls -la'" >> ~/.bashrc
   ```
3. Use screen/tmux for persistent sessions
   ```bash
   screen -S session_name  # Create named session
   tmux new -s session_name  # Create tmux session
   ```

Remember:
- Always backup before major system changes
- Test commands in non-production environment first
- Document all system modifications
- Keep security patches up to date
- Monitor system logs regularly
- Implement proper access controls
- Maintain system documentation
