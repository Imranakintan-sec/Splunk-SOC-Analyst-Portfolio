# SOC Analyst | Cybersecurity | SIEM & Threat Detection

## About This Portfolio

Welcome to my Splunk SOC Analyst Portfolio.

This repository showcases my practical experience building and investigating a Security Information and Event Management (SIEM) lab using Splunk Enterprise.

Through hands-on exercises, I configured a centralised log collection environment, connected a Windows endpoint using Splunk Universal Forwarder, investigated authentication failures, analysed web server activity, and explored role-based access control (RBAC).

The portfolio demonstrates my practical understanding of log ingestion, security event investigation, SIEM administration, and basic security monitoring.

## Lab Environment

| Component | Description |
|---|---|
| SIEM Platform | Splunk Enterprise 10.6.2.5 |
| Endpoint | Windows 10 Pro (FLARE-VM) |
| Forwarder | Splunk Universal Forwarder 10.6.2.5 |
| Analysis System | Kali Linux |
| Virtualisation | Oracle VirtualBox |
| Network Configuration | NAT and Host-only networking |

## Projects and Investigations

### 01. Lab Architecture
Documented the virtual lab infrastructure, network configuration, IP addressing, and log collection workflow.

[View Lab Architecture](01-Lab-Architecture/lab-architecture.md)

### 02. Splunk Enterprise Installation
- Installed Splunk Enterprise on Kali Linux.
- Configured Splunk Web.
- Enabled the receiving port for forwarded data.
- Verified service startup and installation.

[View Installation Documentation](02-Splunk-Installation/splunk-installation.md)

### 03. Windows Universal Forwarder
- Installed Splunk Universal Forwarder on Windows 10.
- Configured Windows Event Log inputs.
- Established connectivity with the Splunk Enterprise server.
- Verified successful log forwarding and ingestion.

[View Screenshots](03-Universal-Forwarder/screenshots/)

### 04. Windows Event Log Investigation
- Investigated Windows Security Event ID 4625.
- Examined failed authentication attempts.
- Analysed account names, logon types, and failure reasons.
- Assessed the events using the available lab context.

[View Investigation Report](04-Windows-Event-Log-Analysis/failed-logon-investigation.md)

### 05. Web Log Investigation
- Uploaded and searched web server tutorial data.
- Investigated requests to `/productscreen.html`.
- Examined client IP addresses and URI paths.
- Compared request methods and reviewed activity from a selected client IP.

[View Investigation Report](05-Web-Log-Analysis/web-log-investigation.md)

### 06. Splunk Administration and RBAC
- Created custom Splunk roles.
- Explored role inheritance and capabilities.
- Configured index permissions.
- Created test users and evaluated their search access.
- Compared inherited permissions with a restricted role configuration.

[View RBAC Investigation](06-Splunk-Administration/rbac-investigation.md)

## Technical Skills Practised

- Splunk Enterprise Administration
- Security Information and Event Management (SIEM)
- Search Processing Language (SPL)
- Windows Event Log Analysis
- Splunk Universal Forwarder
- Log Collection and Ingestion
- Authentication Failure Investigation
- Web Server Log Analysis
- Role-Based Access Control (RBAC)
- Basic Security Monitoring
- Git and GitHub

## Future Lab Development

Planned extensions to this environment include:

- Sysmon endpoint telemetry and process monitoring.
- pfSense firewall log collection.
- Active Directory authentication monitoring.
- CrowdSec security event integration.
- Cross-source event correlation and detection engineering.

## Disclaimer

All investigations and configurations documented in this repository were performed as personal learning exercises in a controlled virtual lab environment. The projects demonstrate practical training and technical development, not professional employment experience or production SOC operations.