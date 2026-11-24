# Splunk Enterprise Installation and Configuration

## 1. Project Overview

This document describes the installation and initial configuration of Splunk Enterprise on a Kali Linux virtual machine.

Splunk Enterprise serves as the central SIEM platform in this lab. It receives, indexes, and provides search capabilities for security logs forwarded from the Windows 10 FLARE-VM.

## 2. Lab Environment

| Component | Configuration |
|---|---|
| Operating System | Kali Linux |
| SIEM Platform | Splunk Enterprise |
| Splunk Version | 10.6.2.5 |
| Virtualisation Platform | Oracle VirtualBox |
| Host-only IP Address | 192.168.56.101 |
| Web Interface Port | 8000 |
| Receiving Port | 9997 |

## 3. Installation Process

Splunk Enterprise was installed on Kali Linux using the Debian package (`.deb`) provided by Splunk.

The following command was used:

```bash
sudo dpkg -i splunk*.deb
```

### Command Explanation

- `sudo`: Executes the command with administrative privileges.
- `dpkg`: Debian package management utility.
- `-i`: Installs the specified package.
- `splunk*.deb`: Selects a matching Splunk Debian package in the current directory.

## 4. Splunk Directory and Permissions

Splunk Enterprise was installed in:

```text
/opt/splunk
```

The directory ownership was configured using:

```bash
sudo chown -R splunk:splunk /opt/splunk
```

This assigns ownership of the Splunk installation directory and its contents to the dedicated `splunk` user and group.

## 5. Starting Splunk Enterprise

Splunk Enterprise was started using:

```bash
sudo -u splunk /opt/splunk/bin/splunk start
```

This starts Splunk under its dedicated service account rather than running the application as root.

## 6. Accessing Splunk Web

The Splunk Web interface was accessed through:

```text
http://localhost:8000
```

The web interface provides access to Splunk Search & Reporting, configuration settings, indexes, and other administration features.

## 7. Configuring the Receiving Port

Splunk Enterprise was configured to receive forwarded data from the Windows Universal Forwarder.

The receiving port was configured through Splunk Web.

### Configuration Steps

1. Open Splunk Web.
2. Navigate to Settings.
3. Select Forwarding and Receiving.
4. Under Receive Data, select Configure Receiving.
5. Select New Receiving Port.
6. Enter port `9997`.
7. Save the configuration.

### Receiving Port Details

| Setting | Value |
|---|---|
| Receiving Port | 9997 |
| Protocol | TCP |
| Data Source | Windows Universal Forwarder |
| Receiving Server | Kali Linux |
| Server IP | 192.168.56.101 |

The receiving port allows Splunk Enterprise to accept forwarded event data from the configured Universal Forwarder.