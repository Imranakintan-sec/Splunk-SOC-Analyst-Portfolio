# Splunk SOC Analyst Portfolio

## Overview

This repository documents my hands-on journey learning and practising Security Operations Centre (SOC) analysis using Splunk Enterprise.

The project demonstrates how I configured a Splunk environment, collected Windows Event Logs from a Windows endpoint, investigated authentication failures, and analysed web server logs.

It also serves as a record of my practical exercises, technical findings, troubleshooting experiences, and continuous learning in cybersecurity.

## Lab Environment

| Component | Description |
|---|---|
| SIEM Platform | Splunk Enterprise 10.6.2.5 |
| Endpoint | Windows 10 Pro (FLARE-VM) |
| Forwarder | Splunk Universal Forwarder 10.6.2.5 |
| Analysis System | Kali Linux |
| Virtualisation | Oracle VirtualBox |
| Network | NAT and Host-only networking |

## Practical Projects

### 1. Splunk Installation and Configuration
- Installed Splunk Enterprise on Kali Linux.
- Configured Splunk Web and the receiving port.
- Verified that Splunk services were operational.

### 2. Windows Universal Forwarder
- Installed Splunk Universal Forwarder on Windows 10.
- Configured Windows Event Log inputs.
- Established forwarding to the Splunk Enterprise instance.
- Verified connectivity and successful data ingestion.

### 3. Windows Event Log Analysis
- Analysed Windows Security Event Logs.
- Investigated Event ID 4625 (failed logon attempts).
- Examined account names, logon types, and failure reasons.

### 4. Web Log Analysis
- Uploaded tutorial web log data into Splunk.
- Investigated client IP addresses and requested URL paths.
- Used SPL searches to filter and examine web activity.

## Skills Practised

- Splunk Enterprise
- Security Information and Event Management (SIEM)
- SPL (Search Processing Language)
- Windows Event Log Analysis
- Log Ingestion and Forwarding
- Authentication Event Investigation
- Web Log Analysis
- Basic Security Monitoring
- Git and GitHub

## Future Improvements

Planned integrations and investigations include:

- Sysmon endpoint telemetry
- pfSense firewall logs
- Active Directory authentication logs
- CrowdSec security events
- Cross-source event correlation

## Disclaimer

This repository contains documentation of personal cybersecurity learning activities conducted in a controlled virtual lab environment. It does not represent professional employment experience or production security operations.