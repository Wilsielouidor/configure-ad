<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure 
- Confirm connection between the Client and Domain Controller 
- Install Active Directory 
- Create Organizational Units and and Admin User Account
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<h3> Setup Resources in Azure</h3>
<h4>Create Domain Controller VM (which would include on-premises Active Directory)</h4>


<p>
<img width="1404" alt="Screen Shot 2023-10-16 at 9 10 41 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/636a230e-46e6-4f86-9465-30e67eaec8c1">

</p>
<p>
First create a Virtual Machine that will be the Domain Controller. Name it DC-1 (with the image as Windows Server 2022/3-2 vcpus storage) Create a resource group while making virtual machine. Under Adminstrator account create a username and password. 
</p>
<br />

<p>
<img width="780" alt="Screen Shot 2023-10-16 at 9 13 06 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/1e88049a-8af3-464e-a55c-a98636de11f6">
</p>
<p>
This is the Vnet that has been automatically created while making the DC-1 Virtual Machine. Make sure that when Client-1 Virtual Machine is created it has the same Vnet as DC-1
</p>
<br />

<h4>Create Client-1 VM</h4>
<p>
<img width="1435" alt="Screen Shot 2023-10-16 at 9 47 18 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/0bd1498e-1ecd-494a-a3e0-0779dd164de8">
</p>
<p>
Create Client VM (with Windows 10 as the image/3-2 vcpus storage). Use the same resource group and Vnet that was created from the first VM (DC-1) that was created. After the Client-1 has deployed double check if both VMs have the same vnet and are in the same region.
</p>
<br />


<h4>Set Domain Controller’s NIC Private IP address to be static</h4>
<p>
<img width="1440" alt="Screen Shot 2023-10-16 at 10 00 41 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/7ed56c09-c431-41ec-8f55-483a1c36dcaf">
</p>
<p>
 Go to DC-1 under virtual machines in Azure. Go to Network settings located on the left side-> Click DC-1's Network Interface-> Click IP configurations that is on the left side-> Scroll down and click ipconfig1-> Click static under Allocation-> Click Save 
</p>
<br />

<h3> Ensure Connectivitiy Between the Client and Domain Controller</h3>
<p>
<img width="1440" alt="Screen Shot 2023-10-16 at 10 44 56 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/8d870ed4-576b-4631-bbc9-056cb89babe7">
</p>
<p>
<ul>
<li>Login to Client-1 with Remote Desktop-> Open Command prompt from search engine next to windows icon->ping DC-1’s private IP address with ping -t <ip address> (which is a perpetual ping)</li>
<li>Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall (by typing in wf.msc on search engine next to the start button)</li>
<li>Go to inbound rules</li>
<li>Look for ICMPv4 Echo by clicking protocol to bring it in order based on protocol</li>
<li>Then enable two echo requests</li>
</ul>
</p>
<br />

<p>
<img width="993" alt="Screen Shot 2023-10-16 at 10 33 36 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/fb47cc2c-c78c-4f21-9f33-953c227f6967">
</p>
<p>
Check back at Client-1 to see the ping succeed
</p>
<br />

<h3>Install Active Directory</h3>
<p>
<img width="1440" alt="Screen Shot 2023-10-16 at 11 35 23 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/f11f1cfd-77fe-4678-89ee-2e1d8f4044a8">
</p>
<p>
<ul>
<li>Login to DC-1 and install Active Directory Domain Services</li>
<li>Click add roles and features</li> 
<li>Click next and then make sure to check box of Active Directory Domain Services then install</li>
<li> Continue to click next and then install</li>
<li> Click the Exclamation sign</li>
<li>Promote as a DC: Setup a new forest as wilsiepm.com (can be anything, just remember what it is)</li>
<li>Restart and then log back into DC-1 as user: wlabuser@wilsiepm.com</li>
</ul>
</p>
<br />


<p>
<img width="1440" alt="Screen Shot 2023-10-16 at 11 43 05 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/30b9539c-ea79-4c6c-8034-3591d3afb985">
</p>
<p>
<ul>
<li> Click the Exclamation sign</li>
<li>Promote as a DC: Setup a new forest as wilsiepm.com (can be anything, just remember what it is)</li>
<li> Create a password (you would not need to use during these steps) </li>
<li>Restart and then log back into DC-1 as user: wlabuser@wilsiepm.com</li>
</ul>
</p>
<br />

<h3>Create Organizational Units and an Admin User Account in AD</h3>

<p>
<img width="1440" alt="Screen Shot 2023-10-17 at 12 43 48 AM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/173fe67b-8acf-46a1-84c5-d5febf263f94">

</p>
<p>
  <ul>
<li> Go to Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”</li>
<li>Create a new OU named “_ADMINS”</li>
  </ul>
</p>
<br />


<p>
<img width="1440" alt="Screen Shot 2023-10-17 at 12 49 18 AM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/3a995d00-be48-4e2b-8824-c45326dceed0">

</p>
<p>
  <ul>
<li>Create a new employee named “Mateo Maxwell” (same password) with the username of “mateo_admin” under _ADMINS folder</li>

  </ul>
</p>
<br />

<p>
<img width="1440" alt="Screen Shot 2023-10-17 at 12 53 39 AM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/ea967430-9b03-4af4-9fb3-7539b031a4a4">

</p>
<p>
  <ul>
<li>Add mateo_admin to the “Domain Admins” Security Group</li>
<li>Right click on mateo_admin->properties-> remote control->add-> types in domain admins-> Click Check Names-> Click OK->Click Apply-> Click OK</li>
<li>Log out/close the Remote Desktop connection to DC-1 and log back in as “mateo_admin@wilsiepm.com”</li>
<li>User mateo_admin as your admin account from now on</li>
  </ul>
</p>
<br />


<p>
<img width="621" alt="Screen Shot 2023-10-17 at 12 58 03 AM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/08e1a771-faf3-4cb6-ac28-d369380b8c8f">
</p>
<p>
  <ul>
<li>Log out/close the Remote Desktop connection to DC-1 and log back in as “mateo_admin@wilsiepm.com”</li>
<li>Use mateo_admin as your admin account from now on</li>
  </ul>
</p>
<br />
<h3>Join Client-1 to your domain (mydomain.com)</h3>
<p>
<img width="1440" alt="Screen Shot 2023-10-17 at 3 26 18 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/ec65c9e2-942d-4824-a88f-9c4672088fe7">
</p>

<p>
 <img width="1440" alt="Screen Shot 2023-10-17 at 4 40 01 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/bfbafd59-a165-4f17-b8a5-131bf4611032">
</p>

<p>
  <ul>
<li>From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address</li>
  <ul>
<li>Go to client-1</li>
<li>Click Network settings on the left side</li>
<li> Click the name of the Network Interface</li>
<li>On the left click DNS server</li>
<li>Click custom and type in DC-1’s private IP Address (which is 10.0.0.4 in this case)-> click save</li>
  </ul>
   <p> <img width="1162" alt="Screen Shot 2023-10-17 at 4 41 45 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/82886361-4848-443e-ab76-84e115c31bfe">
</p>
   <ul>
<li>From the Azure Portal, restart Client-1</li>
   </ul>
</ul>
  </ul>
</p>
<br />

<p>
<img width="1439" alt="Screen Shot 2023-10-17 at 5 04 17 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/7067fd2a-0bde-4ebb-bbf2-a735a02962fb">
</p>

<p>
 <img width="1433" alt="Screen Shot 2023-10-17 at 5 17 57 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/0c1ecf24-4dcb-471c-9947-6444688ab19d">
</p>

<p><img width="1440" alt="Screen Shot 2023-10-17 at 5 20 09 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/448e70cb-3017-4447-a18b-9908d1b24fd5">
</p>

<p>
 <img width="1440" alt="Screen Shot 2023-10-17 at 5 21 29 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/e40a2ce3-edaa-43e2-8534-55c77ed83098">
</p>
<p>
  <ul>
<li>Login to Client-1 (Remote Desktop) as the original local admin (wlabuser) and join it to the domain (computer will restart)</li>
<ul>
<li>Right click the start menu-> Systems->rename this pc advance</li>
<li>Go to change-> click domain-> type in domain name: wilsiepm.com</li>
<li>Click ok and a another window will pop up to confirm the changes by using the admin login account to confirm the domain change</li>
<li>Client-1 will automatically restart after confirming the domain changes</li>
<li>Login Client-1 using mateo_admin</li>
</ul>
  </ul>
</p>
<br />

<p>
<img width="1440" alt="Screen Shot 2023-10-17 at 11 00 34 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/5c7d5506-2adb-4824-afda-94dd51eea534">
</p>

<p><img width="1431" alt="Screen Shot 2023-10-17 at 11 02 01 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/946bcd3d-c793-4768-9349-4632a5da1706">
</p>
<p>
 <ul>
<li>Log into Client-1 as mateo_admin@wilsiepm.com and open system properties</li>
 <li>Click “Remote Desktop”</li>
<li>Select users that can remotely access this PC</li>
<li>Click Add-> type in domain users-> check name</li>
<li>Allow “domain users” access to remote desktop</li>
<li>You can now log into Client-1 as a normal, non-administrative user now</li> 
 </ul>
</p>
<br />
<h3>Create Additional Users and Attempt to log into Client-1 with one of the Users</h3>
<p>
<img width="1440" alt="Screen Shot 2023-10-17 at 11 06 50 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/62e97643-88b7-4dcd-b98c-fa9713574576">
</p>
<br />

<p><img width="1437" alt="Screen Shot 2023-10-17 at 11 09 28 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/88d927bc-cdc9-4f4b-8294-2907c4be2854">
</p>
<br />

<p><img width="1432" alt="Screen Shot 2023-10-17 at 11 13 13 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/58b3fa53-994a-4c27-9f98-61c70f7e78ad">
</p>
<br />


<p>
 <ul>
<li>Login to DC-1 as mateo_admin</li>
<li>Open PowerShell_ise as an administrator </li>
  <ul>
<li>Type PowerShell_ise on search engine next to the start button</li>
<li>Right click on Powershell_ise and click run as administrator</li>
  </ul>
<li>Create a new File and paste the contents of the <a href= "https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1"> script </a>into it.</li>
  <img width="1275" alt="Screen Shot 2023-10-17 at 8 27 11 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/be9c6601-b39f-43ee-8d7c-da3f01e7f547">
<li>Run the script and observe the accounts being created</li>

 </ul>
</p>
<br />

<p>
 <img width="752" alt="Screen Shot 2023-10-17 at 10 06 17 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/07b88b2a-c2ed-421a-8c3c-aa5ed79b8ee4">
 </p>
<p>
When finished, open ADUC and observe the accounts in the appropriate OU
</p>
<br />


<p>
<img width="1435" alt="Screen Shot 2023-10-17 at 11 20 20 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/9e384719-87f1-4ac8-a24a-723cc8609b3c">
</p>

<p>
Attempt to log into Client-1 with one of the accounts (take note of the password in the script which is "Password1" in this case)
</p>
<br />


<p>
END.
</p>
