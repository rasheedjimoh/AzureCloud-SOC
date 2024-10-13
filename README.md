# Honeynet: Real-Time Azure Cloud Defense SOC

# Infrastructure
![architect1](https://github.com/user-attachments/assets/9eec6476-ec9e-4aac-a3a6-0cf8c7eca70f)


# Cloud SOC
![Cloud Honeynet / SOC](https://i.imgur.com/M97KMNu.jpg)

## Introduction

This project involves implementing a comprehensive Security Information and Event Management (SIEM) system in Microsoft Azure concurrently deploying a honeynet. The SIEM system enables the Security Operations Center (SOC) to thoroughly monitor logs and track login activities to detect security threats in real time. By integrating insights from the honeynet and SIEM, the SOC can proactively identify and mitigate cybersecurity incidents, enhancing its defence mechanism. The honeynet will be intentionally exposed to the internet for 24 hours to capture real-world attacker traffic, contributing valuable data to refine cybersecurity defences. After the initial 24 hours, security controls will be implemented to fortify the environment. A subsequent 24-hour monitoring phase will compare incident rates, assessing the mitigation achieved through enhanced security measures.

By the project's conclusion, we will develop a heat map to visually represent the sources of incoming attacks.

Key metrics for analysis include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Workspace Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet/Network Security Groups logs)
  
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
Regulatory compliance is crucial in IT and security for legal adherence, data protection, and trust. It ensures strong cybersecurity practices, mitigates risks, and avoids legal consequences, fostering a secure and trustworthy environment.

<a href="https://csrc.nist.gov/pubs/sp/800/37/r2/final">NIST 800-37 for Risk Management Framework	</a>

<a href="https://csrc.nist.gov/Projects/risk-management/sp800-53-controls/release-search#/families?version=5.1">NIST 800-53 for Security and Privacy Controls</a>

<a href="https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final">NIST 800-61 for Incident Handling</a>

![NIST-800-61-Incident Management](https://github.com/rasheedjimoh/AzureCloud-SOC/assets/157264080/cc4559f9-8467-4481-b91d-7d9e615cef44)



## Architecture Before Hardening / Security Controls
All the resources were deployed with direct exposure to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open. Similarly, all other resources were also deployed with public endpoints that were visible to the internet. As a result, there was no need for Private Endpoints.
![Architecture Diagram](https://i.imgur.com/Z6Tr6OH.png)

## Remediation process using NIST 800-61 Incident Handling Guide
We implemented SC-7 Boundary Protection Control of NIST 800-53 by doing the following:

1. Network Security Groups (NSG): Restrict traffic exclusively to your admin workstation.
2. Built-in Firewall of Resources: Implement additional security layers. For Windows, Windows Defender Firewall has been implemented.
3. Private Endpoints: Incorporate into your network architecture for augmented protection.
4. Network Security Groups (NSG) for Subnet: Add to the subnet to further fortify security measures.
5. Activated and enabled NIST Compliance in Azure.

## Architecture After Hardening / Security Controls
All resources are protected, as illustrated below, with an added layer of security from the Network Security Group (NSG) attached to the subnet.
![Architecture Diagram](https://i.imgur.com/vvbiDo9.png)


## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/8Lf2sxY.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/BgJg9Jr.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/aIlLyxn.png)<br>



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-01-18 11:07:47
Stop Time 2024-01-19 11:07:47

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 30145
| Syslog                   | 3368
| SecurityAlert            | 1
| SecurityIncident         | 180
| AzureNetworkAnalytics_CL | 4392

## Attack Maps After Hardening / Security Controls

```All map queries returned no results due to no instances of malicious activity for the 24 hours after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 2024-01-20 06:05:28
Stop Time	2024-01-21 06:05:28

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 5508
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In conclusion, the impact of our security enhancements is evident in the substantial improvements across key metrics. Strengthening our security framework resulted in an 81.73% decrease in security events for Windows VMs and a 99.85% reduction in syslog events for Linux VMs. Remarkably, security alerts from Microsoft Defender for Cloud and security incidents in Sentinel Incidents showed a 100% elimination. The mitigation of NSG inbound malicious flows allowed demonstrated a robust 100% reduction, highlighting the efficacy of our proactive security approach.

Throughout the project, we implemented a mini honeynet in Microsoft Azure, integrating log sources into a Log Analytics workspace. Microsoft Sentinel was leveraged to trigger alerts and create incidents based on ingested logs. Metric evaluations before and after the application of security controls showed a significant reduction in security events and incidents, showcasing the tangible effectiveness of these measures.

It's crucial to acknowledge that if network resources were heavily utilized by regular users, there could have been an increase in security events and alerts within the 24 hours following the implementation of security controls. These outcomes affirm the success of our security enhancements, ensuring a more resilient and fortified digital environment.
