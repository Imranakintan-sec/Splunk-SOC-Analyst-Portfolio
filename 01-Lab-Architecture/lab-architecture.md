# Splunk SOC Analyst Lab Architecture

## 1. Project Overview

This document describes the architecture of my personal Security Operations Centre (SOC) lab. The lab was built using Oracle VirtualBox, Kali Linux, Windows 10 FLARE-VM, Splunk Enterprise, and Splunk Universal Forwarder.

The primary objective is to collect Windows event logs from the Windows virtual machine, forward them to Splunk Enterprise running on Kali Linux, and investigate security events using Splunk Search Processing Language (SPL).

## 2. Lab Components

| Component | Operating System / Tool | Role |
|---|---|---|
| SIEM Server | Kali Linux | Hosts Splunk Enterprise |
| Endpoint | Windows 10 FLARE-VM | Generates Windows event logs |
| Log Forwarder | Splunk Universal Forwarder | Collects and forwards Windows logs |
| Virtualisation Platform | Oracle VirtualBox | Runs both virtual machines |
| SIEM Platform | Splunk Enterprise 10.6.2.5 | Receives, indexes, and searches logs |
| Forwarding Port | TCP 9997 | Receives forwarded event data |

## 3. Network Architecture

The lab uses Oracle VirtualBox with two network adapters configured for each virtual machine.

### 3.1 NAT Network Adapter

The NAT adapter provides internet connectivity to the virtual machines when required, while the Host-only adapter provides private communication between the two lab machines.

### 3.2 Host-only Network Adapter

The Host-only adapter provides private communication between the Kali Linux and Windows virtual machines.

The Host-only network is used for Splunk log forwarding between the Windows Universal Forwarder and Splunk Enterprise.

### 3.3 IP Address Configuration

| Virtual Machine | Role | Host-only IP Address |
|---|---|---|
| Kali Linux | Splunk Enterprise Server | 192.168.56.101 |
| Windows 10 FLARE-VM | Splunk Universal Forwarder | 192.168.56.106 |

### 3.4 Splunk Communication

The Windows Universal Forwarder sends collected Windows event logs to Splunk Enterprise on Kali Linux.

- **Source:** Windows 10 FLARE-VM
- **Destination:** Kali Linux (`192.168.56.101`)
- **Protocol:** TCP
- **Receiving Port:** 9997

## 4. Log Collection and Data Flow

The lab uses a forwarder-based log collection architecture.

The workflow is as follows:

1. Windows 10 generates security, system, and application events.
2. Splunk Universal Forwarder collects the configured Windows Event Logs.
3. The Universal Forwarder forwards the collected events over TCP port 9997.
4. Splunk Enterprise on Kali Linux receives the forwarded events.
5. Splunk indexes the events, making them available for searching and security investigations.

### 4.1 Configured Windows Event Logs

The Universal Forwarder was configured to collect:

- Windows Security Event Logs
- Windows System Event Logs
- Windows Application Event Logs

### 4.2 Splunk Index

The Windows Event Log inputs were configured to send collected events to the `main` index in Splunk Enterprise.

### 4.3 Verification

Successful log forwarding was verified using Splunk Search & Reporting with the following SPL query:

```spl
index=main
```

The search returned Windows event data, confirming successful communication between the Universal Forwarder and Splunk Enterprise.

A successful TCP connectivity test from Windows FLARE-VM to Kali Linux on port 9997 was also confirmed using PowerShell's Test-NetConnection command.

## 5. Lab Architecture Diagram

```text
             ORACLE VIRTUALBOX
       ______________________________
      |                              |
      |       NAT NETWORK            |
      |     Internet Connectivity    |
      |______________________________|

      |______________________________|
      |                              |
      |       HOST-ONLY NETWORK      |
      |                              |
      |  _________________________   |
      | |                         |  |
      | |     WINDOWS 10 PRO      |  |
      | |       FLARE-VM          |  |
      | |                         |  |
      | |  Windows Event Logs     |  |
      | |           |             |  |
      | |           v             |  |
      | |  Splunk Universal       |  |
      | |  Forwarder              |  |
      | |_________________________|  |
      |             |                |
      |             | TCP 9997       |
      |             v                |
      |  _________________________   |
      | |                         |  |
      | |       KALI LINUX        |  |
      | |                         |  |
      | |   Splunk Enterprise     |  |
      | |                         |  |
      | |   Receive and Index     |  |
      | |   Windows Events        |  |
      | |_________________________|  |
      |______________________________|

       Host-only IP addresses:

       Windows: 192.168.56.106
       Kali:    192.168.56.101
```

## 6. Security Considerations

The lab uses a Host-only network adapter for private communication between the virtual machines. Although Host-only networking provides private communication between the lab virtual machines, the NAT adapter still provides an external network path. Additional isolation measures are therefore necessary when conducting potentially risky security experiments.

Security considerations include:

- Using Host-only networking for communication between the lab machines.
- Avoiding Bridged networking when conducting isolated security experiments.
- Disabling unnecessary shared folders and clipboard sharing.
- Taking virtual machine snapshots before major configuration changes.
- Limiting access to Splunk receiving ports to trusted lab systems.
- Using legitimate test activity and controlled security events for investigations.

## 7. Conclusion

This lab demonstrates a basic SIEM architecture using Splunk Enterprise and Splunk Universal Forwarder.

Windows event logs are collected from the Windows 10 FLARE-VM and forwarded to Splunk Enterprise running on Kali Linux. The collected data is indexed and investigated using SPL.

This architecture provides a practical environment for developing security monitoring, log analysis, event investigation, and SIEM administration skills.