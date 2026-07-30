# SOC Home Lab
## Building a Realistic Detection & Prevention Pipeline

---

## 1. Network Topology & IP Addressing

<img width="1125" height="604" alt="image" src="https://github.com/user-attachments/assets/0df1e206-8b64-454f-9925-df82d95b6411" />


The lab environment is segmented into three logical zones: Attacker Network, Internal Network, and Monitoring Network. All traffic between the attacker and victim is forced through an inline IPS to ensure inspection and control.

| Component | IP Address | Zone | Role |
|-----------|-----------|------|------|
| Kali Linux (eth0) | 192.168.214.128 | Attacker Network | Primary attack(Nmap, Hydra, DoS, SSH Brute Force, Vulnerability Scanning (Nessus, Nikto)) |
| Kali Linux (eth1) | 192.168.1.21 | Monitoring Network | Snort IDS sensor (mirror interface) |
| Windows 11 (Adapter 1) | 192.168.1.40 | Internal Network | Monitored victim (Wazuh Agent) |
| Windows 11 (Adapter 2) | 192.168.10.1 | Internal Network | Target interface for attacks |
| Ubuntu IPS (ens38) | 192.168.214.133 | Boundary | Connected to the attacker network |
| Ubuntu IPS (ens37) | 192.168.10.133 | Boundary | Connected to the victim network |
| Ubuntu IPS (Gateway) | 192.168.10.133 | Boundary | Default gateway for victim |
| Wazuh Server (Ubuntu) | 192.168.1.26 | Monitoring Network | SIEM (log collection & analysis) |

---

## 2. Introduction

This project demonstrates the design and implementation of a fully functional SOC Home Lab environment that simulates real-world enterprise security operations. The lab integrates multiple security layers including SIEM, IDS, and IPS, enabling both detection and prevention of cyber threats.

The architecture is built using virtualized systems and segmented networks to emulate attacker, victim, and monitoring zones. Tools such as Wazuh, Snort, and Kali Linux are combined to provide full visibility, alerting, and active response capabilities.

This lab transitions from passive monitoring (IDS) to active defense (IPS), validating detection accuracy and prevention effectiveness through real attack simulations.

---

## 3. Objectives

The main objectives of this SOC lab are:

- Design a segmented and controlled lab environment simulating real SOC architecture
- Deploy and configure a SIEM solution using Wazuh
- Implement network-based detection using Snort in IDS mode
- Transition Snort into inline IPS mode for active threat prevention
- Integrate IDS/IPS logs into SIEM for centralized monitoring and correlation
- Simulate real-world attacks (Reconnaissance, DoS, Brute Force, Vulnerability Scanning)
- Validate detection accuracy and prevention efficiency before and after IPS enforcement
- Build hands-on experience in SOC operations, threat detection, and incident analysis

---

## 4. Tools Used

| Category | Tool | Purpose |
|----------|------|---------|
| SIEM | Wazuh | Log collection, correlation, and alerting |
| IDS/IPS | Snort | Network intrusion detection & prevention |
| Attacking Machine | Kali Linux | Attack simulation (Nmap, Hydra, etc.) |
| Target System | Windows 11 | Victim machine |
| IPS System | Ubuntu | Inline IPS deployment |
| SIEM Server | Ubuntu | Hosts Wazuh Manager |
| Virtualization | VMware Workstation | Lab environment |

---

## 5. Step by Step

### ➤ Step 1: Initial Setup, Wazuh SIEM Deployment, and Endpoint (victim) Integration

<img width="1150" height="341" alt="image" src="https://github.com/user-attachments/assets/18ed78eb-ce96-4e05-8fc5-4e49cf79bca0" />


Figure 1: Updating and Verifying Ubuntu Package Repository Status

<img width="1138" height="94" alt="image" src="https://github.com/user-attachments/assets/701eeff0-c9dc-4f81-b06b-8e8e250cb9df" />

Figure 2: Downloading the Wazuh Installation Script Using curl

<img width="1121" height="402" alt="image" src="https://github.com/user-attachments/assets/221b5b1a-bd33-4f72-a507-315a8d7573e6" />

Figure 3: Executing the Wazuh Installation Script (Indexer Initialization Phase)

<img width="1108" height="378" alt="image" src="https://github.com/user-attachments/assets/7840f57b-02ba-4264-91e8-00b43a945328" />

Figure 4: Completing Wazuh Manager and Dashboard Installation

<img width="1130" height="252" alt="image" src="https://github.com/user-attachments/assets/93f684ea-9eb3-4133-b4ff-fbb3a018a94f" />

Figure 5: Wazuh Dashboard Login Page

<img width="1130" height="343" alt="image" src="https://github.com/user-attachments/assets/2c2e144b-9caf-4ac7-a101-ba31f7e8c59d" />


Figure 6: Wazuh Dashboard Health Check

<img width="1147" height="449" alt="image" src="https://github.com/user-attachments/assets/7812bfa4-6366-413a-8e97-da136939ddad" />

Figure 7: Successful Ping from Windows 11 to Wazuh Server

<img width="1055" height="278" alt="image" src="https://github.com/user-attachments/assets/3692ed89-e1e7-4332-843a-2870c1064471" />

Figure 8: Wazuh Dashboard – Agents Overview (No Agents Connected)


<img width="1082" height="334" alt="image" src="https://github.com/user-attachments/assets/a550fe2d-0e16-4cd5-b095-4e623131e6fb" />

Figure 9: Wazuh Agent Deployment Page (Configuration Settings)

<img width="1080" height="290" alt="image" src="https://github.com/user-attachments/assets/94a48b9c-5940-469a-88fa-53c53ff846e8" />

Figure 10: Wazuh Agent Installation Command via PowerShell

<img width="1128" height="97" alt="image" src="https://github.com/user-attachments/assets/28c82448-5958-4af6-ab71-0e8c346bba01" />


Figure 11: Downloading and Installing Wazuh Agent on Windows

<img width="1128" height="98" alt="image" src="https://github.com/user-attachments/assets/59192566-d163-4967-a473-2d1a6ca86a80" />


Figure 12: Successful Download of Wazuh Agent Package

<img width="1125" height="129" alt="image" src="https://github.com/user-attachments/assets/8d9a20b3-0bf8-469f-a6c2-fd964b720f7b" />


Figure 13: Wazuh Agent Service Status (Already Running)

<img width="1127" height="326" alt="image" src="https://github.com/user-attachments/assets/789efed8-ca6e-43df-b7d4-98899fd228bb" />


Figure 14: Wazuh Dashboard – Active Agent Successfully Connected

<img width="1125" height="600" alt="image" src="https://github.com/user-attachments/assets/a3ec50c8-1e3a-4b11-853b-4ea740787acc" />


Figure 15: Wazuh Dashboard – Event Logs from Connected Windows 11 Agent (Log Visibility in Discover View)

<img width="1093" height="215" alt="image" src="https://github.com/user-attachments/assets/60ae37ff-8b92-4c72-847f-c40ad355d260" />


Figure 16: VMware Virtual Network Editor Configuration (VMnet0, VMnet1, VMnet8)

<img width="1108" height="525" alt="image" src="https://github.com/user-attachments/assets/3b7ea7a2-decd-46ef-8d5a-becc6f72b7e9" />

Figure 17: Kali Linux Virtual Machine Network Adapter Settings (Multi-NIC Configuration)

<img width="1125" height="544" alt="image" src="https://github.com/user-attachments/assets/d4fa6a73-cc11-4111-a9fd-d7dd0fdb73ff" />

Figure 18: Verifying Assigned IP Addresses on Kali Linux (ip a Output)

<img width="1125" height="609" alt="image" src="https://github.com/user-attachments/assets/9086f1bd-d338-44c4-80e9-450229d21e70" />

Figure 19: Routing Table and Interface Status Verification on Kali Linux

**Explanation:**

This step establishes the foundational SOC lab environment by preparing the Ubuntu system, deploying the Wazuh SIEM platform (192.168.1.26), and integrating a Windows 11 endpoint (192.168.1.40) as a monitored agent. The process begins with system updates, followed by automated installation of the full Wazuh stack, including the Manager, Indexer, and Dashboard. After verifying SIEM availability through the web interface, network connectivity between the SIEM and the endpoint is confirmed. The Wazuh agent is then deployed on the Windows machine, enabling centralized log collection and visibility within the SIEM. Additionally, Kali Linux is configured with multiple network interfaces (eth0: 192.168.214.128, eth1: 192.168.1.21) to support multi-network interaction, forming the basis for attack simulation and monitoring. This step ensures that the SIEM is fully operational, endpoints are successfully integrated, and the lab network is properly structured for subsequent security testing and analysis.

**Commands Used:**

```bash
# Update and upgrade Ubuntu system (Ensures system packages are up-to-date)
sudo apt update
sudo apt upgrade

# Download Wazuh installation script. (Download the official Wazuh installer)
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh

# Install Wazuh SIEM (All-in-One) (Deploys Wazuh Manager, Indexer, and Dashboard)
sudo bash wazuh-install.sh -a

# Access Wazuh Dashboard (Web interface for SIEM monitoring)
https://192.168.1.26:443

# Verify connectivity from Windows victim (Confirms network communication with SIEM)
ping 192.168.1.26

# Install Wazuh Agent on Windows (PowerShell) (Installs and starts Wazuh agent)
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.4-1.msi -OutFile $env:tmp\wazuh-agent.msi
msiexec.exe /i $env:tmp\wazuh-agent.msi /q WAZUH_MANAGER='192.168.1.26' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='Windows11'
NET START Wazuh

# Verify network configuration on Kali (Validates interfaces and connectivity)
ip a
ip route
nmcli device status
sudo ethtool eth0 | grep "Link detected"
sudo ethtool eth1 | grep "Link detected"
sudo ethtool eth2 | grep "Link detected"
```

---

### ➤ Step 2: Install and Validate Snort IDS on Kali Linux

<img width="1112" height="445" alt="image" src="https://github.com/user-attachments/assets/fe933456-f30d-465b-8c90-267388867254" />

Figure 20: Updating Kali Linux Package Repository and Upgrading System Packages

<img width="1121" height="511" alt="image" src="https://github.com/user-attachments/assets/ecf978b5-8c1a-4ec2-a06a-4a291db61055" />

Figure 21: Installing Snort IDS and Required Dependencies

<img width="1117" height="382" alt="image" src="https://github.com/user-attachments/assets/7b3d14a5-0c27-4e26-a0d4-b1252b61b47e" />

Figure 22: Verifying Snort Installation Version and Configuration

<img width="1107" height="400" alt="image" src="https://github.com/user-attachments/assets/166afdde-a50e-497c-b255-21687071c51c" />


Figure 23: Inspecting Snort Configuration Directory (/etc/snort)

<img width="1110" height="342" alt="image" src="https://github.com/user-attachments/assets/de552be7-2557-42f0-b3ea-f2dd185fc8aa" />

![Figure 24: Reviewing Default Snort Rule Sets](s)

**Explanation:**

This step installs and validates the Snort IDS on the Kali Linux machine to enable network traffic monitoring and threat detection. The system is prepared and Snort is installed with its required dependencies and rule sets. Installation is verified to ensure proper functionality, and configuration files along with detection rules are confirmed to be in place. This ensures Snort is correctly configured and ready to operate as an IDS within the SOC lab environment.

**Commands Used:**

```bash
sudo apt update
sudo apt upgrade
sudo apt install snort -y
snort -v
ls -all /etc/snort/
ls -all /etc/snort/rules/
```

---

### ➤ Step 3: Configure Snort IDS (snort.lua Customization)

<img width="1125" height="279" alt="image" src="https://github.com/user-attachments/assets/1a2b1399-e657-4d85-b19e-851b8a005345" />


Figure 25: Editing Snort Configuration File (snort.lua – HOME_NET Definition)

<img width="1125" height="344" alt="image" src="https://github.com/user-attachments/assets/3046d2a2-5516-449f-944c-7b2b78eeb90c" />

Figure 26: Adding Local Rule Path in Snort Configuration

<img width="1125" height="347" alt="image" src="https://github.com/user-attachments/assets/0c478eb4-9529-49b8-b458-61cfdddf2abf" />

Figure 27: Configuring Snort Alert Output (alert_fast and alert_json Modes)

**Commands Used:**

```bash
sudo nano /etc/snort/snort.lua
```

**Explanation:**

This step configures Snort alert output formats to support both real-time monitoring and SIEM integration. The alert_fast mode is enabled for quick, human-readable alerts during live analysis and debugging, while alert_json is configured to generate structured logs suitable for ingestion by the Wazuh SIEM. This ensures efficient alert visibility and seamless log correlation within the SOC environment.

---

### ➤ Step 4: Create Custom Detection Rules and Validate Snort Alerts

<img width="1151" height="417" alt="image" src="https://github.com/user-attachments/assets/1898a1bf-8cfd-4f85-9481-72b8f6c070ad" />


Figure 28: Adding Snort Local Rules File (local.rules)

<img width="1151" height="764" alt="image" src="https://github.com/user-attachments/assets/9a1273fc-9065-4b08-b28e-d3bc6cf1dc91" />

Figure 29: Validating Snort Configuration (Test Mode)

<img width="1125" height="150" alt="image" src="https://github.com/user-attachments/assets/b975f5d7-570a-48a6-88ca-05fb12002d40" />

Figure 30: Running Snort in IDS Mode (alert_fast Output)

<img width="1125" height="485" alt="image" src="https://github.com/user-attachments/assets/5919a178-392d-4d11-9c34-a4693932e075" />

Figure 31: Real-Time Snort Alerts During Nmap SYN Scan and ICMP Traffic Simulation

<img width="1124" height="940" alt="image" src="https://github.com/user-attachments/assets/11e86429-5b78-4d43-92d8-ccc254b91ece" />

Figure 32: Real-Time Snort Alerts in JSON Format (SYN Scan, ICMP Ping, and ICMP Flood Detection)

**Commands Used:**

```bash
sudo nano /etc/snort/rules/local.rules
sudo snort -T -c /etc/snort/snort.lua -i eth1
sudo mkdir -p /var/log/snort
sudo snort -c /etc/snort/snort.lua -i eth1 -l /var/log/snort
nmap -sS 192.168.1.40
ping -i 0.5 192.168.1.40
sudo cat /var/log/snort/alert_fast.txt
sudo cat /var/log/snort/alert_json.txt | jq
```

**Explanation:**

This step involves creating custom Snort detection rules and validating their effectiveness through simulated attack scenarios. Custom rules are defined to detect ICMP ping, ICMP flood (DoS), and TCP SYN scan activities, enabling tailored threat detection within the lab environment. The configuration is tested to ensure all rules are correctly loaded without errors, followed by running Snort in IDS mode for real-time monitoring. Attack simulations using Nmap and ICMP traffic successfully trigger the defined rules, generating alerts in both alert_fast and alert_json formats. The results confirm accurate detection, proper logging, and readiness for analysis and integration with the SIEM, validating Snort's functionality as an effective IDS in the SOC lab.

---

### ➤ Step 5: Integration of Snort IDS with Wazuh SIEM and End-to-End Alert Validation


<img width="1135" height="453" alt="image" src="https://github.com/user-attachments/assets/2dbcdbc8-df22-4dd4-aa95-40ae8862836c" />


Figure 33: Wazuh Dashboard – Linux Agent Deployment Configuration (DEB Package Selection)


<img width="1131" height="351" alt="image" src="https://github.com/user-attachments/assets/3ddecfa8-1649-472c-a7e1-3295276b3613" />


Figure 34: Generated Wazuh Agent Installation Command for Kali Linux

<img width="1133" height="459" alt="image" src="https://github.com/user-attachments/assets/69691698-635e-4ad4-adee-0d2ece4aa64c" />

Figure 35: Installing and Starting Wazuh Agent on Kali Linux (Snort Node)

<img width="1133" height="345" alt="image" src="https://github.com/user-attachments/assets/d7d58676-4041-4151-b263-252564d39fd8" />

Figure 36: Wazuh Dashboard – Kali Agent Successfully Connected (Snort_IDS_IPS Active)


<img width="1133" height="522" alt="image" src="https://github.com/user-attachments/assets/3f2c6176-06f8-4c0e-a3a7-06eb8ea8c897" />


Figure 37: Wazuh Discover View – Incoming Logs from Snort Agent (Pre-Parsing Stage)

<img width="1136" height="129" alt="image" src="https://github.com/user-attachments/assets/5a6122ad-9429-416a-ab50-79d27aa75ca3" />

Figure 38: Editing Wazuh Agent Configuration File (ossec.conf) on Kali Linux

<img width="1138" height="417" alt="image" src="https://github.com/user-attachments/assets/9cbf1414-945e-4d7c-baf4-ab5f61b4ef52" />

Figure 39: Configuring Client Buffer and Agent Enrollment Settings

<img width="1125" height="397" alt="image" src="https://github.com/user-attachments/assets/1e9ec1f9-f99a-48fb-acfa-d5f8551d4138" />

Figure 40: Adding Snort JSON Log Source to Wazuh Agent Configuration

<img width="1125" height="406" alt="image" src="https://github.com/user-attachments/assets/887ea821-3637-4bf3-acb6-6fb48217481f" />

Figure 41: Adjusting Log File Permissions and Restarting Wazuh Agent Service

<img width="1133" height="70" alt="image" src="https://github.com/user-attachments/assets/64d7355e-ff76-4970-b1b1-4809670f4222" />

Figure 42: Editing Wazuh Manager Configuration File (ossec.conf) on Ubuntu Server

<img width="1125" height="332" alt="image" src="https://github.com/user-attachments/assets/946e9535-fdcf-45b7-9459-1994821ba6ef" />

Figure 43: Enabling JSON Output Logging on Wazuh Manager

<img width="1125" height="369" alt="image" src="https://github.com/user-attachments/assets/17134668-2297-4ead-810b-5d6af72db558" />

Figure 44: Adding Additional Log Sources on Wazuh Manager

<img width="1125" height="52" alt="image" src="https://github.com/user-attachments/assets/732f3f53-b0a2-4b8c-bca1-8a423cc71031" />

Figure 45: Editing Local Rules File on Wazuh Manager (local_rules.xml)

<img width="1125" height="426" alt="image" src="https://github.com/user-attachments/assets/e52f7668-2a77-40fe-9b4c-a325677e4078" />


Figure 46: Creating Custom Wazuh Rules for Snort Alert Correlation (SYN Scan Detection)

<img width="1124" height="514" alt="image" src="https://github.com/user-attachments/assets/85407a9f-4053-4cb5-b3d8-16a412ba62a1" />

Figure 47: Validating Wazuh Rules Using wazuh-logtest Tool

<img width="1124" height="579" alt="image" src="https://github.com/user-attachments/assets/f447e097-c5f9-4108-98b9-9850b7a0faf6" />

Figure 48: Modifying Filebeat Ingest Pipeline to Handle Timestamp Parsing Issue

<img width="1125" height="219" alt="image" src="https://github.com/user-attachments/assets/d049f380-c8d4-4aa0-8cee-cdfb8019f300" />

Figure 49: Reloading Filebeat Pipelines and Restarting Wazuh Stack Services

<img width="1125" height="553" alt="image" src="https://github.com/user-attachments/assets/c1a8e251-3886-4b58-98eb-4133558805f7" />

Figure 50: Real-Time Attack Simulation (Nmap SYN Scan) and Snort Alert Generation

<img width="1125" height="547" alt="image" src="https://github.com/user-attachments/assets/631527e2-43da-4256-b409-379952b04d4d" />

Figure 51: Wazuh Discover View – Confirmed Detection of SYN Scan Alerts (Rule ID: 100201)

**Explanation:**

In this step, Snort IDS is fully integrated with Wazuh SIEM by deploying the Wazuh agent on the Kali machine and configuring it to monitor Snort JSON alert logs. The agent is tuned using buffer optimization and proper file permissions to ensure reliable log collection. On the Wazuh Manager side, JSON logging is enabled and custom detection rules are created to parse and classify Snort alerts, specifically identifying SYN scan attacks with high severity. The configuration is validated using wazuh-logtest, and a parsing issue related to timestamps is resolved by modifying the Filebeat ingest pipeline. After reloading the pipeline and restarting the SIEM stack, Snort is executed and an attack is simulated using Nmap. The generated alerts are successfully ingested, parsed, indexed, and visualized in Wazuh, confirming complete end-to-end integration and accurate threat detection within the SOC environment.

**Commands Used:**

```bash
# Install Wazuh Agent on Kali
sudo wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.14.4-1_amd64.deb
sudo WAZUH_MANAGER='192.168.1.26' WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='Snort_IDS_IPS' dpkg -i ./wazuh-agent_4.14.4-1_amd64.deb

# Start and Enable Agent
sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
sudo systemctl status wazuh-agent

# Edit Agent Configuration (Kali)
sudo nano /var/ossec/etc/ossec.conf

# Adjust Permissions for Snort Logs
sudo chmod 755 /var/log/snort
sudo chmod 644 /var/log/snort/alert_json.txt

# Restart Agent
sudo systemctl restart wazuh-agent
sudo systemctl status wazuh-agent

# Edit Wazuh Manager Configuration (Ubuntu)
sudo nano /var/ossec/etc/ossec.conf

# Edit Local Rules
sudo nano /var/ossec/etc/rules/local_rules.xml

# Validate Rules
sudo /var/ossec/bin/wazuh-logtest

# Edit Filebeat Pipeline
sudo nano /usr/share/filebeat/module/wazuh/alerts/ingest/pipeline.json

# Reload Pipelines and Restart Services
sudo filebeat setup --pipelines --modules wazuh
sudo systemctl restart filebeat
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard

# Run Snort IDS
sudo snort -c /etc/snort/snort.lua -i eth1 -l /var/log/snort &

# Simulate Attack
nmap -sS 192.168.1.40

# Verify Alerts in Elasticsearch
curl -k -u admin:PASSWORD "https://localhost:9200/wazuh-alerts-4.x-*/_search?q=rule.id:100201&pretty"
```

---

### ➤ Step 6: Deploying Snort as an Inline IPS Using NFQUEUE for Active Traffic Prevention

<img width="1130" height="1344" alt="image" src="https://github.com/user-attachments/assets/a0a8ff93-5304-4575-8f47-579a0c458a85" />


Figure 52: Virtual Machine Network Adapter Configuration for Inline IPS Deployment


<img width="1125" height="549" alt="image" src="https://github.com/user-attachments/assets/b95c59f0-bfcf-4467-822d-660aa79c07bb" />


Figure 53: System Preparation and Enabling IP Forwarding for Packet Routing

<img width="1125" height="839" alt="image" src="https://github.com/user-attachments/assets/f1f4b351-ee33-4740-a4e5-d4668623e23d" />


Figure 54: Identifying Victim Network Interface and Interface Index on Windows

<img width="1125" height="317" alt="image" src="https://github.com/user-attachments/assets/90ba301d-ff16-41e1-a782-299d89a12dab" />

Figure 55: Configuring Default Gateway on Victim to Route Traffic via IPS

<img width="1125" height="515" alt="image" src="https://github.com/user-attachments/assets/8e26362d-c3d8-41a4-b6c3-914ef3a1353e" />


Figure 56: Configuring iptables to Redirect Traffic into NFQUEUE for Inline Inspection

<img width="1125" height="397" alt="image" src="https://github.com/user-attachments/assets/2aeb30c1-2d8b-4686-93b2-cb01a3908ef9" />


Figure 57: Assigning Static IP Addresses and Disabling DHCP on IPS Interfaces

<img width="1125" height="514" alt="image" src="https://github.com/user-attachments/assets/fbe9349f-da35-43e8-b52f-762a4d5167fb" />


Figure 58: Verifying Network Interfaces and Routing Table on IPS Machine

<img width="1125" height="524" alt="image" src="https://github.com/user-attachments/assets/f69de4c0-d95d-456b-8205-04037153ced5" />


![Figure 59: Configuring Attacker Routing to Forward Traffic via IPS

<img width="1124" height="333" alt="image" src="https://github.com/user-attachments/assets/0fc7788c-b990-49b5-9db8-73875565a146" />


![Figure 60: Verifying Snort Installation and Version

<img width="1125" height="347" alt="image" src="https://github.com/user-attachments/assets/f8518d2b-dbe3-4145-9e1a-be67d1cbd6d7" />


![Figure 61: Configuring HOME_NET and EXTERNAL_NET in Snort

<img width="1125" height="468" alt="image" src="https://github.com/user-attachments/assets/f3e12f54-fe28-4b7a-bb70-c8a823e8a160" />


![Figure 62: Configuring DAQ Module for NFQUEUE Inline Mode

<img width="1125" height="460" alt="image" src="https://github.com/user-attachments/assets/b5729f9f-4790-4a11-81b3-fb8639746c1b" />


![Figure 63: Enabling IPS Mode and Loading Custom Detection Rules


<img width="1125" height="455" alt="image" src="https://github.com/user-attachments/assets/88c33dec-c2d1-4f67-a6df-7cfe41857c52" />

Figure 64: Configuring Snort Output Modules (alert_fast and alert_json)

<img width="1125" height="185" alt="image" src="https://github.com/user-attachments/assets/c5e456bb-1e13-4397-9b78-1839ad2bf1a8" />

Figure 65: Creating Log Directory and Setting Proper Permissions

<img width="1124" height="333" alt="image" src="https://github.com/user-attachments/assets/c8a81ce1-a7a7-43c0-a86a-f6b52b4cc02b" />

<img width="1124" height="333" alt="image" src="https://github.com/user-attachments/assets/36c4bd65-7a95-42ef-8194-58dabd9091fe" />

Figure 66: Creating Custom Snort Rules for Attack Detection and Prevention

<img width="1125" height="336" alt="image" src="https://github.com/user-attachments/assets/3cba2e62-3475-4c6e-9f07-b1221bb9c9cb" />

Figure 67: Running Snort in Inline IPS Mode with NFQUEUE

<img width="1125" height="432" alt="image" src="https://github.com/user-attachments/assets/ac223082-09f0-4c8d-852e-f3d51e55d72d" />

Figure 68: Validating Traffic Blocking from Attacker (ICMP Failure)

<img width="1125" height="454" alt="image" src="https://github.com/user-attachments/assets/1332e8cb-6c0e-43b2-aa74-790475df1e52" />

Figure 69: Verifying Snort Alerts Generated from Blocked Traffic

**Explanation:**

In this step, a dedicated Ubuntu machine is deployed as an inline Intrusion Prevention System (IPS) using Snort with NFQUEUE integration. The network architecture is adjusted to force all traffic between the attacker (Kali Linux) and the victim (Windows 11) to pass through the IPS by modifying routing tables and default gateways. IP forwarding is enabled to allow packet traversal, while iptables rules are configured to redirect traffic into NFQUEUE for deep inspection. Snort is configured in inline mode with custom detection rules targeting common attack techniques such as ICMP flooding, SYN scans, and brute-force attempts. Once operational, Snort actively inspects and blocks malicious traffic in real time. The setup is validated by launching attack simulations, where traffic is successfully dropped and corresponding alerts are generated, confirming effective inline prevention and proper IPS functionality within the SOC lab environment. (an Ubuntu-based inline IPS (192.168.1.5) is deployed using Snort with NFQUEUE between the attacker (192.168.214.129 – eth0) and victim (192.168.10.1 – VMnet1) networks via interfaces ens38 (192.168.214.133) and ens37 (192.168.10.133), where routing and iptables are configured to force traffic through the IPS, enabling real-time inspection and active blocking of malicious traffic such as ICMP and SYN scans.)

**Commands Used:**

```bash
# Update system packages (Ensures the system is up-to-date before configuration)
sudo apt update && sudo apt upgrade

# Enable IP forwarding (Allows the system to route packets between interfaces)
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
cat /proc/sys/net/ipv4/ip_forward

# Verify NFQUEUE support (Confirms that Snort supports inline mode via NFQUEUE)
snort --daq-list | grep nfq

# Install iptables persistence (Ensures iptables rules persist after reboot)
sudo apt install iptables-persistent -y

# Configure iptables rules for inline IPS (Redirects forwarded traffic into NFQUEUE for inspection)
sudo iptables -F
sudo iptables -X
sudo iptables -F FORWARD
sudo iptables -I FORWARD -i ens37 -o ens38 -j NFQUEUE --queue-num 5 --queue-bypass
sudo iptables -I FORWARD -i ens38 -o ens37 -j NFQUEUE --queue-num 5 --queue-bypass
sudo iptables -L FORWARD -v -n
sudo netfilter-persistent save

# Configure network interfaces (Assign static IPs and verify routing configuration)
sudo nano /etc/netplan/00-installer-config.yaml
sudo netplan apply
ip a
ip route

# Configure attacker routing (Forces traffic to pass through the IPS)
sudo ip route add 192.168.10.0/24 via 192.168.214.133 dev eth0
ip route show

# Verify Snort installation (Displays Snort version and confirms installation)
snort -V

# Edit Snort configuration (Configure networks, DAQ, and IPS mode)
sudo nano /etc/snort/snort.lua

# Prepare logging directory (Creates log files and assigns proper permissions)
sudo mkdir -p /var/log/snort
sudo chmod 755 /var/log/snort
sudo touch /var/log/snort/alert_fast.txt
sudo touch /var/log/snort/alert_json.txt
sudo chmod 644 /var/log/snort/*

# Create custom rules (Adds detection and prevention rules)
sudo nano /etc/snort/rules/local.rules

# Run Snort in inline IPS mode (Starts Snort in inline mode)
sudo snort -Q --daq nfq --daq-mode inline -c /etc/snort/snort.lua -i 5 -l /var/log/snort -A alert_fast

# Test attack traffic (Simulates attack traffic)
ping -I eth0 192.168.10.1

# View alerts (Verifies detection and blocking)
sudo cat /var/log/snort/alert_fast.txt
```

---

### ➤ Step 7: Attack Simulation and IPS Effectiveness Validation

<img width="1125" height="498" alt="image" src="https://github.com/user-attachments/assets/36f1ff70-0e0b-405c-8f94-ec4a877a2320" />


Figure 70: Nmap SYN Scan Behavior Before and After Enabling Snort IPS

<img width="1125" height="677" alt="image" src="https://github.com/user-attachments/assets/059b270e-bb99-4685-ae82-820bdd2734dc" />

Figure 71: ICMP Flood (DoS Attack) Before and After IPS Enforcement

<img width="1125" height="289" alt="image" src="https://github.com/user-attachments/assets/6c43c26b-fd2e-4822-8cab-69d90cf71a4e" />

Figure 72: SSH Brute Force Attack Execution Using Hydra
<img width="1125" height="547" alt="image" src="https://github.com/user-attachments/assets/32d1a08d-0e55-41dd-8e8f-2acdd377f72b" />


Figure 73: SSH Brute Force Attack Results (Before vs After IPS)

<img width="1125" height="539" alt="image" src="https://github.com/user-attachments/assets/d2915863-71a7-4a41-aaed-262ae1cb2782" />

Figure 74: Web Vulnerability Scanning Using Nikto (Before vs After IPS)

<img width="1125" height="521" alt="image" src="https://github.com/user-attachments/assets/f71a5557-fd48-4d81-af0c-4eface18e0b3" />

Figure 75: Nessus Scan Results Before Enabling Snort IPS

<img width="1124" height="289" alt="image" src="https://github.com/user-attachments/assets/2f4e8159-a494-4c7e-a024-2028de83095b" />


Figure 76: Nessus Scan Results After Enabling Snort IPS

<img width="1125" height="568" alt="image" src="https://github.com/user-attachments/assets/ce4145c7-0643-4877-a8e5-a15b213b593e" />

Figure 77: Snort IPS Alerts During Attack Simulation

**Explanation:**

This step evaluates the effectiveness of the deployed security architecture by simulating multiple real-world attack scenarios and comparing system behavior before and after enabling Snort in inline IPS mode.

Initially, reconnaissance and exploitation techniques such as SYN scanning, ICMP flooding, SSH brute force attacks, and vulnerability scanning using Nikto and Nessus are executed while the IPS is inactive. During this phase, all attacks successfully reach the target system, demonstrating normal network communication and full service exposure.

After enabling Snort IPS, a significant behavioral change is observed. Reconnaissance activities such as Nmap and Nessus scans fail to identify the target, ICMP flood traffic is completely dropped, and brute force attempts are disrupted due to connection limitations and filtering mechanisms. Additionally, web vulnerability scanning using Nikto is blocked, preventing further enumeration of the target system.

Simultaneously, Snort generates real-time alerts for each detected attack, including SYN scan and port scanning activities, providing full visibility into malicious behavior.

This confirms that the system has successfully transitioned from a passive detection model to an active prevention architecture, where threats are not only identified but also mitigated in real time. The results validate the effectiveness of Snort as an inline IPS within the SOC lab environment.

**Commands Used:**

```bash
# Nmap SYN Scan
nmap -sS -e eth0 192.168.10.1

# ICMP Flood (DoS Attack)
ping -i 0.05 -I eth0 192.168.10.1

# SSH Brute Force Attack using Hydra
hydra -l sshuser -P /usr/share/wordlists/rockyou.txt ssh://192.168.10.1 -t 4 -V -f

# Web Vulnerability Scanning using Nikto
nikto -h http://192.168.10.1

# Nessus Vulnerability Scanning (via Web Interface)
https://127.0.0.1:8834

# View Snort Alerts
sudo cat /var/log/snort/alert_fast.txt
```

---

## 6. Conclusion

This project successfully demonstrates the design and implementation of a fully integrated SOC Home Lab that combines monitoring, detection, and active prevention capabilities within a controlled virtual environment. The lab evolved from a basic SIEM deployment using Wazuh into a multi-layered security architecture incorporating both IDS and IPS functionalities through Snort. By enforcing traffic flow through an inline IPS, the environment effectively simulates real-world network defense strategies where inspection and control are centralized. Through multiple attack simulations using Kali Linux, the system demonstrated a clear transition from passive detection (log generation and alerting) to active prevention (real-time traffic blocking). Integration with the SIEM enabled centralized visibility, correlation, and analysis of security events across the entire environment. Overall, this lab validates the effectiveness of combining SIEM, IDS, and IPS technologies to build a realistic SOC workflow that covers the full lifecycle of cyber threat management:

**Detection → Analysis → Correlation → Response → Prevention**
