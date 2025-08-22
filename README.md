# Security Operations Center and Honeynet in Azure

<img width="946" height="636" alt="Untitled Diagram2 drawio" src="https://github.com/user-attachments/assets/8f5a1919-7cdb-463c-9a37-12f5dab06d36" />


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

## Metrics Before Hardening
The following table shows the metrics we measured in our insecure environment for 24 hours:
</br>
| Start Time 8/20/2025, 13:11:13 PM
</br>
| Stop Time 8/21/2025, 13:11:13 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25301
| Syslog                   | 5147
| SecurityAlert            | 172
| SecurityIncident         | 226

## Metrics After Hardening
The following table shows the metrics we measured in our insecure environment for 24 hours:
</br>
| Start Time 8/21/2025, 13:36:59 PM
</br>
| Stop Time 8/22/2025, 13:36:59 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 1621
| Syslog                   | 0
| SecurityAlert            | 5
| SecurityIncident         | 5

## Results & Analysis

| Results                                       | Change after security environment                                                                                                                                            |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| Security Events (Windows VMs)                | **93.6% (from 25,301 → 1,621)** The drastic reduction shows that Windows systems were hardened, eliminating most brute-force attempts. |
| Syslog (Linux VMs)                           | **100% (from 5,147 → 0)** Indicates that SSH brute-force and other Linux-targeted attempts were fully blocked after applying hardening. |
| SecurityAlert (Microsoft Defender for Cloud) | **97.1% (from 172 → 5)** Defender alerts decreased significantly, confirming that security posture improvements (patching, NSG, key vault access restrictions) prevented most malicious detections.  |
| Security Incident (Sentinel Incidents)       | **97.8% (from 226 → 5)** The SOC incident queue dropped dramatically, showing Sentinel had far fewer true/false positives to escalate after attack surface reduction.             |

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
| SecurityAlert (Microsoft Defender for Cloud) | SecurityAlert<br>\| where DisplayName !startswith "CUSTOM" and DisplayName !startswith "TEST"<br>\| where TimeGenerated >= ago(24h)<br>\| count  |
| Security Incident (Sentinel Incidents)       | SecurityIncident<br>\| where TimeGenerated >= ago(24h)<br>\| count                                                                               |

## Reference
- [GitHub](https://github.com/kphillip1/azure-soc-honeynet/blob/main/README.md)
- [Youtube](https://www.youtube.com/watch?v=mOjbD7FkUUI)
