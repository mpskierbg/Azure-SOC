# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/agJzZNJ.jpeg)

## Introduction

In this project, I set up a mini honeynet in Azure and configured it to ingest log data from various sources into a Log Analytics workspace. This workspace is then utilized by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured certain security metrics in the insecure environment over a 24-hour period, applied security controls to harden the environment, and then measured the metrics again for another 24 hours. The results are shown below. The metrics we will present are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/QtX5blN.jpeg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/6WrASuP.jpeg)

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
NSG Allowed Inbound Malicious Flows
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/ItDxFOA.png)<br>
Linux Syslog Auth Failures
![Linux Syslog Auth Failures](https://i.imgur.com/sSqBv0g.png)<br>
Windows RDP/SMB Auth Failures
![Windows RDP/SMB Auth Failures](https://i.imgur.com/jsysWiA.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in my insecure environment for 24 hours:
Start Time 2024-05-15, 8:58:39 PM
Stop Time 2023-05-16, 8:58:39 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 33357
| Syslog                   | 4058
| SecurityAlert            | 44
| SecurityIncident         | 234
| AzureNetworkAnalytics_CL | 2287

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-05-21, 8:15:37 PM
Stop Time	2023-05-22, 8:15:37 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12751
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, we constructed a mini honeynet within Microsoft Azure, integrating diverse log sources into a Log Analytics workspace. Leveraging Microsoft Sentinel, we orchestrated the triggering of alerts and creation of incidents based on these logs. Our initial assessment measured security metrics in the unsecured environment, followed by a subsequent evaluation post-implementation of security controls. The results revealed a notable reduction in both security events and incidents, underscoring the efficacy of the applied security measures.

It's worth noting that had the network resources experienced heavier usage by regular users, the 24-hour period following security control implementation might have yielded a higher volume of security events and alerts.
