# Steps:

This is the steps in order to replicate my home virtual lab on MacOS.

## ⚙️ Step 1: Install Virtualisation Software
Choose one:
- UTM (recommended for Apple Silicon Macs, what i used)
- VirtualBox (better for intel Macs)

Install and launch the hypervisor before creating VMs.

## 💻 Step 2: Create Windows VM
Download Windows ISO from Microsoft

Create a new VM:
– RAM: 4–8 GB
- CPU: 2–4 cores
- Disk: 64+ GB (Min 64GB required)

Attach ISO and install Windows

Disable unnecessary protections (for testing only):

Windows Defender (temporarily)

Firewall (adjust rules instead of full disable if preferred)

## 🐉 Step 3: Create Kali Linux VM

Download Kali Linux ISO

Create a new VM:
- RAM: 2–4 GB
- CPU: 2 cores
- Disk: 30+ GB

Install Kali Linux

Update system:

```bash
sudo apt update && sudo apt upgrade -y
```
## 🌐 Step 4: Configure Networking

Set both VMs to:

Internal Network (preferred), OR
Host-only Adapter

Assign static IPs if needed:
Example:

Windows VM: 192.168.56.10

Kali VM: 192.168.56.11

Test connectivity:
ping 192.168.56.10

## 🔍 Step 5: Basic Security Testing

1. Network Scanning
nmap -sV 192.168.56.10
2. Vulnerability Scanning
nmap --script vuln 192.168.56.10
3. Packet Analysis
Use Wireshark to capture traffic between machines.

## 📊 Step 6: Splunk (SIEM) Integration
### 🔽 Install Splunk Enterprise on Windows VM

Download Splunk Enterprise:

Visit: https://www.splunk.com/en_us/download/splunk-enterprise.html

Choose the Windows version (.msi installer)

Run the installer on your Windows VM:
- Accept license agreement
- Choose default settings
- Set admin username and password

Start Splunk:
- Open browser and go to:
- http://localhost:8000
- Log in with your credentials

📥 Enable Windows Log Ingestion

Splunk can automatically ingest Windows logs without additional agents when installed locally.

In Splunk Web UI:
- Go to Settings → Data Inputs
- Click Local Inputs
- Select Windows Event Logs
- Add the following logs:
- Application
- Security
- System
- Save your configuration

🔎 Verify Logs Are Being Ingested

Go to Search & Reporting
Run:
- index=* sourcetype="WinEventLog:*"

You should now see live logs from your Windows machine.

📡 (Optional – Advanced) Forward Logs from Another Machine
To simulate a real SOC environment (Due to RAM constraints, I am unable to do this since my Mac has 8GB RAM):
- Install Splunk Universal Forwarder on another VM
- Configure it to send logs to your Splunk instance:
- splunk add forward-server <Splunk_Server_IP>:9997

Enable receiving on Splunk:
- Settings → Forwarding and Receiving → Enable port 9997

## 🦈 Step 7: Install and Use Wireshark for Traffic Analysis
### 🔽 Install Wireshark on Windows VM

Download Wireshark:
- Visit: https://www.wireshark.org/download.html
- Choose the Windows installer

Run the installer:
- Keep default settings
- Ensure Npcap is selected (required for packet capture)
- Launch Wireshark after installation

🌐 Select the Correct Network Interface
When Wireshark opens:
- You will see a list of network interfaces
- Look for your internal / host-only adapter (used by your lab network)

💡 Tip: The correct interface will show active traffic when your VMs are communicating.

▶️ Start Capturing Traffic
- Double-click the correct interface
- Begin live packet capture

🔎 Generate Traffic from Kali Linux
From your Kali VM, run:
- ping <Windows_VM_IP>

Or perform a scan:
- nmap -sS <Windows_VM_IP>

📊 Analyze Traffic in Wireshark
Use filters to focus on relevant traffic:

ICMP (ping traffic):
icmp

TCP traffic:
tcp

Traffic from Kali attacker box:
ip.addr == <Kali_VM_IP>
