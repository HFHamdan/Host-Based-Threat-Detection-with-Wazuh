# Host-Based IDS Wazuh

![img alt](https://github.com/HFHamdan/Host-Based-IDS-Wazuh/blob/main/Wazuh.png)

# Overview
This project demonstrates end-to-end threat simulation, telemetry pipeline engineering, and custom detection engineering using Wazuh SIEM, Sysmon, and Atomic Red Team. The goal is to simulate realistic adversary behavior aligned with the MITRE ATT&CK framework within an isolated lab environment, capture raw host telemetry, and build custom XML detection logic to alert on malicious activity.


## Tools & Setup

- Wazuh (manager on Ubuntu VM)
- VMware Workstation (bridge adapter)
- Windows 11 as monitored endpoint
- Atomic Red Team (see MITRE ATT&CK) 


## Lab Architecture 

```bash 
+-----------------------------------------------------------------------+
|                             VIRTUAL LAB                               |
|                                                                       |
|   +-----------------------+               +-----------------------+   |
|   |    Wazuh Manager      |               |     Windows Target    |   |
|   |     (Ubuntu VM)       |<--------------|      (Windows VM)     |   |
|   |  - Centralized SIEM   |  Port 1514/    |  - Wazuh Agent       |   |
|   |  - Custom XML Rules   |  WinEventLog  |  - Sysmon             |   |
|   |  - Alerting           |               |  - Invoke-AtomicRed   |   |
|   +-----------------------+               +-----------------------+   |
+-----------------------------------------------------------------------+
```
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

- SIEM / Log Aggregator: Wazuh Manager on Linux VM.
  
- Target Host: Windows VM isolated with snapshots enabled.

- Telemetry Source: Microsoft Sysmon (System Monitor) + Windows Event Logs.

- Adversary Simulation: Atomic Red Team (Invoke-AtomicRedTeam).



# Simulated Threat Scenarios

| Technique ID | Technique Name                                | Tactic            |
|--------------|-----------------------------------------------|-------------------|
| T1003        | OS Credential Dumping                         | Credential Access |
| T1053        | Scheduled Task                                | Persistence       |
| T1059.001    | Command and Scripting Interpreter, PowerShell | Execution         |
| T1082        | System Information Discovery                  | Discovery         |
| T1112        | Modify Registry                               | Defense Evasion   |

```bash
├── README.md                  <- Main project documentation & overview
├── config/                    <- Sysmon & Wazuh agent configuration files
│   ├── ossec.conf
│   └── sysmonconfig.xml
├── rules/                     <- Custom Wazuh XML detection rules
│   └── local_rules.xml
└── docs/                      <- Detailed technique writeups & walkthroughs
    ├── T1082_discovery.md
    ├── T1112_registry.md
    ├── T1059.001_powershell.md
    ├── T1053_scheduled_task.md
    └── T1003_credential_dumping.md
```

