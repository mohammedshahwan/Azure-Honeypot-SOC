# Honeypot SOC Project with Azure: LIVE Cyber Attacks

## Introduction
In this project, I created exposed honeypot VMs to simulate vulnerable machines that would attract attackers from around the world using Microsoft Azure. The honeypots were set up to collect and log the geographic coordinates of any attackers that attempted to access the machines. The log sources were then ingested into Log Analytics Workspace and fed to Microsoft Sentinel to create attack maps of the incidents. Microsoft Defender for Cloud generated security alerts in response to the incidents, describing the incident and giving insight on the attack pattern while suggesting mitigation and prevention strategies. The goal of this project is to demonstrate best security practices, incident response, and the impacts of environment hardening.

## Technologies Utilized
- Virtual Machines (Windows & Linux)
- Azure Network Security Groups (NSGs)
- Log Analytics Workspace
- Microsoft Sentinel (Azure cloud-native SIEM)
- Microsoft Defender for Cloud
- Kusto Query Language (KQL)
- PowerShell (For gathering attack logs)
- Bash (For gathering attack logs)

## Methodology for Incident Response
[NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) for Incident Handling Guide

- <b>Preparation:</b> The virtual machines were connected to Log Analytics Workspace to ingest the logs. Microsoft Sentinel and Microsoft Defender for Cloud were configured.
- <b>Detection & Analysis:</b> Attack attempt logs were sent through Log Analytics Workspace to Sentinel to display on the attack map. Microsoft Defender for Cloud generated security alerts with information on the attack patterns.
- <b>Containment, Eradication, & Recovery:</b> Contain and verify if any of the virtual machines were breached. (Non-breach Situation) Hardening measures were implemented to prevent the incidents from escalating and to prevent unauthorized access to the VMs.
- <b>Post-Incident Activity:</b> No machines were breached and the newly hardened environments completely prevented any unauthorized access attempts to the VMs. A lessons-learned review of the incident was carried out.

## Before Hardening
- In the first stage of this project, all resources were deployed while completely exposed to the public internet. The virtual machines had their built-in firewalls and Network Security Groups (NSGs) wide open, leaving the VMs completely exposed to attackers. The machines were left exposed for 24 hours, gathering and logging information from the attackers. The log data was extracted and sorted using a KQL query to produce the following attack maps:

<br />

<b>This attack map showcases numerous SSH authentication failures experienced by the Linux server I had deployed.</b>

![image](https://github.com/mohammedshahwan/Azure-Honeypot-SOC/blob/main/assets/Linux%2024hr%20Failed%20SSH%20Map.png)

<br />

<b>This attack map displays various RDP and SMB authentication failures directed at the Microsoft Windows VM, showcasing the persistent attempts of global attackers to bruteforce these protocols.</b>

![image](https://github.com/mohammedshahwan/Azure-Honeypot-SOC/blob/main/assets/MS%2024hr%20Failed%20RDP-SMB%20Map.png)

<br />

Microsoft Defender for Cloud also generated security alerts and provided insight on some of the attacks that had occurred, as is shown below.

![image](https://github.com/mohammedshahwan/Azure-Honeypot-SOC/blob/main/assets/MDfC%20Security%20Alert.png)

<br />

## After Hardening

<b>In the last stage of this project, after the 24 hour capture, I implemented a series of hardening measures to improve the overall security posture of the honeypot VMs' environments.</b>

- <b>Built-in Firewalls:</b> I configured the built-in firewalls on each of the virtual machines to restrict access from unauthorized users, minimizing the attack surface.
- <b>Network Security Groups (NSGs):</b> I reconfigured the NSGs to block all inbound traffic while whitelisting my own public IP address. The new parameters ensured that only authorized traffic from trusted sources where allowed access to the virtual machines.

By comparing the attack maps of the previous 24-hour cycle, I was able to demonstrate the impact and effectiveness of the hardening measures taken to improve the overall security posture of each of the virtual machines. The VMs were then left for another 24 hours to compare the activity after hardening.

> The map queries for both virtual machines returned no results as there were no instances of malicious activity after hardening.

![image](https://github.com/mohammedshahwan/Azure-Honeypot-SOC/blob/main/assets/No%20Results%20Query.png)

## Conclusion
In this project, I set up honeypots that effectively attracted and logged the authentication attempts of various attackers. Microsoft Sentinel was used to create incidents based on logs ingested through Log Analytics Workspace. The baseline activity of the insecure honeypot environments was recorded over a 24-hour timespan, then again over another 24-hour timespan after hardening the environments against potential threats.

By comparing the activity before and after hardening, we can see that the overall security posture has improved, as indicated by the significant reduction in malicious activity. However, it is important to note that if the network resources were extensively utilized by regular users, it is very likely that there would have been a larger amount of security events and alerts generated in the 24-hour timespan after implementing the new security controls.
