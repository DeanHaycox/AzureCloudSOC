# Building a SOC + Honeynet in Azure (Live Attack Traffic)
![Cloud Honeynet / SOC](https://imagizer.imageshack.com/img923/2794/uW8WGe.png)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://imagizer.imageshack.com/img923/6817/xRfj8M.png)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://imagizer.imageshack.com/img924/1751/5wNcFr.png)

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
![NSG Allowed Inbound Malicious Flows](https://imagizer.imageshack.com/img923/4112/qT4QOX.png)<br>
![Linux Syslog Auth Failures](https://imagizer.imageshack.com/img924/7361/F8D5lK.png)<br>
![Windows RDP/SMB Auth Failures](https://imagizer.imageshack.com/img922/5291/daGddo.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
start time 23/4/2023, 6:10:52.861 PM
Stop time 24/4/2023, 6:10:52.861 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 60668
| Syslog                   | 8337
| SecurityAlert            | 1
| SecurityIncident         | 273
| AzureNetworkAnalytics_CL | 320

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 30/4/2023, 12:03:52.219 PM
Stop Time	1/5/2023, 12:03:52.219 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 12590
| Syslog                   | 23
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

From the above metrics and the Azure Defender Secuirty posture recommendations. You will see in the below tables the reductions in percentages. 

![Azure Defender Security Posture (Before)](https://imagizer.imageshack.com/img923/144/xL3O1z.png)<br>
![Azure Defender Security Posture (after](https://imagizer.imageshack.com/img922/1859/UnPMOX.png)<br>
![Before Securing the environment](https://imagizer.imageshack.com/img924/7021/lYJYP6.png)<br>
![After Securing the environment](https://imagizer.imageshack.com/img922/323/cELO8X.png)<br>


## Security Controls
For this project I have used and tried to map the following controls below. Some of them don't directly map. You will find as I did, the majority of the controls do pretty much the exact same thing, but they may differ in how they are implemented and callled. For example, PCI-DSS as a strict implementation of there controls. 

- NIST 800-53
- CIS
- Cyber essentials
- ISO 27002
- PCI-DSS


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastially reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.


