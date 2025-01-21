# Essential Linux Commands for Senior DevOps Engineers

## System Information and Monitoring

### Process Management
- `ps aux` - Display all running processes
  ```bash
  ps aux | grep nginx  # Find all nginx processes
  ```
- `top` / `htop` - Interactive process viewer
  ```bash
  htop -u username  # Monitor processes for specific user
  ```
- `lsof` - List open files and processes
  ```bash
  lsof -i :80  # Show processes using port 80
  ```

### Resource Monitoring
- `vmstat` - Virtual memory statistics
  ```bash
  vmstat 1 5  # Display stats every 1 second, 5 times
  ```
- `iostat` - CPU and disk I/O statistics
  ```bash
  iostat -xz 1  # Extended disk stats, omit zero values
  ```
- `sar` - System activity reporter
  ```bash
  sar -n DEV 1  # Monitor network interfaces every second
  ```

### System Information
- `uname` - System information
  ```bash
  uname -a  # All system information
  ```
- `dmesg` - Kernel ring buffer
  ```bash
  dmesg -T | grep -i error  # Show timestamped kernel errors
  ```
- `systemctl` - Control systemd services
  ```bash
  systemctl status docker  # Check Docker service status
  ```

## File System Operations

### Navigation and Search
- `find` - Search for files
  ```bash
  find /var/log -type f -mtime +7 -name "*.log"  # Find log files older than 7 days
  ```
- `locate` - Quick file search using database
  ```bash
  updatedb && locate pattern  # Update database and search
  ```
- `grep` - Search text patterns
  ```bash
  grep -R "ERROR" /var/log/  # Recursively search for "ERROR" in logs
  ```

### File Operations
- `dd` - Convert and copy files
  ```bash
  dd if=/dev/zero of=/tmp/testfile bs=1M count=100  # Create 100MB test file
  ```
- `rsync` - Fast file copying and synchronization
  ```bash
  rsync -avz --progress /source/ user@remote:/destination/  # Sync directories
  ```
- `tar` - Archive manipulation
  ```bash
  tar -czf backup.tar.gz /path/to/directory  # Create compressed archive
  ```

## Network Operations

### Connectivity
- `netstat` - Network statistics
  ```bash
  netstat -tulpn  # Show listening ports and processes
  ```
- `ss` - Socket statistics
  ```bash
  ss -tunlp  # Show TCP/UDP listening ports
  ```
- `ip` - Show/manipulate routing, network devices, interfaces
  ```bash
  ip addr show  # Show network interfaces
  ip route show  # Show routing table
  ```

### Network Diagnostics
- `tcpdump` - Packet analyzer
  ```bash
  tcpdump -i eth0 port 80  # Capture HTTP traffic
  ```
- `nmap` - Network exploration and security scanning
  ```bash
  nmap -sS -p 1-65535 hostname  # TCP SYN scan all ports
  ```
- `curl` - Transfer data from/to servers
  ```bash
  curl -v -X POST https://api.example.com/endpoint -d '{"key":"value"}'
  ```

## Storage Management

### Disk Operations
- `df` - Disk free space
  ```bash
  df -h  # Human readable disk usage
  ```
- `du` - Disk usage
  ```bash
  du -sh /var/*  # Summarize disk usage of /var subdirectories
  ```
- `fdisk` - Partition table manipulator
  ```bash
  fdisk -l  # List all partitions
  ```

### Logical Volume Management
- `pvs`, `vgs`, `lvs` - LVM status
  ```bash
  lvextend -L +10G /dev/mapper/vg-lv  # Extend logical volume
  ```
- `resize2fs` - Resize filesystem
  ```bash
  resize2fs /dev/mapper/vg-lv  # Resize filesystem after LV extension
  ```

## Container and Orchestration

### Docker
- `docker` - Container operations
  ```bash
  docker stats  # Live container resource usage
  docker system prune -af  # Clean up unused resources
  ```
- `docker-compose` - Multi-container applications
  ```bash
  docker-compose up -d --scale service=3  # Scale service to 3 replicas
  ```

### Kubernetes
- `kubectl` - Kubernetes cluster management
  ```bash
  kubectl get pods -A -o wide  # List all pods with details
  kubectl logs -f deployment/name  # Stream deployment logs
  ```

## Performance Analysis

### CPU Profiling
- `perf` - Performance analysis tools
  ```bash
  perf top -p $(pgrep process_name)  # CPU analysis for process
  ```
- `strace` - Trace system calls
  ```bash
  strace -f -p $(pgrep nginx)  # Trace nginx process system calls
  ```

### Memory Analysis
- `free` - Memory usage
  ```bash
  free -m  # Display memory usage in megabytes
  ```
- `pmap` - Process memory map
  ```bash
  pmap -x $(pgrep mysql)  # Show MySQL process memory map
  ```

## Security and Access Control

### User Management
- `useradd` / `usermod` - User account management
  ```bash
  useradd -m -G docker,sudo username  # Create user with groups
  ```
- `chown` / `chmod` - Change ownership and permissions
  ```bash
  chmod -R 750 /path/to/directory  # Recursive permission change
  ```

### Security Monitoring
- `ausearch` - Search audit logs
  ```bash
  ausearch -m LOGIN --success no  # Find failed login attempts
  ```
- `fail2ban-client` - Ban management
  ```bash
  fail2ban-client status sshd  # Check SSH jail status
  ```

## Shell Scripting Essentials

### Text Processing
- `awk` - Pattern scanning and processing
  ```bash
  awk '{sum+=$1} END {print sum/NR}' data.txt  # Calculate average
  ```
- `sed` - Stream editor
  ```bash
  sed -i 's/old-text/new-text/g' file.txt  # In-place text replacement
  ```
- `jq` - JSON processor
  ```bash
  curl api.example.com | jq '.items[] | select(.status=="active")'
  ```

### Process Control
- `nohup` - Run command immune to hangups
  ```bash
  nohup long-running-command &  # Run in background
  ```
- `nice` / `renice` - Adjust process priority
  ```bash
  nice -n 10 cpu-intensive-command  # Run with lower priority
  ```

## Advanced Troubleshooting

### System Calls
- `ltrace` - Library call tracer
  ```bash
  ltrace -p $(pgrep application)  # Trace library calls
  ```
- `pidstat` - Process statistics
  ```bash
  pidstat -d 1  # Monitor I/O statistics every second
  ```

### Network Debugging
- `mtr` - Network diagnostic tool
  ```bash
  mtr -n example.com  # Continuous traceroute
  ```
- `nc` (netcat) - Network utility
  ```bash
  nc -zv hostname 22  # Test TCP connection to port 22
  ```

## Best Practices
1. Always use `-i` flag with `rm` for interactive deletion
2. Use `ctrl+r` for reverse history search
3. Implement `alias` for frequently used commands
4. Use `screen` or `tmux` for persistent sessions
5. Regular system monitoring with `sar` and storing historical data
6. Implement log rotation and management
7. Regular backup verification and testing
8. Use configuration management tools (Ansible, Puppet, etc.)
9. Implement proper access control and sudo policies
10. Regular security updates and patch management

Remember to always test commands in a non-production environment first, especially those that modify system state or configuration.
