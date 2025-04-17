Home Network Security Lab
Overview
This project simulates a secure home network using VirtualBox on Arch Linux with Hyprland. It includes a pfSense firewall managing WAN, LAN (192.168.10.0/24), and DMZ (192.168.2.0/24) networks, a LAN client on Ubuntu (192.168.10.100), and a DMZ web server on Alpine Linux (192.168.2.101) running Nginx. The goal is to practice firewall configuration, traffic monitoring, and network administration skills for a system and network administrator internship.
Setup Steps
1. pfSense VM Setup

Downloaded: netgate-installer-amd64.iso.gz from pfsense.org.
Decompressed: gunzip netgate-installer-amd64.iso.gz.
VM Config: 512MB RAM, 8GB disk, NAT (WAN), Internal LAN (initially 192.168.1.1/24, later changed to 192.168.10.1/24 to avoid Starlink conflict).
Installed pfSense: Set LAN DHCP range to 192.168.10.100–200.
Resolved KVM Conflict: Unloaded kvm_intel to allow VirtualBox to run (April 16, 2025).
Web UI: Accessible at https://192.168.10.1 (admin/pfsense).

2. LAN Client Setup

Created VM: LAN-Client (Ubuntu, 2GB RAM, 10GB disk).
Network: Internal Network LAN, assigned IP 192.168.10.100/24 via DHCP.
Tested Access: Confirmed connectivity to pfSense (ping 192.168.10.1) and web UI.

3. DMZ Web Server Setup

Created VM: DMZ-Web-Alpine (Alpine Linux, 512MB RAM, 2GB disk).
Network: Internal Network DMZ, assigned IP 192.168.2.101/24 via DHCP.
Installed Nginx: Document root at /var/www/localhost/htdocs.
Test Page: Added index.html with <h1>Welcome to DMZ Web Server</h1>.
Timezone Fix: Set to Africa/Antananarivo (Madagascar, UTC+3) using tzdata (April 16, 2025).
Tested Access: From LAN client via curl http://192.168.2.101.

4. Firewall Configuration

LAN Rules:
Allow LAN to DMZ web server (192.168.2.101:80).
Allow LAN to internet.
Block LAN to RFC1918 networks (using alias RFC1918_Networks).


DMZ Rules:
Allow DMZ to internet.
Block DMZ to LAN.
Block DMZ to RFC1918 networks.


Fixes: Reordered rules and adjusted RFC1918 alias to exclude lab subnets (April 17, 2025).

5. Traffic Monitoring

Enabled Logging: On firewall rules for passed and blocked traffic.
Monitored Logs: Via Status > System Logs > Firewall.
Packet Capture: Used Diagnostics > Packet Capture to analyze LAN traffic.

Challenges and Solutions

Starlink IP Conflict: Changed pfSense LAN from 192.168.1.1 to 192.168.10.1 to avoid conflict with Starlink router (April 16, 2025).
VirtualBox KVM Error: Unloaded kvm_intel to allow pfSense VM to run (April 16, 2025).
DMZ Connectivity: Added NAT for DMZ internet access and adjusted firewall rules (April 16, 2025).
Nginx 404 Error: Fixed document root to /var/www/localhost/htdocs on Alpine (April 16, 2025).
RFC1918 Blocking Issue: Reordered rules and excluded lab subnets from alias (April 17, 2025).

Future Improvements

Add IDS/IPS (e.g., Suricata) for advanced threat detection.
Simulate attacks (e.g., brute force) to test firewall rules.
Expand to include VLANs, as practiced in Packet Tracer (April 10, 2025).

Tools Used

Arch Linux with Hyprland
VirtualBox
pfSense 2.7.2
Ubuntu (LAN client)
Alpine Linux (DMZ web server)
Nginx
Git/GitHub for version control

