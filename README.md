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

<div align="center">

<h2>Lab Walk-through:<br><br>
 Part I: Lab Setup</h2>
<h3>Overview of Azure Security Operations Center</h3>

<img src="https://i.imgur.com/9icHPhr.png" height="80%" width="80%"/>

</div>

STEP 1: Microsoft Azure Subscription<br>
1.	Begin with an Azure subscription (free or paid): https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account<br>
2.	Once subscribed, sign in here:  https://portal.azure.com<br><br>

STEP 2: Create a Resource Group<br>
1.	From the search bar on the home page of portal.azure.com, search for ‘resource groups’ and hit Enter.<br>
2.	In ‘Resource groups’ select ‘+ Create’ to create a new resource group.<br>
3.	Select Azure subscription from a list.<br>
4.	Enter the name of your resource group, mine is ‘SIEMLab’.<br>
5.	If satisfied with information entered in the fields, select ‘Review + Create’ and finally ‘+ Create’ to complete the process.<br><br>

<div align="center">
 
<img src="https://i.imgur.com/IBdDUEm.png" height="80%" width="80%"/>

</div>
<br><br>

STEP 3: Create a Virtual Network
1.	Once Step 2 is complete, from the search bar, search for ‘virtual networks’ and hit Enter.
2.	Select ‘+ Create’ in the proceeding page.
3.	In the ‘Basics’ tab, select ‘Subscription’, ‘Resource Group’, and ‘Region’. II am using the subscription and resource group created in the preceding steps. The name of my virtual network is       SIEMLabVM-vnet.
4.	Default settings are fine for the rest of the tabs. Select ‘Review + Create’ and finally ‘+ Create’ to complete the process. 



<p align="center">
Launch the utility: <br/>

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
