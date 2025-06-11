<h1>Configure a SIEM in Azure</h1>

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

<table align="center" style="border: 5px solid #000; box-shadow: 5px 5px 5px #888;">
  <tr>
    <td>
      <img src="https://i.imgur.com/9icHPhr.png" width="80%" alt="Framed Image"/>
    </td>
  </tr>
</table>
<br>
<br>

</div>

<strong>STEP 1: Microsoft Azure Subscription</strong><br>
1.	Begin with an Azure subscription (free or paid): https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account<br>
2.	Once subscribed, sign in here:  https://portal.azure.com<br><br>

<strong>STEP 2: Create a Resource Group</strong><br>
1.	From the search bar on the home page of portal.azure.com, search for ‘resource groups’ and hit Enter.<br>
2.	In ‘Resource groups’ select ‘+ Create’ to create a new resource group.<br>
3.	Select Azure subscription from a list.<br>
4.	Enter the name of your resource group, mine is ‘SIEMLab’.<br>
5.	If satisfied with information entered in the fields, select ‘Review + Create’ and finally ‘+ Create’ to complete the process.<br><br>

<div align="center">

<table style="border: 5px solid #000; box-shadow: 5px 5px 5px #888;">
  <tr>
    <td>
      <img src="https://i.imgur.com/IBdDUEm.png" width="60%" alt="Framed Image"/>
    </td>
  </tr>
</table>

</div>

<br><br>

<strong>STEP 3: Create a Virtual Network</strong>
1.	Once Step 2 is complete, from the search bar, search for ‘virtual networks’ and hit Enter.
2.	Select ‘+ Create’ in the proceeding page.
3.	In the ‘Basics’ tab, select ‘Subscription’, ‘Resource Group’, and ‘Region’. I am using the subscription and resource group created in the preceding steps. The name of my virtual network is       SIEMLabVM-vnet.
4.	Default settings are fine for the rest of the tabs. Select ‘Review + Create’ and finally ‘+ Create’ to complete the process.<br><br> 

<div align="center">

<table style="border: 5px solid #000; box-shadow: 5px 5px 5px #888;">
  <tr>
    <td>
      <img src="https://i.imgur.com/NfYM65k.png" width="60%" alt="Framed Image"/>
    </td>
  </tr>
</table>

</div>

<br><br>

<strong>STEP 4: Create A Network Security Group</strong>
1.	From the search bar at the top of the page, search for ‘network security groups’ and select ‘network security groups’ from the list.
2.	Select ‘+ Create’ in the proceeding page.
3.	Make the appropriate selections for subscription, resource group, and region. Select ‘Review + Create’ and then ‘+ Create’.
4.	Once created, open the NSG and edit ‘Inbound security rules’ to allow any protocol from any source as shown below.     

<strong>NOTE:</strong> The Network Security Group acts as a firewall in the cloud environment. The goal here is to make it easier for attackers to find us for the sake of the lab.<br><br>

<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr>
    <td>
      <img src="https://i.imgur.com/lao0atq.png" width="80%" alt="First Framed Image"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr>
    <td>
      <img src="https://i.imgur.com/A1OntQM.png" width="80%" alt="Second Framed Image"/>
    </td>
  </tr>
</table>

</div>
<br>
<br>


<strong>STEP 5: Create a Virtual Machine</strong>
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

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr><td>
    <img src="https://i.imgur.com/d2W3eLg.png" width="80%" alt="Image 1"/>
  </td></tr>
</table>
<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr><td>
    <img src="https://i.imgur.com/PDvTfyl.png" width="80%" alt="Image 2"/>
  </td></tr>
</table>
<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr><td>
    <img src="https://i.imgur.com/BbV5QiS.png" width="80%" alt="Image 3"/>
  </td></tr>
</table>
<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr><td>
    <img src="https://i.imgur.com/lNYIi9z.png" width="80%" alt="Image 4"/>
  </td></tr>
</table>
<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr><td>
    <img src="https://i.imgur.com/Vut1BW9.png" width="80%" alt="Image 5"/>
  </td></tr>
</table>
<br><br>

</div>


<strong>NOTE:</strong> Once the VM is created, log in to it via RDP using its public email address provided in the Azure portal. Once logged in, type ‘wf.msc’ and hit enter in the search bar in the taskbar > select ‘Windows Defender Firewall’ and cycle through the tabs – ‘domain’, ‘private’, and ‘public’ ensure that the status in each is set to ‘off’. This will ensure that the VM is reachable.

<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888;">
  <tr>
    <td>
      <img src="https://i.imgur.com/Jex1AWP.png" width="60%" alt="Framed Image"/>
    </td>
  </tr>
</table>

</div>

<br><br>

<div align="center">

<h2>Part II: Log Collection<br><br>
Virtual Machine Logs</h2>


</div>

Windows Event Logs can be accessed on the VM by searching for and launching Event Viewer > 'Windows Logs' > 'Security' as shown below:<br><br>

<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/hU8buGx.png" width="60%" alt="Framed Image"/>
    </td>
  </tr>
</table>

</div>


<br>
<br>
Select ‘Filter Current Log’ on the left-hand side of the window and enter event ID 4625 – Failed log in attempts should yield one of my 3 purposely failed log in attempts by ‘fakeuser3’ as shown below:
<br>
<br>
<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/IqTIgJr.png" width="80%" alt="Framed Image 1"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/fYvBMcS.png" width="80%" alt="Framed Image 2"/>
    </td>
  </tr>
</table>

</div>

<br>
<br>

<strong>STEP 6: Create a Log Repository in Azure</strong>
1.	Type ‘log analytics workspace’ in the search bar at the top of the Azure portal page and select the top result.
2.	On the next page, select ‘+ Create’ and ensure subscription, resource group, and region are the same as in the preceding steps.
3.	Select ‘Review + Create’ and then ‘+ Create’.<br><br>

<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/vPDq8HS.png" width="80%" alt="Image 1"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/ou0Fn25.png" width="60%" alt="Image 2"/>
    </td>
  </tr>
</table>

</div>

<br>
<br>

<strong>STEP 7: Create a SIEM (Security Information and Event Management) Solution</strong><br>
1.	Type ‘microsoft sentinel’ in the search bar at the top of the Azure portal page and select the top result.<br>
2.	On the next page, select ‘+ Create’ and select the Log Analytics Workspace from the preceding steps and select ‘Add’.<br>

<Strong>NOTE:</strong> This step links the Log Analytics Workspace to the SIEM.<br><br>

<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/ABEeppG.png" width="80%" alt="Image 1"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/UKS9sQ4.png" width="80%" alt="Image 2"/>
    </td>
  </tr>
</table>

</div>

<br>
<br>

<strong>STEP 8: Establish a Connection Between Log Analytics Workspace and the Virtual Machine</strong>
1.	Type ‘microsoft sentinel’ in the search bar at the top of the Azure portal page and select the top result.<br>
2.	On the next page, select the Log Analytics Workspace previously created. Within the workspace, navigate to ‘Content Management’ > ‘Content Hub’. Within the Content Hub, search for ‘windows security events’, select it and select ‘Install’.<br>
3.	Once Windows Security Events is installed, select ‘Manage’ > select ‘Windows Security Events via AMA’ > select ‘Open Connector Page’ > select ‘+ Create a Data Collection Rule’. NOTE: this is used by the VM to forward logs into the Logs Analytics Workspace.<br>
4.	Enter a name for the Data Collection Rule, select the appropriate subscription, resource group and select ‘Next’.<br>
5.	Under ‘Resources’, expand all and select the target VM. Select ‘All Events’ and select ‘Review + Create’.<br><br>


<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/RjmbcsA.png" width="80%" alt="Image 1"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/BMZXFfn.png" width="80%" alt="Image 2"/>
    </td>
  </tr>
 
<div align="center">
<h3>Viewing Logs in Log Analytics Workspace</h3>
</div>

<div align="left">
1. Search for ‘log analytics workspace’ from the search bar at the top of the Azure portal page and select the top result.<br>
2. Select ‘Logs’ and close pop-up windows that appear.<br>
3. Enter queries such as ‘SecurityEvent’ for example and select ‘Run’. View the resulting logs.<br><br> 
</div>

<table align="center" style="border: 5px solid #000; box-shadow: 5px 5px 5px #888;">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/0MgGrTM.png" width="80%" alt="Framed Image"/>
    </td>
  </tr>
</table>
<br>
<br>
</div>
<br>
<strong>STEP 9: Create a Map Using Log Data</strong><br><br>
1.	Download the spreadsheet from here: https://drive.google.com/file/d/13EfjM_4BohrmaxqXZLB5VUBIz2sv9Siz/view.<br>
2.	Back in Azure portal, search for and select ‘microsoft sentinel’.<br>
3.	Select the Log Analytics Workspace previously created > select ‘Configuration’ > select ‘Watchlist’ > select ‘New’ > enter ‘Name’ (I used ‘GeoIP’) > Alias (can be same as name) > 
   select ‘Next’ and browse for location of downloaded file from above > select ‘Network’ for ‘Searchkey’ > select ‘Review + Create’<br>
4.	From the Log Analytics Workspace within Microsoft Sentinel, navigate to ‘Threat Management’ and select ‘Workbooks’ > select ‘Add’ > select ‘Edit’ and clear all 
   content > copy JSON file from here: https://drive.google.com/file/d/1ErlVEK5cQjpGyOcu4T02xYy7F31dWuir/view > select ‘+’ query and paste the JSON.<br><br>


<div align="center">

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/zRxLAU2.png" width="80%" alt="Image 1"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/N997OCK.png" width="80%" alt="Image 2"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/AcZFlBP.png" width="80%" alt="Image 4"/>
    </td>
  </tr>
</table>

<br><br>

<table style="border: 2px solid #000; box-shadow: 2px 2px 5px #888; border-collapse: collapse;" cellspacing="0" cellpadding="0">
  <tr>
    <td style="padding: 0;">
      <img src="https://i.imgur.com/OB0z9Xr.png" width="80%" alt="Image 5"/>
    </td>
  </tr>
</table>

</div>

<br>
<br>
























