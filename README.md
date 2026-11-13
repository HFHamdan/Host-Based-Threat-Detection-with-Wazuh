# Host-Based IDS Wazuh

![img alt](https://github.com/HFHamdan/Host-Based-IDS-Wazuh/blob/main/Wazuh.png)

# Overview
This project demonstrates end-to-end threat simulation, telemetry pipeline engineering, and custom detection engineering using Wazuh SIEM, Sysmon, and Atomic Red Team. The goal is to simulate realistic adversary behavior aligned with the MITRE ATT&CK framework within an isolated lab environment, capture raw host telemetry, and build custom XML detection logic to alert on malicious activity.


## Tools & Setup

- Wazuh (manager on Ubuntu VM)
- VMware Workstation (bridge adapter)
- Windows 11 as monitored endpoint
- Atomic Red Team (see MITRE ATT&CK) 
- Draw.io

## Lab Architecture 

'''bash
+-----------------------------------------------------------------------+
|                             VIRTUAL LAB                               |
|                                                                       |
|   +-----------------------+               +-----------------------+   |
|   |    Wazuh Manager      |               |     Windows Target    |   |
|   |     (Ubuntu VM)       |<--------------|      (Windows VM)     |   |
|   |  - Centralized SIEM   |  Port 1514/    |  - Wazuh Agent        |   |
|   |  - Custom XML Rules   |  WinEventLog  |  - Sysmon 15.0        |   |
|   |  - Alerting & Indexing|               |  - Invoke-AtomicRed   |   |
|   +-----------------------+               +-----------------------+   |
+-----------------------------------------------------------------------+

<!--![imag_alt](https://github.com/HFHamdan/Host-Based-IDS-Wazuh/blob/main/IDS_Arch.drawio.png) -->
<!-- [ Devices/Nodes:
Ubuntu VM box — labeled "Wazuh Manager"
Windows 11 box — labeled "Monitored Endpoint (Wazuh Agent)"

Connections:

An arrow between them labeled "Agent Communication (TCP 1514)" 
Another arrow labeled "Dashboard Access (HTTPS 443)"

Access point:

Windows 11 machine accessing the Wazuh dashboard via browser — label it "Web UI Access"

Optional but nice:

A small Atomic Red Team logo or label on the Windows 11 box indicating that's where simulated attacks originate
VMware Workstation as the outer container/boundary around the Ubuntu VM ] -->


- Wazuh manager deployed on Ubuntu VM (VMware Workstation, bridge adapter)
- Windows 11 VM configured as monitored agent
- Agent deployment handled via WinSCP due to clipboard limitations between host and VM
- Network connectivity verified before agent activation


# Simulated Threat Scenarios

- T1059 – Command and Scripting Interpreter (PowerShell specifically, T1059.001)
Extremely common in real attacks and real SOC alerts. Simulate a suspicious PowerShell execution (e.g., encoded command).
- T1053 – Scheduled Task/Job
Classic persistence technique. Very visible in Windows Event Logs (Event ID 4698), which Wazuh picks up cleanly.
- T1003 – OS Credential Dumping
High-value alert type  this is the kind of thing SOC analysts get paged for.
- T1082 – System Information Discovery
Low-noise, easy to simulate, good for "low severity but worth logging" alerts, not critical.
- T1112 – Modify Registry
Good  to show persistence/defense evasion coverage.

