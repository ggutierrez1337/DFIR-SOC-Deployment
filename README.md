# End-to-End SOC Deployment and Incident Response

## Description

This project simulates a functional Security Operations Center (SOC) environment using the ELK stack and Elastic Defend for endpoint monitoring, a Fleet Server to manage Elastic agents, osTicket for incident management, a vulnerable Windows and SSH Server honeypot, Kali Linux to establish a Mythic C2 server. This Project includes alert creation for real-world brute-force attacks across geographic locations, investigation of command and control (C2) activities using Mythic, dashboard visualizations, and integration with ticketing (osTicket) for alert and response management. The setup covers the emulation of a SOC as well as real brute force attacks and C2 activity displaying an end-to-end workflow from detection to response across Windows and Linux environments.

---

## Network Topology
![image](https://github.com/user-attachments/assets/4fd0714f-ea61-4d26-9ecd-f8cb354aea3f)

---

## Tools and Utilities

- **ELK Stack (Elasticsearch, Logstash, Kibana)** – Event Searching, Log ingestion, alerting, and dashboarding  
- **Elastic Defend** – Endpoint Detection and Response (EDR)  
- **Fleet Server** – Agent management for Elastic integrations   
- **Mythic C2** – Command and control framework for payload delivery and exploitation  
- **osTicket** – Open-source ticketing system for alert tracking and response logging  
- **Ubuntu 22.04 & Windows Server** – Target hosts for RDP/SSH Brute Force Attacks and C2 agent deployment
- **Kali Linux** – Red team emulation and C2 payload deployment site  

---

## Environments

- **Elastic Server**: Ubuntu-based ELK deployment
- **Fleet Server**: Centralized management hub for Elastic Agents  
- **Windows Server 2019/2022**: Endpoint configured with Elastic Defend for detection of adversary activity  
- **Linux (Ubuntu)**: SSH server configured for brute force attack simulation  
- **Kali Linux**: Used to deploy and control Mythic C2 and simulate attacks  
- **osTicket Server**: Running on Linux for managing alerts and case documentation  

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



