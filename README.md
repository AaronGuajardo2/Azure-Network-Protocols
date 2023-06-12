<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we will observe various network traffic to and from two different Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Various Command-Line Tools (Powershell)
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create a Resource Group and Virtual machines in Azure environment
- Use Remote Desktop to access Windows VM and install Wireshark software
- Use Wireshark protocol analyzer to observe the following protcol traffic
      - ICMP, SSH, DHCP, DNS, RDP
- Within Virtual machine use power shell or command line to initiate ping requests and other actions to be observed by Wireshark
- (in this demonstration power shell was used)

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/85Fqllk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a Resource Group
  - Resource Group -> create -> name the group -> region: pick the one closest to your location for optimal performance and cost efficiency -> Review and Create -> Create

<p>
<img src="https://i.imgur.com/5FK3Oxr.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
<p>
<img src="https://i.imgur.com/isct0wJ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a Windows 10 Virtual Machine (VM1)
  - Virtual Machine -> create -> select the resource group we just created -> name your machine (VM1) -> Select a region (make sure its the same as resource group) -> Select the Windows 10 image -> select at least 2 cpus -> user: Labuser1 -> create a password -> check the box for Liscense Agreement -> Review and create -> Create

</p>
<br /> 

<p>
<img src="https://i.imgur.com/KsGpyyb.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/vBZSCwO.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a Linux (Ubuntu) virtual machine(VM2)
  - Virtual Machine -> create -> select the resource group we just created -> name your machine (VM2) -> Select a region (same as your resource group) -> Select the Ubuntu image -> select at least 2 cpus -> switch from SSH to password -> user: Labuser1 -> enter a password -> Review and create -> Create
  
<p>
<img src="https://i.imgur.com/VTukJgN.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  - Observe Your Virtual Network within Network Watcher or the Windows-vnet topology

</p>
<br />

<p>
<img src="https://i.imgur.com/d3tTje0.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Use Remote Desktop to connect to your Windows virtual machine (VM1)
      - Login with credentials created earlier. User:Labuser1 and corresponding password

<p>
<img src="https://i.imgur.com/a3U3Hbo.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>  

- Observe ICMP Traffic
  - Within your VM1, open a browser window, go to a search engine and type in "download Wireshark" and follow install steps
    - [WireShark](https://www.wireshark.org/)
  - Once installed open WireShark and filter for ICMP traffic only
  - From the VM1, open command line or Powershell(Powershell in this case) and attempt to ping a public website (www.google.com) and observe the traffic in Wireshark
  
</p>
<br />

<p>
<img src="https://i.imgur.com/YcK7egR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 <p>
<img src="https://i.imgur.com/toLtKB9.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p> 
  
- Use the private IP address of the Linux Ubuntu virtual machine(VM2) and ping it from within the Windows 10 VM(VM1)
- Initiate a non-stop ping from VM1 to your VM2
  - in Powershell, type "ping -t" and then the private IP address (10.0.0.5)

</p>
<br />

<p>
<img src="https://i.imgur.com/gLESYN2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p> 
  
- Open the Network Security Group your Ubuntu VM(VM2) is using and disable incoming ICMP traffic
  - Azure portal -> search Network Security Group -> Inbound Security Rules -> Add (a rule) -> click ICMP -> Deny -> set priority to 200 -> name the rule (Deny_IMCP_Ping_From_Anywhere) -> Add

</p>
<br />

<p>
<img src="https://i.imgur.com/YBciq50.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Go back to the Windows 10 VM(VM1), observe the ICMP traffic in Wireshark and the powershell Ping activity
  - Stop the Ping activity by using ctrl + c
  
</p>
<br />

<p>
<img src="https://i.imgur.com/fn58FJs.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/SZj1Uwm.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>

 - Observe SSH Traffic
  - Go back to WireShark and filter for SSH traffic only
  - From your Windows 10 VM(VM1), "SSH into" your Ubuntu VM(VM2) (via its private IP address)
    - cmd -> ssh Labuser1@10.0.0.5 (the private ip) -> enter -> yes -> type the password -> enter
  - note when typing in password characters will not show up but inputs are being recorded
  - type in a few commands when ssh connection is enabled to observe effects it has on ssh traffic

</p>
<br />

<p>
<img src="https://i.imgur.com/idSX6c6.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe DHCP Traffic
  -  Go back to WireShark and filter for DHCP traffic only
  - From your Windows 10 VM(VM1), attempt to issue your VM a new IP address from the command line (ipconfig /renew)
    - cmd -> ipconfig /renew -> enter

</p>
<br />

<p>
<img src="https://i.imgur.com/GcAfo4z.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe DNS Traffic
  -  Go back to WireShark and filter for DNS traffic only
  - From your Windows 10 VM(VM1) within Powershell, use "nslookup" to see what google.com and disney.com's IP addresses are
    - cmd -> nslookup www.google.com -> enter

</p>
<br />

<p>
<img src="https://i.imgur.com/de1hVxL.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe RDP Traffic
  -  Go back to WireShark and filter for RDP traffic only (tcp.prt==3389)
  - Observe the seemingly never ending spam of traffic.This is because RDP is an active connection and is continuosly transmitting data between the host and remote computer.

</p>
<br />



  
