# Linux Network Troubleshooting Guide

A comprehensive guide to network troubleshooting in Linux systems, including essential commands, common issues, and best practices.

## Table of Contents
1. [Basic Connectivity Testing](#basic-connectivity-testing)
2. [Network Interface Analysis](#network-interface-analysis)
3. [Socket and Port Analysis](#socket-and-port-analysis)
4. [Packet Analysis](#packet-analysis)
5. [Bandwidth Testing](#bandwidth-testing)
6. [DNS Troubleshooting](#dns-troubleshooting)
7. [Network Configuration](#network-configuration)
8. [Firewall Analysis](#firewall-analysis)
9. [SSL/TLS Testing](#ssltls-testing)
10. [Network Performance](#network-performance)
11. [Common Issues and Solutions](#common-issues-and-solutions)
12. [Best Practices](#best-practices)

## Basic Connectivity Testing

### Ping Test
Test basic connectivity to a host:
```bash
ping google.com
ping -c 4 google.com  # Limited to 4 packets
```

### Traceroute
Trace the route packets take to a destination:
```bash
traceroute google.com
mtr google.com        # Continuous trace with statistics
```

### DNS Resolution
Test DNS name resolution:
```bash
nslookup google.com
dig google.com
dig +short google.com  # Brief output
```

### Port Connectivity
Test specific port connectivity:
```bash
telnet host.example.com 80
nc -zv host.example.com 80
```

## Network Interface Analysis

### View Network Interfaces
```bash
# Show all interfaces
ifconfig -a
ip addr show

# Show specific interface
ip addr show eth0
```

### Network Statistics
```bash
# Interface statistics
netstat -i
ip -s link

# Real-time monitoring
iftop
nethogs
```

## Socket and Port Analysis

### List Open Ports
```bash
# Show listening ports
netstat -tuln
ss -tuln

# Show with process information
netstat -tulnp
ss -tulnp
```

### Process Network Usage
```bash
# List processes using network
lsof -i
lsof -i :80  # Specific port
```

## Packet Analysis

### Capture Packets
```bash
# Basic capture
tcpdump -i eth0

# Capture HTTP traffic
tcpdump -i eth0 'port 80'

# Save capture to file
tcpdump -i eth0 -w capture.pcap

# Read saved capture
tcpdump -r capture.pcap
```

## Bandwidth Testing

### Network Speed Test
```bash
# Start iperf server
iperf -s

# Run iperf client
iperf -c server_ip

# Monitor bandwidth
bmon
nload
```

## DNS Troubleshooting

### DNS Configuration
```bash
# Check DNS servers
cat /etc/resolv.conf

# Clear DNS cache
systemd-resolve --flush-caches
/etc/init.d/nscd restart

# Test specific DNS server
dig @8.8.8.8 google.com
```

## Network Configuration

### Routing
```bash
# Show routing table
route -n
ip route show

# Add static route
ip route add 192.168.1.0/24 via 192.168.0.1
```

### Interface Configuration
```bash
# Check interface settings
ethtool eth0

# Monitor interface errors
dmesg | grep -i eth
```

## Firewall Analysis

### IPTables
```bash
# List all rules
iptables -L

# List with line numbers
iptables -L --line-numbers

# Show NAT rules
iptables -t nat -L
```

### UFW (Uncomplicated Firewall)
```bash
# Check status
ufw status
ufw status verbose

# Test port access
nc -zv host port
```

## SSL/TLS Testing

### Certificate Testing
```bash
# Test SSL connection
openssl s_client -connect host:443

# Check certificate expiration
openssl s_client -connect host:443 2>/dev/null | openssl x509 -noout -dates

# Verify certificate chain
openssl s_client -connect host:443 -showcerts
```

## Network Performance

### Performance Monitoring
```bash
# Interface errors
ip -s link show eth0

# Network latency
mtr google.com

# Socket statistics
ss -s
```

## Common Issues and Solutions

### 1. No Network Connectivity
```bash
# Check interface status
ip link show

# Check IP address assignment
ip addr show

# Verify default gateway
ip route show

# Test DNS
ping 8.8.8.8
```

### 2. Slow Network Performance
```bash
# Check for interface errors
ethtool -S eth0

# Monitor bandwidth usage
iftop

# Check for packet loss
ping -c 100 gateway | grep "packet loss"
```

### 3. DNS Resolution Issues
```bash
# Verify DNS servers
cat /etc/resolv.conf

# Test with different DNS
dig @8.8.8.8 google.com

# Check local DNS cache
systemd-resolve --statistics
```

## Best Practices

1. Documentation
   - Maintain network diagrams
   - Document baseline performance
   - Keep logs of troubleshooting steps
   - Create runbooks for common issues

2. Monitoring
   - Set up proactive monitoring
   - Monitor system resources
   - Keep historical performance data
   - Set up alerts for critical issues

3. Security
   - Regular security audits
   - Keep firewall rules documented
   - Monitor unusual network activity
   - Regular updates and patches

4. Backup and Recovery
   - Regular configuration backups
   - Document recovery procedures
   - Test recovery processes
   - Keep backup configs accessible

5. Testing
   - Test from multiple locations
   - Verify findings with multiple tools
   - Test during off-peak hours
   - Document test results

## Required Tools

Install these common network troubleshooting tools:
```bash
# Debian/Ubuntu
sudo apt-get install net-tools tcpdump traceroute iptables \
  nmap netcat-openbsd iftop nethogs mtr-tiny bmon

# RHEL/CentOS
sudo yum install net-tools tcpdump traceroute iptables \
  nmap nc iftop nethogs mtr bmon
```

## Additional Resources

1. Man Pages
   - `man ping`
   - `man tcpdump`
   - `man netstat`
   - `man iptables`

2. System Logs
   - `/var/log/syslog`
   - `/var/log/messages`
   - `/var/log/dmesg`

3. Configuration Files
   - `/etc/network/interfaces`
   - `/etc/resolv.conf`
   - `/etc/hosts`
   - `/etc/sysconfig/network-scripts/`

Remember to always document your troubleshooting steps and maintain a record of baseline performance metrics for comparison during issues.
