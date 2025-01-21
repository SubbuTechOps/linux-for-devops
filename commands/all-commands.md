# Complete Linux Commands Guide for System Administrators

## Section 1: Basic Commands

### Directory Operations
1. Print Working Directory
```bash
pwd   # Output: /home/username
```

2. List Directory Contents
```bash
ls              # Basic listing
ls -l           # Long format
ls -la          # Include hidden files
ls -lh          # Human readable sizes
ls -ltr         # Sort by time, reversed
ls -R           # Recursive listing
```

3. Change Directory
```bash
cd /path        # Go to specific path
cd ..           # Go up one level
cd ~            # Go to home directory
cd -            # Go to previous directory
```

4. Create/Remove Directories
```bash
mkdir folder            # Create directory
mkdir -p a/b/c         # Create nested directories
rmdir folder           # Remove empty directory
rm -rf folder          # Remove directory and contents
```

### File Operations
1. Create Files
```bash
touch file.txt         # Create empty file
echo "text" > file.txt # Create with content
cat > file.txt        # Create and write (Ctrl+D to save)
```

2. Copy Files
```bash
cp source dest         # Copy file
cp -r folder1 folder2  # Copy directory recursively
cp -p file1 file2     # Preserve permissions
cp -i file1 file2     # Interactive (ask before overwrite)
```

3. Move/Rename Files
```bash
mv file1 file2        # Rename file
mv file dir/          # Move file to directory
mv -i source dest     # Interactive move
```

4. Delete Files
```bash
rm file.txt           # Remove file
rm -i file.txt        # Interactive removal
rm -f file.txt        # Force removal
rm -rf folder         # Remove directory and contents
```

### File Viewing
1. View File Contents
```bash
cat file.txt          # Display entire file
less file.txt         # Page through file
more file.txt         # Page through file (old version)
head -n 10 file.txt   # Show first 10 lines
tail -n 10 file.txt   # Show last 10 lines
tail -f file.txt      # Follow file updates
```

2. File Information
```bash
file filename         # Show file type
stat filename         # Show file stats
wc filename          # Count lines, words, chars
```

## Section 2: Text Processing

### Basic Text Processing
1. grep (Pattern Matching)
```bash
grep pattern file     # Search for pattern
grep -i pattern file  # Case-insensitive search
grep -r pattern dir   # Recursive search
grep -v pattern file  # Inverse match
grep -c pattern file  # Count matches
```

2. sed (Stream Editor)
```bash
sed 's/old/new/' file         # Replace first occurrence
sed 's/old/new/g' file       # Replace all occurrences
sed -i 's/old/new/g' file    # In-place replacement
sed '1d' file                # Delete first line
sed '/pattern/d' file        # Delete lines matching pattern
```

3. awk (Text Processing)
```bash
awk '{print $1}' file        # Print first column
awk -F: '{print $1}' file    # Use custom delimiter
awk 'NR==1{print}' file      # Print first line
awk '{sum+=$1} END{print sum}' file  # Sum first column
```

### Advanced Text Processing
1. sort
```bash
sort file                    # Sort lines
sort -n file                # Numeric sort
sort -r file                # Reverse sort
sort -k2 file               # Sort by second column
```

2. uniq
```bash
uniq file                   # Remove adjacent duplicates
uniq -c file                # Count occurrences
uniq -d file                # Show only duplicates
```

3. cut
```bash
cut -d: -f1 file           # Cut by delimiter
cut -c1-5 file             # Cut by characters
```

## Section 3: System Administration

### User Management
1. User Commands
```bash
useradd username           # Create user
userdel username           # Delete user
usermod -aG group user     # Add user to group
passwd username            # Set/change password
```

2. Group Management
```bash
groupadd groupname         # Create group
groupdel groupname         # Delete group
groupmod -n new old        # Rename group
```

3. Permissions
```bash
chmod 755 file             # Change permissions
chmod u+x file             # Add execute permission
chown user:group file      # Change ownership
chgrp group file          # Change group ownership
```

### Process Management
1. Process Information
```bash
ps aux                     # List all processes
ps -ef                     # Full process listing
pgrep process_name         # Get process ID
top                        # Dynamic process view
htop                       # Enhanced top
```

2. Process Control
```bash
kill pid                   # Kill process
kill -9 pid               # Force kill
pkill process_name        # Kill by name
nice -n 10 command        # Run with priority
renice 10 pid             # Change priority
```

### System Monitoring
1. System Information
```bash
uname -a                   # System information
uptime                     # System uptime
free -h                    # Memory usage
df -h                      # Disk usage
du -sh directory          # Directory size
```

2. Performance Monitoring
```bash
vmstat 1                   # Virtual memory stats
iostat                     # IO statistics
sar                        # System activity report
```

## Section 4: Networking

### Network Configuration
1. Interface Management
```bash
ip addr                    # Show IP addresses
ip link                    # Show interfaces
ip route                   # Show routing table
ifconfig                   # Legacy interface config
```

2. Network Testing
```bash
ping host                  # Test connectivity
traceroute host           # Trace packet route
mtr host                  # Combined trace/ping
netstat -tuln             # Show listening ports
ss -tuln                  # Modern netstat
```

3. Remote Access
```bash
ssh user@host             # SSH connection
scp file user@host:path   # Secure copy
rsync -av source dest     # Sync directories
```

### Network Troubleshooting
1. Network Diagnostics
```bash
dig domain                # DNS lookup
nslookup domain          # Name server lookup
whois domain             # Domain information
```

2. Packet Analysis
```bash
tcpdump -i eth0          # Capture packets
nmap host                # Port scan
iptables -L              # List firewall rules
```

## Section 5: Storage Management

### Disk Operations
1. Disk Information
```bash
fdisk -l                  # List partitions
lsblk                     # List block devices
blkid                     # Block device info
```

2. Filesystem Operations
```bash
mount device path         # Mount filesystem
umount device            # Unmount filesystem
fsck device              # Check filesystem
mkfs.ext4 device         # Create ext4 filesystem
```

3. Logical Volume Management
```bash
pvcreate device          # Create physical volume
vgcreate vg device       # Create volume group
lvcreate -L size vg      # Create logical volume
lvextend -L +1G lv       # Extend logical volume
```

## Section 6: System Maintenance

### Package Management
1. Debian/Ubuntu
```bash
apt update               # Update package list
apt upgrade              # Upgrade packages
apt install package      # Install package
apt remove package       # Remove package
```

2. RedHat/CentOS
```bash
yum update              # Update packages
yum install package     # Install package
yum remove package      # Remove package
rpm -qa                 # List installed packages
```

### System Services
1. Systemd
```bash
systemctl start service  # Start service
systemctl stop service   # Stop service
systemctl status service # Check status
systemctl enable service # Enable at boot
```

2. Logging
```bash
journalctl             # View system logs
journalctl -u service  # View service logs
journalctl -f          # Follow log updates
```

### Backup and Recovery
1. Backup Commands
```bash
tar -czf backup.tar.gz files  # Create archive
tar -xzf backup.tar.gz       # Extract archive
dd if=/dev/sda of=disk.img   # Disk image
```

2. System Rescue
```bash
fsck /dev/sda1              # Check filesystem
badblocks device           # Check for bad blocks
rescue mode                # Boot rescue mode
```

## Section 7: Security

### Access Control
1. File Permissions
```bash
umask 022                 # Set default permissions
getfacl file              # Get ACL
setfacl -m u:user:rw file # Set ACL
```

2. Security Monitoring
```bash
last                      # Show last logins
lastb                     # Show bad login attempts
who                       # Show logged in users
w                         # Show user activity
```

### System Security
1. Security Tools
```bash
fail2ban-client status    # Show banned IPs
chroot directory command  # Run in chroot
SELinux commands          # Security policy
```

2. Encryption
```bash
gpg -c file              # Encrypt file
gpg file.gpg             # Decrypt file
ssh-keygen               # Generate SSH key
```

## Section 8: Shell Scripting

### Script Basics
1. Script Structure
```bash
#!/bin/bash              # Shebang line
set -e                   # Exit on error
set -x                   # Debug mode
```

2. Variables and Loops
```bash
variable=value           # Set variable
for i in {1..10}        # For loop
while true              # While loop
case $var in            # Case statement
```

### Advanced Scripting
1. Functions
```bash
function_name() {        # Define function
local variable          # Local variable
return value            # Return value
}
```

2. Error Handling
```bash
trap 'cleanup' EXIT     # Trap signals
if [ $? -ne 0 ]        # Check exit status
```

## Best Practices

### System Administration
1. Regular Maintenance
- Update system regularly
- Monitor system logs
- Back up important data
- Check system performance
- Document changes

2. Security
- Use strong passwords
- Implement firewall rules
- Keep software updated
- Monitor security logs
- Use encryption

3. Performance
- Monitor resource usage
- Optimize system settings
- Clean up unused files
- Schedule maintenance tasks

### Command Line Tips
1. Keyboard Shortcuts
- Ctrl+R: Search history
- Ctrl+A: Start of line
- Ctrl+E: End of line
- Ctrl+U: Clear to start
- Ctrl+K: Clear to end

2. Command History
```bash
history                 # View command history
!number                 # Run command by number
!!                      # Repeat last command
```

Remember:
- Always test commands in a non-production environment first
- Make backups before significant changes
- Document all system modifications
- Keep security in mind
- Monitor system regularly
- Maintain comprehensive documentation
