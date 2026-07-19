# 🛡️ Ultimate Cybersecurity Command Reference

> **Overview:** A practical, cybersecurity-focused command reference organized by job function. This guide is tailored for professionals in Blue Team, SOC, VAPT, Incident Response, Threat Hunting, and GRC/Auditing environments. 
> 
> *Format: Interactive Markdown — easily expandable sections with quick copy-paste code blocks.*

---

## 🎯 Target Platforms
- 🐧 **Linux** (Ubuntu, Kali, Debian, RHEL)
- 🪟 **Windows** (CMD)
- ⚡ **Windows PowerShell**

---

## 📑 Table of Contents
1. [🌐 Network Configuration](#1-network-configuration)
2. [📡 Connectivity Testing](#2-connectivity-testing)
3. [🔍 Network Monitoring & Packet Analysis](#3-network-monitoring--packet-analysis)
4. [📶 Wireless Networking](#4-wireless-networking)
5. [⚔️ Security Assessment (VAPT)](#5-security-assessment-vapt)
6. [🧱 Firewall & Network Security](#6-firewall--network-security)
7. [📜 Log & Event Management](#7-log--event-management)
8. [⚙️ Processes & Network](#8-processes--network)
9. [📁 File Transfer](#9-file-transfer)
10. [🤖 Automation & Scripting](#10-automation--scripting)
11. [📋 Compliance & Audit](#11-compliance--audit)
12. [🛡️ SOC & Threat Hunting Toolkit](#12-soc--threat-hunting-toolkit)
13. [🔥 Master Cheat Sheet & Learning Path](#13-master-cheat-sheet--learning-path)

---

## <a id="1-network-configuration"></a>🌐 1. Network Configuration

<details>
<summary><b>👁️ View IP Configuration & Interfaces</b> (Click to expand)</summary>

### View IP Configuration
**Linux**
```bash
ip addr
ip a
ifconfig
```

**Windows CMD**
```cmd
ipconfig
ipconfig /all
```

**Windows PowerShell**
```powershell
Get-NetIPAddress
Get-NetIPConfiguration
```

### View Interfaces
**Linux**
```bash
ip link
nmcli device
iw dev
```

**Windows CMD / PowerShell**
```cmd
netsh interface show interface
```
```powershell
Get-NetAdapter
```
</details>

<details>
<summary><b>⚙️ DHCP & DNS Management</b> (Click to expand)</summary>

**Windows CMD**
```cmd
ipconfig /release
ipconfig /renew
ipconfig /flushdns
ipconfig /registerdns
```

**Linux DNS Configuration**
```bash
cat /etc/resolv.conf
resolvectl status
```
</details>

<details>
<summary><b>🔌 Network Interface States & Routing</b> (Click to expand)</summary>

### Bring Interface Up/Down (Linux)
```bash
sudo ip link set eth0 up
sudo ip link set eth0 down
nmcli connection up "Office"
nmcli connection down "Office"
```

### Configure Static IP & Routing (Linux)
```bash
# Temporary IP Configuration
sudo ip addr add 192.168.1.20/24 dev eth0

# Default Gateway Setup
sudo ip route add default via 192.168.1.1
```

### View Routing Table
**Linux**
```bash
ip route
route -n
```

**Windows**
```cmd
route print
```
```powershell
Get-NetRoute
```
</details>

<details>
<summary><b>🆔 ARP, MAC & Hostname Details</b> (Click to expand)</summary>

### ARP Table
**Linux**
```bash
ip neigh
arp -a
```
**Windows**
```cmd
arp -a
```

### MAC Address
**Linux**
```bash
ip link
```
**Windows**
```cmd
getmac
```

### Hostname Management
**Linux**
```bash
hostname
hostnamectl
```
**Windows**
```cmd
hostname
```
</details>

---

## <a id="2-connectivity-testing"></a>📡 2. Connectivity Testing

<details>
<summary><b>📍 Ping, Traceroute & TCP Ports</b> (Click to expand)</summary>

### Ping
**Linux**
```bash
ping -c 4 google.com
```
**Windows**
```cmd
ping -n 4 google.com
```

### Traceroute & Path Analysis
**Linux**
```bash
traceroute google.com
mtr google.com
```
**Windows**
```cmd
tracert google.com
pathping google.com
```

### Test TCP Ports
**Linux (Netcat / Telnet)**
```bash
nc -vz 192.168.1.5 22
telnet 192.168.1.5 80
```
**PowerShell**
```powershell
Test-NetConnection google.com -Port 443
```
</details>

<details>
<summary><b>🌍 DNS Troubleshooting & Reconnaissance</b> (Click to expand)</summary>

**Linux (Dig & Host)**
```bash
dig google.com
dig MX google.com
dig NS google.com
dig ANY example.com
dig -x 8.8.8.8     # Reverse lookup
host google.com
```

**Cross-Platform**
```bash
nslookup google.com
whois example.com
```
</details>

---

## <a id="3-network-monitoring--packet-analysis"></a>🔍 3. Network Monitoring & Packet Analysis

<details open>
<summary><b>📊 Port & Socket Statistics</b></summary>

### Modern Socket Stats (Linux ss)
```bash
ss -tulnp    # All listening ports with process names
ss -ant      # All established connections
```

### Legacy Netstat
**Linux**
```bash
netstat -tulnp
```
**Windows**
```cmd
netstat -ano
netstat -ab  # Requires Admin
```

### Open Sockets (Linux lsof)
```bash
lsof -i
lsof -i :443
```
</details>

<details open>
<summary><b>🦈 Packet Capture (tcpdump & tshark)</b></summary>

### TCPDump (Linux)
```bash
sudo tcpdump                     # Capture all
sudo tcpdump -i eth0             # Specific interface
sudo tcpdump port 80             # Specific port
sudo tcpdump host 192.168.1.10   # Specific host
sudo tcpdump -w capture.pcap     # Write to PCAP file
tcpdump -r capture.pcap          # Read from PCAP file
```

### TShark (CLI Wireshark)
```bash
tshark
tshark -r file.pcap              # Read PCAP
tshark -z io,stat                # Display Statistics
```
> 💡 *Pro Tip: Use **Wireshark** when a GUI is preferred for deep packet inspection.*
</details>

---

## <a id="4-wireless-networking"></a>📶 4. Wireless Networking (Linux)

<details>
<summary><b>📻 Interfaces, Scanning & Monitor Mode</b> (Click to expand)</summary>

### Interfaces & Info
```bash
iw dev
iwconfig
```

### Wireless Reconnaissance
```bash
sudo iw dev wlan0 scan
```

### Enable Monitor Mode
```bash
sudo airmon-ng start wlan0
```
</details>

---

## <a id="5-security-assessment-vapt"></a>⚔️ 5. Security Assessment (VAPT)

<details open>
<summary><b>🗺️ Network Scanning (Nmap & Masscan)</b></summary>

### Nmap Essentials
```bash
nmap -sn 192.168.1.0/24      # Host discovery (Ping scan)
nmap -sS target              # TCP SYN scan (Stealth)
nmap -sV target              # Service Version detection
nmap -O target               # OS detection
nmap -A target               # Aggressive scan (OS, version, scripts, traceroute)
nmap -sU target              # UDP scan
nmap -sC target              # Default safe scripts
nmap --script vuln target    # Vulnerability scanning scripts
nmap -oX scan.xml target     # Output as XML
nmap -oG scan.gnmap target   # Grepable output
```

### Masscan (High-Speed Scanning)
```bash
masscan 192.168.1.0/24 -p1-65535
```
</details>

<details open>
<summary><b>🕵️ Enumeration, Web & Exploitation Tools</b></summary>

### Netcat Fundamentals
```bash
nc -lvnp 4444                # Start a listener
nc 192.168.1.5 4444          # Connect to a client/listener
nc target 80                 # Simple banner grabbing
```

### Service Enumeration
```bash
smbclient -L //host          # Enumerate SMB shares
enum4linux target            # Windows/Samba enumeration
rpcclient -U "" target       # Null session RPC client
snmpwalk -v2c -c public target # SNMP enumeration
ldapsearch                   # Query LDAP directories
```

### Web Application Testing
```bash
nikto -h target              # Web server vulnerability scanner
gobuster dir -u URL -w list  # Directory and file brute-forcing
ffuf -w list -u URL/FUZZ     # Fast web fuzzer
sqlmap -u "URL"              # Automated SQL injection and database takeover
```
</details>

---

## <a id="6-firewall--network-security"></a>🧱 6. Firewall & Network Security

<details>
<summary><b>🛡️ View & Manage Firewall Rules</b> (Click to expand)</summary>

### Linux Firewalls
```bash
iptables -L                  # List iptables rules
iptables -S                  # Print iptables rules in script format
nft list ruleset             # View modern nftables rules
ufw status                   # Uncomplicated Firewall status
firewall-cmd --list-all      # Firewalld active profile rules
```

### Windows Firewalls
```cmd
netsh advfirewall show allprofiles
```
```powershell
Get-NetFirewallRule
```
</details>

---

## <a id="7-log--event-management"></a>📜 7. Log & Event Management

<details open>
<summary><b>🐧 Linux Logging & Text Parsing</b></summary>

### Log Viewing
```bash
journalctl                   # Systemd comprehensive logs
journalctl -b                # Logs from current boot
cat /var/log/auth.log        # SSH & Auth logs (Debian/Ubuntu)
dmesg                        # Kernel ring buffer logs
tail -f /var/log/syslog      # Live streaming of system logs
```

### Powerful Text Parsing Tools
```bash
grep     # Pattern matching
egrep    # Extended regex pattern matching
awk      # Column-based text processing
sed      # Stream editor for text manipulation
cut      # Remove sections from each line of files
sort     # Sort lines of text files
uniq     # Report or omit repeated lines
wc       # Word, line, character, and byte count
```
</details>

<details open>
<summary><b>🪟 Windows Event Management</b></summary>

**GUI**
```cmd
eventvwr                     # Opens Windows Event Viewer
```

**PowerShell**
```powershell
Get-WinEvent                 # Retrieve events from event logs
Get-EventLog Security        # View classic Security logs
```
</details>

---

## <a id="8-processes--network"></a>⚙️ 8. Processes & Network

<details>
<summary><b>💀 Process Identification & Termination</b> (Click to expand)</summary>

### Linux
```bash
ps aux                       # View all running processes
top                          # Interactive process viewer
htop                         # Enhanced interactive process viewer
lsof -i                      # Map running network processes to PIDs
kill PID                     # Gracefully terminate a process
kill -9 PID                  # Force terminate a process
```

### Windows
```cmd
tasklist                     # List running tasks
taskkill /PID 1234 /F        # Force kill task by PID
```
```powershell
Get-Process                  # PowerShell process viewer
```
</details>

---

## <a id="9-file-transfer"></a>📁 9. File Transfer

<details>
<summary><b>🚚 Download, Copy & Sync</b> (Click to expand)</summary>

### SSH-Based Transfer
```bash
scp file user@host:/tmp      # Secure Copy Protocol
rsync -av source/ dest/      # Sync files/directories securely and efficiently
```

### Web-Based Transfer (cURL & Wget)
```bash
curl http://example.com      # Fetch URL content to stdout
curl -O URL                  # Download file (keeps remote filename)
curl -I URL                  # Fetch only HTTP headers
curl -X POST URL             # Send a POST request
wget URL                     # Download file directly
```
</details>

---

## <a id="10-automation--scripting"></a>🤖 10. Automation & Scripting

<details>
<summary><b>🛠️ Scripting Snippets & Task Scheduling</b> (Click to expand)</summary>

### Bash Loops & Parallel Execution
```bash
# Simple ping sweep loop
for ip in $(seq 1 254); do
  ping -c1 192.168.1.$ip
done

# Parallel execution tools
xargs
parallel
```

### Task Scheduling (Linux)
```bash
crontab -e                   # Edit cron jobs
systemctl list-timers        # View systemd timers
```

### PowerShell Automation
```powershell
ForEach-Object
Invoke-Command
```

### Core Python Modules for Networking
* `socket` - Low-level network interface
* `scapy` - Interactive packet manipulation program
* `requests` - Elegant and simple HTTP library
* `paramiko` - SSH2 protocol library
</details>

---

## <a id="11-compliance--audit"></a>📋 11. Compliance & Audit (Linux Focus)

<details>
<summary><b>🔎 Ports, Packages & System Posture</b> (Click to expand)</summary>

### Open Ports & Packages
```bash
ss -tulnp                    # Audit open ports & binding addresses
dpkg -l                      # List installed packages (Debian/Ubuntu)
rpm -qa                      # List installed packages (RHEL/CentOS)
```

### Services & Permissions
```bash
systemctl list-unit-files    # List all available services
systemctl list-units         # List currently running services
find / -perm -4000 2>/dev/null  # Find SUID binaries (Privilege Escalation check)
find / -perm -777 2>/dev/null   # Find world-writable files
```

### Identity & Access Posture
```bash
cat /etc/passwd              # Review user accounts
cat /etc/group               # Review group memberships
last                         # Review user login history
cat /etc/ssh/sshd_config     # Audit SSH daemon configuration
sudo -l                      # Check sudo permissions for current user
sestatus                     # Check SELinux status
aa-status                    # Check AppArmor status
```
</details>

---

## <a id="12-soc--threat-hunting-toolkit"></a>🛡️ 12. SOC & Threat Hunting Toolkit

| OS Platform | Essential Command Toolkit |
| :--- | :--- |
| **Linux CLI** | `journalctl`, `grep`, `find`, `strings`, `file`, `sha256sum`, `md5sum`, `sha1sum`, `base64`, `xxd`, `hexdump`, `tcpdump`, `tshark`, `lsof`, `ss`, `ps`, `systemctl` |
| **Windows PS** | `Get-WinEvent`, `Get-Process`, `Get-Service`, `Get-NetTCPConnection`, `Get-NetFirewallRule`, `Get-ScheduledTask`, `Get-FileHash` |

---

## <a id="13-master-cheat-sheet--learning-path"></a>🔥 13. Master Cheat Sheet & Learning Path

### High-Yield Commands by Category
| Category | Essential Commands |
| :--- | :--- |
| **Network Config** | `ip`, `ifconfig`, `nmcli`, `route`, `ip route`, `arp`, `hostname` |
| **Connectivity** | `ping`, `traceroute`, `mtr`, `pathping`, `nc`, `telnet` |
| **DNS** | `dig`, `host`, `nslookup`, `whois` |
| **Monitoring** | `ss`, `netstat`, `lsof`, `tcpdump`, `tshark`, `Wireshark` |
| **VAPT / Security** | `nmap`, `masscan`, `nikto`, `gobuster`, `ffuf`, `enum4linux`, `smbclient`, `snmpwalk` |
| **Firewalls** | `iptables`, `nft`, `ufw`, `firewall-cmd`, `netsh advfirewall`, `Get-NetFirewallRule` |
| **Logs & Events**| `journalctl`, `tail`, `grep`, `awk`, `sed`, `Get-WinEvent` |
| **File Transfer** | `scp`, `rsync`, `curl`, `wget` |
| **Incident Response**| `ps`, `top`, `htop`, `lsof`, `find`, `strings`, `sha256sum`, `file`, `xxd`, `hexdump` |
| **Audit & GRC** | `systemctl`, `dpkg`, `rpm`, `sudo -l`, `sestatus`, `aa-status`, `last`, `find` |

### 📈 Recommended Learning Order for Beginners
1. **Core Networking:** `ip`, `ping`, `traceroute`, `ss`, `netstat`, `arp`, `dig`
2. **Traffic Analysis:** `tcpdump`, `tshark`, Wireshark
3. **Enumeration & VAPT:** `nmap`, `nc`, `masscan`, `nikto`, `gobuster`, `enum4linux`
4. **System & Log Analysis:** `journalctl`, `grep`, `awk`, `tail`, `lsof`, `ps`
5. **Firewalls & Routing:** `iptables`, `nftables`, `ufw`, `Get-NetFirewallRule`
6. **Automation:** Bash, PowerShell, Python (`socket`, `scapy`, `requests`)
7. **Compliance / GRC:** Service inspection, package auditing, user/group management, SSH configuration review, and security module status.

---
*Generated for immediate use. Feel free to download, render in Obsidian, GitHub, or any Markdown-compatible viewer.*
