# Security Operations Center and Honeynet in Azure


## Introduction
In this project, I designed and deployed a mini honeynet in Microsoft Azure to simulate an enterprise environment exposed to potential attacks. Logs from various resources were ingested into a Log Analytics Workspace, which was then connected to Microsoft Sentinel to monitor, visualize, and respond to security events.

### The main objectives were to:

- Capture security telemetry from both Windows and Linux systems.
- Visualize attack activity through Sentinel attack maps.
- Trigger alerts and incidents based on malicious activity.
- Apply security hardening controls and measure their effectiveness by comparing pre- and post-hardening metrics.

### The experiment was performed in two phases:

- Insecure environment – resources deployed with minimal security controls.
- Hardened environment – security measures applied to reduce attack surface.

### The following data sources were collected:

- SecurityEvent – Windows Event Logs
- Syslog – Linux Event Logs
- SecurityAlert – Alerts generated in Log Analytics
- SecurityIncident – Incidents generated in Sentinel

## Architecture
The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Attack Maps Before Hardening
...

## Metrics Before Hardening
The following table shows the metrics we measured in our insecure environment for 24 hours:
| Start Time 8/20/2025, 13:11:13 PM
| Stop Time 8/21/2025, 13:11:13 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25301
| Syslog                   | 5147
| SecurityAlert            | 172
| SecurityIncident         | 226

## Attack Maps After Hardening
...

## Metrics After Hardening
The following table shows the metrics we measured in our insecure environment for 24 hours:
| Start Time 8/20/2025, 13:11:13 PM
| Stop Time 8/21/2025, 13:11:13 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0

## Results & Analysis
...

## Summary

This project demonstrates how a SOC team can leverage Azure + Microsoft Sentinel to:

- Detect, investigate, and respond to malicious activity.
- Visualize real-time attacks with attack maps.
- Validate the effectiveness of security hardening.
- Use KQL queries to measure and report key SOC metrics.

By simulating attacks and applying security controls, I showcased both defensive monitoring and incident response capabilities, which are essential for modern SOC and cloud security operations.

## KQL Queries
| Metric                                       | Query                                                                                                                                            |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Start/Stop Time                              | range x from 1 to 1 step 1<br>\| project StartTime = ago(24h), StopTime = now()                                                                  |
| Security Events (Windows VMs)                | SecurityEvent<br>\| where TimeGenerated>= ago(24h)<br>\| count                                                                                   |
| Syslog (Linux VMs)                           | Syslog<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                                         |
| SecurityAlert (Microsoft Defender for Cloud) | SecurityAlert<br>\| where DisplayName !startswith "CUSTOM" and DisplayName !startswith "TEST"<br>\| where TimeGenerated >= ago(24h)<br>\| count |
| Security Incident (Sentinel Incidents)       | SecurityIncident<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                               |

## Reference
...
