# Building a SOC + Honeynet in Azure (Live Traffic)
![Honeynet](https://github.com/portfolioAustinT/Azure-SOC/assets/147944956/9bf28f90-524a-4aaf-80f6-4eeed05cddb0)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Public Internet](https://github.com/portfolioAustinT/Azure-SOC/assets/147944956/7b458f94-a3ca-4f65-9bb9-c140b2fa4807)


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
![Before - nsg-malicious-allowed-in](https://github.com/portfolioAustinT/Azure-SOC/assets/147944956/18638b6e-31ff-41ca-9d3f-15fda1d01c79)
![Before - Linux-Auth-Fail](https://github.com/portfolioAustinT/Azure-SOC/assets/147944956/14579138-135a-4113-ae00-235092c0dc6e)
![Before - windows-rdp-auth-fail](https://github.com/portfolioAustinT/Azure-SOC/assets/147944956/1a34ede9-917a-4e9f-91a7-bfd3f2128a86)
![Before - MSSQL-Auth-Fail](https://github.com/portfolioAustinT/Azure-SOC/assets/147944956/35492b67-8b54-4d87-9f39-bab1af0d2f78)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-04-14 16:30
Stop Time	2024-04-14 16:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 17152
| Syslog                   | 1966
| SecurityAlert            | 5
| SecurityIncident         | 292
| AzureNetworkAnalytics_CL | 1483

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-04-15 16:30
Stop Time	2024-04-15 16:30

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 24788
| Syslog                   | 14082
| SecurityAlert            | 5
| SecurityIncident         | 292
| AzureNetworkAnalytics_CL | 1483

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
