# TECHBIZ-ACADEMY-SAMUEL-WEEK1-TASK3-LAB-SETUP
Virtualized SOC Lab setup using Oracle VirtualBox, Kali, and Windows 10 on an isolated internal network (intnet).

---

# SOC LAB ARCHITECTURE & VIRTUALIZATION REPORT
## VIRTUALBOX HYPERVISOR, KALI/UBUNTU & WINDOWS ENDPOINT DEPLOYMENT
**WEEK 1 - TASK 3 | SOC ANALYST INTERNSHIP | TECHBIZ ACADEMY**

| Field | Details |
| :--- | :--- |
| **Analyst Name (SOC Intern)** | Adekoya Samuel |
| **Program / Batch** | TechBiz Academy — SOC Analyst Internship (B-504) |
| **Date** | 14 November 2026 |
| **Task Completed** | Week 1 · Task 3 — Lab Setup (VirtualBox + Kali/Ubuntu + Windows VM)<br>TryHackMe — Windows Fundamentals 1 (`tryhackme.com/room/windowsfundamentals1xbx`) |
| **Environment / Scope** | Isolated VirtualBox Internal Network (`intnet` — `192.168.56.0/24`) |
| **Authorization / Scope** | Personal isolated virtualized lab environment built strictly for defensive training |
| **Phases Covered** | Phase 1: Hypervisor & VM Provisioning<br>Phase 2: Internal Network & Static IP Configuration<br>Phase 3: Bidirectional Connectivity & Windows Fundamentals Lab |

---

## 1. Lab Scope & Safety Disclaimer
All virtualization deployments, network configurations, and connectivity tests documented in this report were executed strictly inside an isolated, offline Oracle VM VirtualBox Internal Network (`intnet`) hosted on my personal machine for defensive SOC analyst training.

---

## 2. Introduction
This report documents the end-to-end deployment of an isolated Security Operations Center (SOC) home lab using Oracle VM VirtualBox. Establishing a multi-OS virtualized environment provides the foundational infrastructure required for upcoming log collection, SIEM deployment, and live endpoint investigation exercises.

Both a Linux security workstation (Kali Linux / Ubuntu) and a Windows 10/11 evaluation endpoint were provisioned, placed onto a shared VirtualBox Internal Network (`intnet`), configured with static IPv4 addresses, and verified via bidirectional ICMP connectivity testing.

---

## 3. Tools & Virtual Infrastructure Used

| Component / Tool | Purpose |
| :--- | :--- |
| **Oracle VM VirtualBox** | Type-2 hosted hypervisor used to deploy, isolate, and snapshot guest virtual machines |
| **Kali Linux** | Analyst and attack-simulation Linux node (`192.168.56.10/24`) used for network diagnostics and future log analysis |
| **Windows 10 Evaluation VM** | Monitored enterprise endpoint (`192.168.56.20/24`) generating Windows Event Logs and firewall telemetry |
| **VirtualBox Internal Network (`intnet`)** | Isolated virtual switch enabling direct VM-to-VM communication with zero host or internet exposure |
| **Windows Defender Firewall (`netsh`)** | Configured inbound ICMPv4 Echo Request rules to permit diagnostic reachability testing |
| **ICMP Ping (`ping`)** | Command-line utility used on both Linux and Windows nodes to verify bidirectional layer-3 connectivity |
| **TryHackMe Platform** | Hands-on lab platform used to complete the *Windows Fundamentals 1* room |

---

## 4. Phase 1: Hypervisor & Virtual Machine Provisioning

### 4.1 VirtualBox Installation & Resource Allocation
Oracle VM VirtualBox was installed on the host workstation with hardware virtualization (`VT-x`/`AMD-V`) and Guest Additions support enabled. Two guest virtual machines were provisioned with dedicated compute and storage resources:

* **Kali Linux:** Allocated **2 vCPUs**, **2 GB RAM**, and a **80 GB** dynamically allocated VDI disk.
* **Windows 10 Evaluation VM:** Allocated **1 vCPU**, **3 GB RAM**, and a **50 GB** dynamically allocated VDI disk.
* **Baseline Snapshots:** Clean baseline snapshots (`Fresh Install State`) were captured for both VMs prior to network modification so the lab can be restored instantly.

![VirtualBox Manager Showing Kali/Ubuntu and Windows VMs Running](Screenshot%20(614).png)

---

## 5. Phase 2: Internal Network Setup & Connectivity Verification

### 5.1 VirtualBox Internal Network (`intnet`) Configuration
To isolate lab traffic from the physical host network while enabling direct communication between the analyst node and the Windows endpoint:

1. Opened **Settings > Network > Adapter 1** on both VMs.
2. Changed **Attached to** from `NAT` to **`Internal Network`** using the exact network name **`intnet`**.
3. Set **Promiscuous Mode** under Advanced settings to **`Allow VMs`**.

### 5.2 Static IPv4 Assignment & Firewall Rule
Because VirtualBox's Internal Network does not run a built-in DHCP server by default, static IPv4 addresses were assigned on the `192.168.56.0/24` subnet:

* **Kali Linux / Ubuntu VM (`eth0`):** `192.168.56.10` / Subnet Mask: `255.255.255.0`
* **Windows 10/11 Evaluation VM (`Ethernet`):** `192.168.56.20` / Subnet Mask: `255.255.255.0`
* **Windows Firewall ICMP Rule:** Enabled inbound ICMPv4 echo requests via elevated Command Prompt: `netsh advfirewall firewall add rule name="Allow ICMPv4" protocol=icmpv4:8,any dir=in action=allow`

### 5.3 Bidirectional Ping Verification
Executed `ping -c 4 192.168.56.20` from the Kali/Ubuntu terminal and `ping 192.168.56.10` from the Windows Command Prompt, confirming **0% packet loss** and `<1ms` latency across the isolated internal network.

![Bidirectional Ping Connectivity Confirmed Between Linux and Windows VMs](Screenshot%20(615).png)
![](Screenshot%20(616).png)

---

## 6. Phase 3: TryHackMe — Windows Fundamentals 1
To build core endpoint administration knowledge ahead of Windows log analysis, I completed the mandatory free-tier TryHackMe room **Windows Fundamentals 1** (`tryhackme.com/room/windowsfundamentals1xbx`), covering the Windows file system (NTFS), User Account Control (UAC), Control Panel utilities, Task Manager, and Windows Services.

![TryHackMe Windows Fundamentals 1 Room Completion Proof](Screenshots%20(617).png)

---

## 7. Security Best Practices & Lab Hardening
* **Strict Network Segmentation:** Always isolate test endpoints and attack-simulation VMs on an `Internal Network` (`intnet`) rather than `Bridged` mode to prevent accidental traffic leakage onto local physical networks.
* **Maintain Clean Snapshots:** Capture hypervisor snapshots before installing SIEM agents, Sysmon, or malware samples to guarantee rapid recovery.
* **Document IP Allocations:** Maintain a static IP inventory table for all lab nodes to streamline firewall rule auditing and log correlation.
* **Apply Least Privilege on Endpoints:** Keep User Account Control (UAC) enabled and audit local administrator accounts on Windows workstations.
* **Verify Host Firewall Policies:** Review Windows Defender Firewall inbound/outbound rules whenever troubleshooting missing telemetry or dropped diagnostic packets.

---

## 8. Conclusion
During Week 1 · Task 3 of my SOC Analyst Internship at TechBiz Academy, I successfully architected and verified an isolated virtualized SOC lab environment using Oracle VM VirtualBox.

In Phase 1 and Phase 2, I provisioned both a Linux monitoring/analyst node and a Windows 10 evaluation endpoint, isolated them on a shared `intnet` virtual switch with static IPv4 addressing, and validated bidirectional connectivity with zero packet loss. In Phase 3, I reinforced core Windows OS administrative concepts through TryHackMe's *Windows Fundamentals 1* room.

This completed lab infrastructure establishes a reliable, self-contained testbed ready for Week 2 SIEM deployment, log ingestion, and threat detection exercises.
