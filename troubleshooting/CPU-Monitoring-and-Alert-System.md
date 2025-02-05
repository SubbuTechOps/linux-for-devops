# CPU Monitoring and Alert System Documentation

## Overview
This document outlines the setup and configuration of a CPU monitoring system that sends email alerts when CPU usage exceeds a defined threshold.

## Prerequisites
- Ubuntu/Debian Linux system
- Root or sudo access
- Gmail account with 2-Step Verification enabled

## 1. Email Configuration Setup

### 1.1 Install Required Packages
```bash
sudo apt-get update
sudo apt-get install postfix mailutils
```
When installing postfix, select "Internet Site" when prompted.

### 1.2 Configure Postfix for Gmail
Edit the main configuration file:
```bash
sudo vi /etc/postfix/main.cf
```

Replace/update with the following configuration:
```
# See /usr/share/postfix/main.cf.dist for a commented, more complete version
smtpd_banner = $myhostname ESMTP $mail_name (Ubuntu)
biff = no
append_dot_mydomain = no
readme_directory = no
compatibility_level = 3.6

# TLS parameters
smtpd_tls_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
smtpd_tls_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
smtp_tls_CApath=/etc/ssl/certs
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache

# General Mail Settings
myhostname = your-hostname
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
myorigin = /etc/mailname
mydestination = $myhostname, localhost
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128
mailbox_size_limit = 0
recipient_delimiter = +
inet_interfaces = all
inet_protocols = all

# Gmail SMTP Settings
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_security_options = noanonymous
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
smtp_tls_security_level = encrypt
smtp_tls_mandatory_protocols = !SSLv2,!SSLv3,!TLSv1,!TLSv1.1
smtp_tls_mandatory_ciphers = high
smtpd_relay_restrictions = permit_mynetworks permit_sasl_authenticated defer_unauth_destination
```

### 1.3 Setup Gmail App Password
1. Enable 2-Step Verification in your Gmail account
2. Go to Google Account → Security → 2-Step Verification → App passwords
3. Select "Mail" and "Other (Custom name)"
4. Copy the generated 16-character password

### 1.4 Configure Gmail Credentials
Create and edit the password file:
```bash
sudo vi /etc/postfix/sasl_passwd
```

Add your Gmail credentials:
```
[smtp.gmail.com]:587    your-gmail@gmail.com:your-16-character-app-password
```

Set proper permissions:
```bash
sudo postmap /etc/postfix/sasl_passwd
sudo chown root:root /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
sudo chmod 0600 /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
```

Restart Postfix:
```bash
sudo systemctl restart postfix
```

Test email configuration:
```bash
echo "Test email" | mail -s "Test Subject" recipient@gmail.com
```

## 2. CPU Monitoring Script

### 2.1 Create the Script
Create a new file `cpu-alerts.sh`:
```bash
sudo vi cpu-alerts.sh
```

Add the following content:
```bash
#!/bin/bash
# Threshold for CPU utilization
THRESHOLD=80
# Get current date and time
DATE=$(date '+%Y-%m-%d %H:%M:%S')
# Get CPU usage (using mpstat)
CPU_USAGE=$(mpstat 1 1 | grep "Average" | awk '{print $3 + $5}')
echo "[$DATE] Current CPU Usage % is: $CPU_USAGE"

# Remove decimal points for comparison
CPU_USAGE=${CPU_USAGE%.*}
# If CPU usage is greater than threshold, send an alert
if [ "$CPU_USAGE" -gt "$THRESHOLD" ]; then
    echo "[$DATE] Warning: CPU usage is above ${THRESHOLD}%. Current usage: ${CPU_USAGE}%" | \
    mail -s "High CPU Alert on $(hostname) - ${DATE}" your-email@gmail.com
fi
```

### 2.2 Set Script Permissions
```bash
sudo chmod +x cpu-alerts.sh
```

### 2.3 Test the Script
```bash
sudo ./cpu-alerts.sh
```

## 3. Automation (Optional)

### 3.1 Setup Cron Job
Edit crontab:
```bash
sudo crontab -e
```

Add this line to run every 5 minutes:
```
*/5 * * * * /path/to/cpu-alerts.sh
```

## 4. Monitoring and Maintenance

### 4.1 Check Script Logs
To check if emails are being sent:
```bash
tail -f /var/log/mail.log
```

### 4.2 Check Postfix Status
```bash
systemctl status postfix
```

## 5. Troubleshooting

### 5.1 Common Issues
1. Emails not sending:
   - Check Postfix status
   - Verify Gmail app password
   - Check mail logs

2. Script not running:
   - Check script permissions
   - Verify mpstat is installed
   - Check cron logs if automated

### 5.2 Debug Commands
```bash
# Check mail logs
tail -f /var/log/mail.log

# Test mail manually
echo "test" | mail -s "test" your-email@gmail.com

# Check postfix configuration
postconf -n

# Test script manually
sudo ./cpu-alerts.sh
```

## 6. Security Considerations
- Keep the sasl_passwd file secure (600 permissions)
- Regularly update the app password
- Monitor mail logs for unauthorized access
- Use secure TLS settings in postfix configuration

## 7. Customization
The script can be modified to:
- Adjust the threshold value
- Change the monitoring interval
- Add additional metrics
- Modify email format
- Add multiple recipients

## 8. Version History
- v1.0: Initial setup with basic CPU monitoring
- v1.1: Added timestamp to alerts
- v1.2: Enhanced security settings
