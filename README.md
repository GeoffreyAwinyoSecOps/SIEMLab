<h1>Configure a SIEM in Azure</h1>

 ### [YouTube Demonstration](https://youtu.be/7eJexJVCqJo)

<h2>Description</h2>
This project demonstrates how to build a lightweight Security Information and Event Management (SIEM) lab in Microsoft Azure using just a single Windows VM. Instead of installing third-party logging agents, this lab leverages built-in Windows Security Event logs collected via the Azure Monitoring Agent (AMA). These logs are ingested into Log Analytics and monitored using Microsoft Sentinel.

The goal is to simulate basic SOC operations such as:

- Collecting and analyzing native security events

- Creating detection rules using Kusto Query Language (KQL)

- Investigating alerts

- Visualizing attacker IP origins on a world map to demonstrate geolocation-based analysis

By focusing on default logs and a minimal Azure footprint, this lab is ideal for beginners, students, and professionals looking to learn SIEM fundamentals without complex endpoint configurations like Sysmon.


<br />


<h2>Tools and Services Used</h2>

- <b>Microsoft Azure</b> 
- <b>Windows 10 VM</b>
- <b>Azure Monitoring Agent (AMA)</b>
- <b>Log Analytics Workspace</b>
- <b>Microsoft Sentinel</b>
- <b>KQL (Kusto Query Language)</b>
- <b>Geolocation with IP-to-country mapping</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)

<h2>Program walk-through:</h2>

<p align="center">
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
