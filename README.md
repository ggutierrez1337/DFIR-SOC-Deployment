# SOC Automation and Threat Detection Lab

## Description

This project simulates a functional Security Operations Center (SOC) environment using the ELK stack and Elastic Defend for endpoint monitoring. It includes alert creation for brute-force attacks, investigation of command and control (C2) activities using Mythic, dashboard visualizations, and integration with ticketing (osTicket) for incident management. The setup covers the end-to-end workflow from detection to response across Windows and Linux environments.

---

## Network Topology
![image](https://github.com/user-attachments/assets/4fd0714f-ea61-4d26-9ecd-f8cb354aea3f)

---

## Tools and Utilities

- **Elastic Stack (ELK)** – Log ingestion, alerting, and dashboarding  
- **Elastic Defend** – Endpoint Detection and Response (EDR)  
- **Fleet Server** – Agent management for Elastic integrations  
- **Mythic C2** – Command and control framework for payload delivery and management  
- **osTicket** – Open-source ticketing system for alert tracking and response documentation  
- **Ubuntu 22.04 & Windows Server** – Hosts for agents and targets  
- **Kibana** – Visualization and search interface for ELK  
- **Kali Linux** – Red team emulation and payload launch point  

---

## Environments

- **Elastic Server**: Ubuntu-based ELK deployment with Fleet Server installed for managing agents  
- **Windows Server 2019/2022**: Endpoint configured with Elastic Defend for detection of adversary activity  
- **Linux (Ubuntu)**: SSH server configured for brute force attack simulation  
- **Kali Linux**: Used to deploy and control Mythic C2 and simulate attacks  
- **osTicket Web Interface**: Running on Linux for managing alerts and case documentation  

---

## Attacks

- **SSH Brute Force**: Simulated attack on an Ubuntu SSH server  
- **RDP Brute Force**: Simulated unauthorized access attempts to Windows RDP  
- **Mythic Payload Execution**:
  - File download from victim  
  - Command execution from C2  
  - Defender/EDR evasion attempts  
  - Credential harvesting  


---

## Investigative Response

- **SSH & RDP Alerts**:
  - Alerts triggered from failed authentication attempts  
  - Investigated using Kibana dashboards and `auth.log` / Windows event logs  
  - Confirmed if attempts were successful  
  - Blocklisted IPs and updated ticket status in osTicket  

- **Mythic C2 Activity**:
  - Tracked via Elastic Defend detections and Kibana queries  
  - Confirmed suspicious service creation (`svhost-dfir`)  
  - Process lineage and file downloads reviewed  


- **Elastic Defend Integration**:
  - Endpoint visibility and health validated  
  - Real-time response options tested (e.g., isolate host, kill process)  

---



