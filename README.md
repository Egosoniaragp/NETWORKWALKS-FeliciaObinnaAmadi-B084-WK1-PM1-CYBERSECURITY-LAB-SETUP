# NETWORKWALKS-FeliciaObinnaAmadi-B084-WK1-PM1-CYBERSECURITY-LAB-SETUP
Cyber security internship 

# Cybersecurity Lab Environment Setup - Week 1

Building an isolated virtual lab for penetration testing and ethical hacking practice

## Project Overview
This project focuses on setting up a virtual cybersecurity lab using VirtualBox and Kali Linux.
Purpose: create a controlled environment where network scanning, troubleshooting and security testing can be performed safely.

## Lab Components
- VirtualBox - Virtualization
- Kali Linux 2026.3 - Security workstation
- NAT Network 10.0.0.0/24 - Isolated network

## 1. VirtualBox Network Configuration
Created custom NAT Network:
- Name: NatNetwork
- Network: 10.0.0.0/24
- Gateway: 10.0.0.1
- DHCP Disabled for static assignment



## 2. Kali Linux Static IP Configuration
Commands used:
sudo nmcli connection modify "Wired connection 1" ipv4.addresses 10.0.0.2/24 ipv4.gateway 10.0.0.1 ipv4.dns "8.8.8.8,10.0.0.1" ipv4.method manual
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection up "Wired connection 1"

## 3. Validation & Testing
### IP Verification
`ip a` shows 10.0.0.2/24 on eth0
![Success Fix](success-fix.png)
Shows: nmcli modify with ipv4.dad-timeout 0 (off), Connection successfully activated, ip a shows 10.0.0.2/24 on eth0, ping 8.8.8.8 success


### Gateway Verification
`ip r` shows default via 10.0.0.1

### DNS Verification
`cat /etc/resolv.conf` shows 8.8.8.8


### Connectivity Tests
- ping 8.8.8.8 - Success 0% loss
- ping google.com - Success (proves DNS working)


## 4. Challenges & Solution
**Problem:** Connection activation failed: IP configuration could not be reserved (no available address, timeout)
**Cause:** VirtualBox NAT Network Duplicate Address Detection (DAD) default timeout -1 fails for static IP
**Solution:** Set `ipv4.dad-timeout 0` to disable DAD:
`sudo nmcli con mod "Wired connection 1" ipv4.dad-timeout 0`
Verified with `nmcli -f ipv4.dad-timeout con show "Wired connection 1"` showing 0
![Error Example](troubleshooting.png)
Common mistake: using "con down" with IP parameters causes "is not an active connection" error - correct way is "con mod"

## 5. Snapshot
Took VirtualBox snapshot named "Network Fixed 10.0.0.2" after successful fix to preserve working state.
![Snapshot](snapshot.png)
VirtualBox snapshot "kali Linux setup" preserved



## Key Takeaways
- Understanding VirtualBox NAT vs NAT Network is crucial for isolation
- nmcli is the tool to configure static IP in Kali
- DAD timeout must be disabled in VirtualBox lab environments
