<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

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
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>
<h3> Setup Resources in Azure</h3>
- Create Domain Controller VM (which would include on-premises Active Directory) 


<p>
<img width="1404" alt="Screen Shot 2023-10-16 at 9 10 41 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/636a230e-46e6-4f86-9465-30e67eaec8c1">

</p>
<p>
First create a Virtual Machine that will be the Domain Controller. Name it DC-1 (with the image as Windows Server 2022/3-2 vcpus storage) Create a resource group while making virtual machine. 
</p>
<br />

<p>
<img width="780" alt="Screen Shot 2023-10-16 at 9 13 06 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/1e88049a-8af3-464e-a55c-a98636de11f6">
</p>
<p>
This is the Vnet that has been automatically created while making the DC-1 Virtual Machine. Make sure that when Client-1 Virtual Machine is created has the same Vnet as DC-1
</p>
<br />

- Create Client-1 VM
<p>
<img width="1435" alt="Screen Shot 2023-10-16 at 9 47 18 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/0bd1498e-1ecd-494a-a3e0-0779dd164de8">
</p>
<p>
Create Client VM (with Windows 10 as the image/3-2 vcpus storage). Use the same resource group and Vnet that was created from the first VM (DC-1) that was created. After the Client-1 has deployed then make sure to check if both VMs have the same vnet and are in the same region.
</p>
<br />

<p>
<img width="1440" alt="Screen Shot 2023-10-16 at 10 00 41 PM" src="https://github.com/Wilsielouidor/configure-ad/assets/142513380/7ed56c09-c431-41ec-8f55-483a1c36dcaf">
</p>
<p>
Set Domain Controller’s NIC Private IP address to be static. Go to DC-1 under virtual machines in Azure. Go to Network settings located on the left side-> Click DC-1's Network Interface-> Click IP configurations that is on the left side-> Scroll down and click ipconfig1-> Click static under Allocation-> Click Save 
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
<li>Then enable for two echo requests</li>
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
<li>Restart and then log back into DC-1 as user: wilsiepm.com\wlabuser</li>
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
<li>Restart and then log back into DC-1 as user: wilsiepm.com\wlabuser</li>
</ul>
</p>
<br />

<h3>Create an Admin and Normal User Account in AD</h3>

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
<li>Add jane_admin to the “Domain Admins” Security Group</li>
<li>Right click on mateo_admin->properties-> remote control->add-> types in domain admins-> Click Check Names-> Click OK->Click Apply-> Click OK</li>
<li>Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”</li>
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

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
