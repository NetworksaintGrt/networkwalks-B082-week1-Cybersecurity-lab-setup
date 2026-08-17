# networkwalks-B082-week1-Cybersecurity-lab-setup

🔐 Cybersecurity Home Lab — VirtualBox & Kali Linux

Building a private virtual range for penetration testing and security research

📌 Overview

A step-by-step build of a personal cybersecurity home lab using VirtualBox and Kali Linux.

The lab is designed to be self-contained and repeatable — a safe space to run security tools, practice offensive techniques, and simulate real attack scenarios without touching live systems.

Built on a private virtual network with room to plug in additional machines as the lab grows.

🎯 Goals
Spin up and configure VirtualBox
Deploy Kali Linux as the attack platform
Create an isolated NAT Network for the lab
Configure stable network connectivity on the Kali VM
Assign a static IP for clean, consistent documentation
Verify the full network stack — IP, gateway, DNS
Snapshot the baseline state before any testing begins
Document every step for reproducibility
Hand off a ready-to-use environment for future projects
🛡️ Use Cases

A sandboxed environment built for:

Network reconnaissance
Port and service scanning
Vulnerability identification
Packet capture and traffic analysis
Web application attack simulation
Exploit development practice
Security tooling research

⚠️ This lab is for authorized use only. Only test systems you own or have explicit written permission to probe. Unauthorized testing is illegal.

⚙️ Lab Specs
🧩 Component	⚙️ Value
🖥️ Host OS	Windows 10
🧠 Host RAM	8 GB
⚡ Processor	Intel Core i7
🧰 Hypervisor	VirtualBox 7.2
🐉 Guest OS	Kali Linux 2026.2
🧠 VM RAM	2048 MB
🌐 Network Mode	NAT Network
📡 Subnet	10.0.0.0/24
🐧 Kali Static IP	10.0.0.2/24
🚪 Gateway	10.0.0.1
🌍 DNS Server	8.8.8.8
🔮 Target IP Pool	10.0.0.3–10.0.0.99
🪜 Setup Steps

Step 1 — Install 7-Zip
Needed to extract the Kali Linux .7z VM package before importing.

Step 2 — Install VirtualBox
The hypervisor that runs and manages all VMs in this lab.

Step 3 — Create the NAT Network
A dedicated virtual network was created with these settings:

Name: NatNetwork
Subnet: 10.0.0.0/24
DHCP: On
IPv6: Off

NAT Network was picked specifically because VMs on the same network can talk to each other and reach the internet — critical for multi-machine lab scenarios.

Step 4 — Import Kali Linux
Downloaded from the official Kali site and imported directly into VirtualBox.

Adapter config:

Attached to:  NAT Network
Network:      NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop

RAM allocated: 2048 MB
A shared folder was created for host-to-VM file transfers.

Step 5 — Set a Static IP on Kali
Network manually configured for a stable, fixed address:

IP:      10.0.0.2
Mask:    255.255.255.0
Gateway: 10.0.0.1
DNS:     8.8.8.8

Step 6 — Take a Baseline Snapshot
Before touching anything else, a clean snapshot was saved:

Name: Clean Kali - Network Setup

This is the recovery point. If any future exercise breaks the VM, one restore gets it back.

🔎 Verification Tests
✅ Test	🧾 Command	🎯 Pass Condition
IP assignment	ip a	Static IP visible
Gateway reachability	ping 10.0.0.1	Replies returned
Internet access	ping 8.8.8.8	Replies returned
DNS resolution	nslookup networkwalks.com	Domain resolves
Nmap availability	nmap --version	Version output shown
Snapshot integrity	Restore → ip a	Baseline IP restored
🐞 Problems & Fixes

Problem 1 — Internet dropped after static IP was set

NetworkManager didn't handle the manual config cleanly, cutting off outbound access.

Fix:

bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

Restarted the connection — internet came back.

Check your actual connection name first with nmcli connection show before running this.

Problem 2 — VM wouldn't start (VT-x disabled)

Hardware virtualization was off in BIOS, blocking the VM from launching.

Fix:

Reboot into BIOS/UEFI
Enable Intel VT-x
Save and exit
Start the Kali VM

Booted without issues after that.

💡 What This Taught Me

NAT vs NAT Network — Standard NAT isolates VMs from each other. NAT Network lets them communicate with one another while keeping internet access — the right call for any multi-machine lab.

Virtual Networking — How adapters bind to network types and how that controls which machines can see each other.

Static Addressing — Hands-on practice setting IPv4 address, mask, gateway, and DNS inside a Linux environment.

Snapshot Discipline — Capture a clean state before experimenting. Restoring is faster than rebuilding.

Technical Documentation — Logging everything — commands, configs, errors, and fixes — is what separates a professional workflow from guesswork.

🔐 Ethics Notice

Built for learning and authorized security practice only. Not for use against systems without permission.

🔗 Tools Used
7-Zip: https://7-zip.org/download.html
VirtualBox: https://virtualbox.org/wiki/Downloads
Kali Linux: https://kali.org/get-kali

👤 Author

SaintGrt — Cybersecurity Student B082
LinkedIn: https://www.linkedin.com/in/saint-grt-024b45429/
