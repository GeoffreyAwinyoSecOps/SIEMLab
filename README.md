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
 
<img src="https://i.imgur.com/IBdDUEm.png" height="60%" width="60%"/>

</div>
<br><br>

STEP 3: Create a Virtual Network
1.	Once Step 2 is complete, from the search bar, search for ‘virtual networks’ and hit Enter.
2.	Select ‘+ Create’ in the proceeding page.
3.	In the ‘Basics’ tab, select ‘Subscription’, ‘Resource Group’, and ‘Region’. I am using the subscription and resource group created in the preceding steps. The name of my virtual network is       SIEMLabVM-vnet.
4.	Default settings are fine for the rest of the tabs. Select ‘Review + Create’ and finally ‘+ Create’ to complete the process.<br><br> 

<div align="center">
 
<img src="https://i.imgur.com/NfYM65k.png" height="60%" width="60%"/>

</div>
<br><br>

STEP 4: Create A Network Security Group
1.	From the search bar at the top of the page, search for ‘network security groups’ and select ‘network security groups’ from the list.
2.	Select ‘+ Create’ in the proceeding page.
3.	Make the appropriate selections for subscription, resource group, and region. Select ‘Review + Create’ and then ‘+ Create’.
4.	Once created, open the NSG and edit ‘Inbound security rules’ to allow any protocol from any source as shown below.     

NOTE: The Network Security Group acts as a firewall in the cloud environment. The goal here is to make it easier for attackers to find us for the sake of the lab.<br><br>

<div align="center">
 
<img src="https://i.imgur.com/lao0atq.png" height="80%" width="80%"/>
<br>
<br>
<img src="https://i.imgur.com/A1OntQM.png" height="80%" width="80%"/>

</div>
<br><br>

STEP 5: Create a Virtual Machine
1.	From the search bar at the top of the page, search for ‘virtual machines’, hit enter and select ‘virtual machines.’
2.	Select ‘+ Create’ in the proceeding page.
3.	In the ‘Basics’ tab, select subscription, resource group, and region. Select an Windows 10 image.
4.	Select a size that works for your budget, remembering to power-off devices when not in use. For purposes of this lab, ‘Standard’ option should work well.
5.	Create a user with a password and confirm the password.
6.	Check the License agreement box at the bottom of the page and click ‘Next’.
7.	Under ‘Disks’, standard disk size should be fine for this lab.
8.	Under ‘Networking’ select the virtual network, subscription, and regions created/selected earlier. Check the box to delete resources when done. Select ‘Next’.
9.	Under ‘Management’, defaults are okay. Select ‘Next’.
10.	Under ‘Monitoring’, select ‘Disable’ in ‘Boot Diagnostics’. Select ‘Next’.
11.	Under ‘Advanced’ and ‘Tags’, defaults are fine. Select ‘Review + create’ and then ‘+ Create’ to complete creating a VM.<br><br>

<div align="center">
 
<img src="https://i.imgur.com/d2W3eLg.png" height="80%" width="80%"/>
<br>
<br>
<img src="https://i.imgur.com/PDvTfyl.png" height="80%" width="80%"/>
<br>
<br>
<img src="https://i.imgur.com/BbV5QiS.png" height="80%" width="80%"/>
<br>
<br>
<img src="https://i.imgur.com/lNYIi9z.png" height="80%" width="80%"/>
<br>
<br>
<img src="https://i.imgur.com/Vut1BW9.png" height="80%" width="80%"/>
<br>
<br>

</div>
<br><br>

NOTE: Once the VM is created, log in to it via RDP using its public email address provided in the Azure portal. Once logged in, type ‘wf.msc’ and hit enter in the search bar in the taskbar > select ‘Windows Defender Firewall’ and cycle through the tabs – ‘domain’, ‘private’, and ‘public’ ensure that the status in each is set to ‘off’. This will ensure that the VM is reachable.

<div align="center">
 
<img src="https://i.imgur.com/Jex1AWP.png" height="60%" width="60%"/>

</div>
<br><br>
<div align="center">

<h2>Part II: Log Collection<br><br>
Virtual Machine Logs</h2>


</div>

Windows Event Logs can be accessed on the VM by searching for and launching Event Viewer > 'Windows Logs' > 'Security' as shown below:<br><br>

<div align="center">
 
<img src="https://i.imgur.com/hU8buGx.png" height="60%" width="60%"/>

</div>
<br>
<br>
Select ‘Filter Current Log’ on the left-hand side of the window and enter event ID 4625 – Failed log in attempts should yield one of my 3 purposely failed log in attempts by ‘fakeuser3’ as shown below:
<br>
<br>
<div align="center">
 
<img src="https://i.imgur.com/IqTIgJr.png" height="80%" width="80%"/>
<br>
<br>
<img src="https://i.imgur.com/fYvBMcS.png" height="80%" width="80%"/>

</div>
























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
