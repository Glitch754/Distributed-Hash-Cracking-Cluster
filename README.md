# Distributed Hash-Cracking Cluster

## Objective
Build a home-lab distributed computing cluster that combines a dedicated Proxmox
hypervisor server and a GPU-equipped main PC to run distributed password
(hash) cracking jobs, host virtual machines, and serve a web-based status
dashboard — as a hands-on cybersecurity project demonstrating systems

### Skills Learned
- Setting up and managing a Proxmox VE hypervisor (VM provisioning, resource allocation)
- Configuring a local network for reliable multi-machine communication (static IPs, port verification with Nmap)
- Distributed/parallel computing concepts — splitting a workload across multiple machines' CPU and GPU
- Password security fundamentals (hashing algorithms, cracking methods, wordlists/masks) via Hashcat
- Linux system administration (Kali Linux, Debian)
- Python scripting and agent-based architectures (Hashtopolis agents)
- Web application development with Flask, and deploying it as a self-hosted service
- Network traffic verification/troubleshooting with Wireshark
- Secure remote system administration with SSH
- Technical documentation and architecture diagramming for a public portfolio

### Tools Used
- **Proxmox VE** — hypervisor for the server
- **Kali Linux** / **Debian** — operating systems for the main PC and VMs
- **Hashcat** — CPU/GPU password-cracking engine
- **Hashtopolis** — distributed cracking coordinator and agents
- **Python** — agent scripting and dashboard backend
- **Flask** — web dashboard framework
- **Nmap** — network scanning/verification
- **Wireshark** — packet capture and traffic analysis
- **OpenSSH** — secure remote management
- **Git / GitHub** — version control and portfolio hosting
- **Mermaid / draw.io** — architecture diagrams

## Steps
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
