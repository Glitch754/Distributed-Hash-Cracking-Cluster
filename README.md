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

# Step 1: Network setup

Before i install Proxmox or do anything else, i needed to reserve a static IP on both machines (The main PC and the server) so the DHCP doesn't reassign IPs again and again. So i logged into my home router's Homepage.

*Router's Loginpage*

<img width="1013" height="959" alt="TP-Link1" src="https://github.com/user-attachments/assets/29d0b4fb-1957-47c5-bd10-dd03b7ad0447" />


Then i went to "Advenced" -> "Network" -> "DHCP Server" and scrolled down to the "Address Reservation" part of the page, where we will reserve our static IP's. 

*Address Reservation Webpage part*

<img width="762" height="251" alt="TP-Link2" src="https://github.com/user-attachments/assets/fde000fc-2c51-4649-8d33-b9e5e1a9800c" />


I also needed the MAC Addresses of both machines so i can identify them on the network, so i opened a Command Prompt on both machines and used the "ipconfig /all" command to find out.

*CMD ipconfig /all command output main PC*

<img width="621" height="842" alt="TP-Link4" src="https://github.com/user-attachments/assets/cbe5fbf0-f969-4d6e-ac68-f65e1aaa062b" />


*CMD ipconfig /all command output server*

<img width="716" height="924" alt="TP-Link_Server_CMD" src="https://github.com/user-attachments/assets/5b73401d-60de-4839-ab1d-7d9be7afdad6" />


Now that i know both MAC Addresses, i went back to the Address Reservation Webpage part and assigned the "192.168.0.101" IP as my main PC, and the "192.168.0.102" IP as the server

<img width="669" height="210" alt="aaaa_redacted" src="https://github.com/user-attachments/assets/05a4d8f4-2d20-48f6-94eb-3d1837050eea" />


Then i went back on the Command Prompt on both machines as Admin and used the ipconfig /release command to drop its current IP Address and the ipconfig /renew to request the new IP Address from the DHCP Server

*CMD ipconfig /release & ipconfig /renew commands Main PC*

<img width="446" height="830" alt="Main_redacted" src="https://github.com/user-attachments/assets/7662258d-0160-41e5-9001-46082de189da" />


*CMD ipconfig /release & ipconfig /renew commands Server*

<img width="800" height="986" alt="server_redacted" src="https://github.com/user-attachments/assets/af427a41-6649-4ab6-9762-aad7566a9775" />



Now we are ready for step 2

# Step 2: Wipe the server and install Proxmox VE 9.2

I first installed the Proxmox VE 9.2 ISO Installer at: https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso/proxmox-ve-9-2-iso-installer

*Proxmox downloads page*

<img width="947" height="960" alt="Prox1" src="https://github.com/user-attachments/assets/d7faf495-3371-472e-9779-bfdb7428a718" />


Then i verified if the download wasn't corrupted by opening a PowerShell where the file downloaded is located and run: Get-FileHash ./proxmox-ve_9.2-1.iso -Algorithm SHA256 to get the SHA256checksum before comparing it to the official checksum

*PowerShell console showing the SHA256SUM*

<img width="841" height="130" alt="Prox2" src="https://github.com/user-attachments/assets/56d1b20d-3478-4cee-a865-a411b4a34947" />

Now that we are sure the download isn't corrupted, we'll use Rufus to write the ISO image on a bootable USB stick

*Rufus before*

<img width="469" height="539" alt="Rufus1" src="https://github.com/user-attachments/assets/d31fb15a-1809-46a6-b48e-f9288a87b30e" />

*Rufus after*

<img width="471" height="537" alt="Rufus2" src="https://github.com/user-attachments/assets/845dedac-ef56-474b-8e72-6f4e514bc64a" />

Now that we have the installer ready, we will make the server boot into the USB stick to install Proxmox

aaaaaaa

*Boot Menu*

aaaaa

We will select the "Install Proxmox VE (Graphical) option

*License*

aaaa

Click "I agree"

*Target Disk*

aaaa

Since i only have a 2TB HDD on the server i'll only check if the filesystem is "ext4" and just click "Next

*Location/timezone/keyboard*

aaaaaa

Since i'm in Romania, i'll select Romania/Bucharest and leave the keyboard as it is

*Root passord + email*

aaaaaa

Here i'll just put my personal gmail and a password

*Network configuration*

aaaaaa

Here i've put:
1. Management Interface -> My server's MAC Address
2. Hostname (FQDN) -> pve.server (its just the name)
3. IP Address -> 192.168.0.102 (The static ip of my server)
4. Gateway -> my router's Default Gateway
5. DNS Server -> my router's Primary DNS

*Summary*

aaaaa

Simply click "Install" and let it do it's thing 

*ProxMox installing*

aaaa

*ProxMox almost done*

aaaaaa

*Install complete*

Now we will need to configure the server,

Now that ProxMox is finaly local, we can start step 3.

# Step 3

