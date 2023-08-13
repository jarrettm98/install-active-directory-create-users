<p align="center">
<img src="https://i.imgur.com/Q3YpjDD.png" alt="AD Logo"/>
</p>

<h1>Installing Active Directory and Creating Users</h1>
This is a tutorial on how to install Active Directory using Azure Virtual Machines. If you need to learn how to create Resource groups or Virtual Machines in Azure, go to the following links for my tutorials.

https://github.com/jarrettm98/azure-crash-course

https://github.com/jarrettm98/azure-compute-and-networking

<h2>Technology Used</h2>

- Microsoft Azure
- Remote Desktop
- Active Directory

<h2>Operating Systems Used</h2>
- Windows Server 2022
- Windows 10

<h2>Prerequisites</h2>
- Create Domain Controller and Client Virtual Machines
- Enable ICMPv4 rules with Windows Defender

<h2>Step 1: Setup</h2>

First, using Azure, create a Resource Group. Now, create 2 Virtual Machines(VMs). One will be the Domain Controller and the other will be the Client. To create the Domain Controller, give the VM a name as well as assign it to the Resource Group created before. 

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/2cdc4c13-ab7e-470c-aeaa-c6fce4f297ad)

Now for the image use Windows Server 2022. It is recommended for the size to use 2 vcpus.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/42ee1038-9ce5-4cb4-aa3d-cfff71f442fa)

Give the admin log in credentials that can be remembered or just write them down in notepad. Now, click "Next" until reaching the "Networking" tab. Take note of the "Virtual Network" created. This will be important when creating the Client VM. Check the box under Licensing then "Review and create" the VM.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/4d0af242-ce65-4bee-bd48-03a050c4bc4b)

Now, create the Client VM. Same thing as the first one except the image should be using Windows 10.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/7a5a22a1-2b5e-4565-a37e-91a20d3da805)

Click, Next until reaching the Networking tab. Make sure the Virtual Network is the same as the Domain Controller. Now Review and create.

Now it's time to set the Domain Controller's NIC Private IP to static. Go to the Domain Controller and click on the "Networking" tab. Next, click on the "Network Interface."

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/692c8cfc-8161-4f4c-a288-e0c3624ebde4)

Now, go the "IP configurations" tab and click on the IP configuration. 

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/c8f86905-0a5c-4de4-8c40-b449880b7643)

Now, change the Allocation from "Dynamic" to "Static." Then click Save.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/ec5ef55f-c26e-47c7-86de-ca07eb7d260d)

Now, using the user and password created before, login to the Client with it's IP address in Remote Desktop Connection. 

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/952e6c9e-6079-46bb-994c-469b36da2ecb)

Now, using Command Prompt, ping the Domain Controller with it's Private IP Address. Type in "ping (Your DC Private IP) -t" to perpetually ping. For now it will time out.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/5acef08a-c31f-4261-9f9a-218c28777b35)

Next its time to enable ICMPv4. First, login to the Domain Controller VM then open "Windows Defender Firewall with Advanced Security" 

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/7918fdd6-808a-424f-a718-00fbcff72be6)

Click on "Inbound Rules" and Sort by "Protocol". Look for the rules with "Core Networking Diagnostics - ICMP Echo Request(ICMPv4-In)" There will be two of them (Both on the bottom of the image below)

![icmpv4 rules](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/d9080231-46b1-442c-872e-a8fff345bea0)

Right-click and Enable both rules. Now go back to the Client VM and check on the command prompt. It should now be properly pinging the Domain Controller.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/ccd209de-9c2a-4eeb-a5d8-0b2addd974e2)


<h2>Step 2: Installing Active Directory</h2>

Now time to Install Active Directory. Go to the Domain Controller. In "Server Manager" click on "Add roles and features."

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/fbc991e3-3855-447b-aaa8-70d18c93ad21)

Click "Next" until reaching the "Server Roles" section. Now, check the box next to "Active Directory Domain Services" then "Add Features."

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/d90f9137-1393-4cc4-a9d4-7bd6e32869a2)

Click Next until reaching the "Confirmation" tab then click "Install." It may take a while to install. Once it says "Configuration required. Installation succeeded on (Your DC name here). Click "Close"

Towards the top-right corner of the Server Manager window, there will be a flag and a yellow triangle with a "!" symbol. Click on that then "Promote the server to a domain controller"

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/488877a6-b734-4f24-9930-9cfc3ae58931)

A window will pop up for a Configuration Wizard. Check the bubble next to "Add a new forest" then give it a domain (Example in image below) Click next.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/e25321f9-3964-4a51-a147-dc505db66db9)

Give it a DSRM password (Required but wont be used in this tutorial) Click next.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/f89f6021-2ccc-4253-be0a-dfbc1b857743)

Next, the NETBIOS domain will be made. This may take a moment. Once it is made, Click next until reaching the "Prerequisites Check" tab. This process will take a moment. Now click "Install"

After Installing the VM will reboot. Once it is rebooted, Log back into the Domain Controller with the domain name and the username. Example below.

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/5e8736c2-dda9-4ec4-8bfc-a954bc34cce9)

Once logged in, using Server Manager click on tools in the top-right corner. Next click on "Active Directory Users and Computers".

![image](https://github.com/jarrettm98/install-active-directory-create-users/assets/140662793/8fe68c25-2b8d-44de-9ebe-4fbc4ff1df84)
