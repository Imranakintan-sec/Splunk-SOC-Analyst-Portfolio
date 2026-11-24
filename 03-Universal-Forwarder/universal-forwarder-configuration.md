# Splunk Universal Forwarder Installation and Configuration

## 1. Project Overview

This document describes the installation and configuration of Splunk Universal Forwarder on a Windows 10 Pro virtual machine.

The Universal Forwarder collects Windows Event Logs and forwards them to Splunk Enterprise running on a separate Kali Linux virtual machine.

This configuration enables centralised log collection and security monitoring within the lab environment.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Endpoint Operating System | Windows 10 Pro (FLARE-VM) |
| Forwarder | Splunk Universal Forwarder |
| Forwarder Version | 10.6.2.5 |
| Receiving Server | Kali Linux |
| Splunk Enterprise IP | 192.168.56.101 |
| Receiving Port | 9997 |
| Network | VirtualBox Host-only |

## 3. Installation

Splunk Universal Forwarder was installed on the Windows 10 virtual machine.

During installation:
- Administrator credentials were configured.
- The Deployment Server field was left blank because centralised deployment management was not required for this exercise.
- The receiving indexer was configured to use the Splunk Enterprise server.

The installation completed successfully.

## 4. Forwarding Configuration

The Universal Forwarder was configured to send collected data to Splunk Enterprise.

The receiving server was:

| Setting | Value |
|---|---|
| Forwarding Server | 192.168.56.101 |
| Receiving Port | 9997 |
| Protocol | TCP |

The following command was used to inspect the configured forwarding destination:

```powershell
.\splunk.exe list forward-server
```

The output showed:

```text
Active forwards:
    192.168.56.101:9997

Configured but inactive forwards:
    None
```

This confirmed that the Universal Forwarder had an active forwarding connection to the Splunk Enterprise receiving server.

## 5. Network Connectivity Verification

Connectivity to the Splunk receiving port was tested using PowerShell:

```powershell
Test-NetConnection -ComputerName 192.168.56.101 -Port 9997
```

The test returned:

```text
TcpTestSucceeded : True
```

This confirmed that the Windows virtual machine could establish a TCP connection to the Splunk receiving port.

## 6. Windows Event Log Input Configuration

The Universal Forwarder was configured to collect three Windows Event Log channels.

The configuration file was:

```text
C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf
```

The following configuration was used:

```ini
[WinEventLog://Security]
disabled = 0
index = main

[WinEventLog://System]
disabled = 0
index = main

[WinEventLog://Application]
disabled = 0
index = main
```

### Configuration Explanation

| Setting | Purpose |
|---|---|
| WinEventLog://Security | Collects Windows Security events. |
| WinEventLog://System | Collects Windows System events. |
| WinEventLog://Application | Collects Windows Application events. |
| disabled = 0 | Enables the input. |
| index = main | Sends collected events to the main index. |

The configuration enables collection from the three selected Windows Event Log channels.

## 7. Universal Forwarder Service Verification

The Windows service was checked using:

```powershell
Get-Service *Splunk*
```

The Splunk Forwarder service was confirmed to be running.

After configuration changes, the Universal Forwarder was restarted using:

```powershell
cd "C:\Program Files\SplunkUniversalForwarder\bin"
.\splunk.exe restart
```

The restart completed successfully.

## 8. Data Ingestion Verification

After configuring the Universal Forwarder, Splunk Web was opened on the Kali Linux virtual machine.

The following SPL search was executed:

```spl
index=main
```

The search returned Windows event data, confirming successful ingestion.

Additional searches were used to verify the Windows Security, System, and Application event sources.

## 9. Security and Operational Observations

This exercise demonstrated the importance of centralised log collection in a security monitoring environment.

The Universal Forwarder allowed Windows event data to be transferred to a separate Splunk Enterprise instance for analysis.

Key observations:

- The forwarding destination was active.
- TCP connectivity to port 9997 was successful.
- Windows Security, System, and Application inputs were enabled.
- The Splunk Forwarder service was running.
- Windows event data was searchable in Splunk Enterprise.

The Deployment Server field remained unconfigured because this exercise used direct forwarding rather than centrally managed forwarder deployment.

## 10. Screenshots and Evidence

The following screenshots are stored in the `screenshots/` directory:

- `universal_forwarder_installation_complete.png`
- `universal_forwarder_admin_configuration.png`
- `universal_forwarder_recieving_indexer_configuration.png`
- `universal_forwarder_connectivity_test.png`
- `universal_forwarder_input_configuration.png`
- `windows_universal_forwarder_service_running.png`

## 11. Conclusion

The Splunk Universal Forwarder was successfully installed and configured on the Windows 10 virtual machine.

Connectivity to the Splunk Enterprise receiving server was verified, Windows Event Log inputs were enabled, and forwarded event data was successfully received and indexed.

This exercise provided practical experience with endpoint log collection, forwarder configuration, network connectivity verification, and centralised SIEM data ingestion.