# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

This project involves implementing a comprehensive Security Information and Event Management (SIEM) system in Microsoft Azure concurrently deploying a honeynet. The SIEM system enables the Security Operations Center (SOC) to thoroughly monitor logs and track login activities to detect security threats in real time. By integrating insights from the honeynet and SIEM, the SOC can proactively identify and mitigate cybersecurity incidents, enhancing its defence mechanism. The honeynet will be intentionally exposed to the internet for 24 hours to capture real-world attacker traffic, contributing valuable data to refine cybersecurity defences. After the initial 24 hours, security controls will be implemented to fortify the environment. A subsequent 24-hour monitoring phase will compare incident rates, assessing the mitigation achieved through enhanced security measures.

By the project's conclusion, we will develop a heat map to visually represent the sources of incoming attacks.

Key metrics for analysis include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)
  
## Technologies
- Azure Virtual Network(VNet)
- Azure Virtual Machines(Windows and Linux OS VM)
- Microsoft SQL Server
- Network Security Group(NSG)
- Microsoft Sentinel
- Entra ID/Azure Active Directory
- Log Analytics Workspace(LAW)
- Azure Storage Account
- Azure Key Vault
- Microsoft Defender for Cloud
- Remote Desktop Protocol(RDP) for Windows Remote access control
- Command Line Interface/SecureShell(SSH) for Linux Remote Server Management
- PowerShell for Automation
- KQL(Kusto Query Language) for data query and analysis
- Azure Private Link - for security
- Azure Monitor

## Regulations
<a href="https://csrc.nist.gov/pubs/sp/800/37/r2/final">NIST 800-37 for Risk Management Framework	</a>

<a href="https://csrc.nist.gov/Projects/risk-management/sp800-53-controls/release-search#/families?version=5.1">NIST 800-53 for Security and Privacy Controls</a>

<a href="https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final">NIST 800-61 for Incident Handling</a>

![NIST-800-61-Incident Management](https://github.com/rasheedjimoh/AzureCloud-SOC/assets/157264080/cc4559f9-8467-4481-b91d-7d9e615cef44)



## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://imgur.com/ri2jWvV)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
