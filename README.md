<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
  
<p>
<img src="https://i.imgur.com/VLPJNJZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a Resource Group
  - Resource Group -> create -> name the group -> region: pick one near your location for optimal performance -> Review and Create -> Create

<p>
<img src="https://i.imgur.com/PgEf0tk.png" height="60%" width="60%" alt="Disk Sanitization Steps"/> 
</p>
<p>
<p>
<img src="https://i.imgur.com/jqVMSq4.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a Windows 10 Virtual Machine (VM)
  - Virtual Machine -> create -> select the resource group we just created -> name your machine (Windows-VM) -> Select a region (same as your resource group) -> Select the Windows 10 image -> select at least 2 cpus -> user: labuser -> create a password -> check the box for Liscense Agreement -> Review and create -> Create

</p>
<br /> 

<p>
<img src="https://i.imgur.com/egwGob7.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/jK0x396.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Create a Linux (Ubuntu) VM
  - Virtual Machine -> create -> select the resource group we just created -> name your machine (Windows-VM) -> Select a region (same as your resource group) -> Select the Ubuntu image -> select at least 2 cpus -> switch from SSH to password -> user: labuser -> enter a password -> Review and create -> Create
  
- Observe Your Virtual Network within Network Watcher

</p>
<br />

<p>
<img src="https://i.imgur.com/iDO5Gh4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe ICMP Traffic
  - Use Remote Desktop to connect to your Windows 10 VM
  - Within your Windows 10 VM, install Wireshark
    - [WireShark](https://www.wireshark.org/)
  - Open WireShark and filter for ICMP traffic only
  - From the Windows 10 VM, open command line or Powershell and attempt to ping a public website (www.google.com) and observe the traffic in Wireshark
  
</p>
<br />

<p>
<img src="https://i.imgur.com/5bxSUBv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
- Retrieve the private IP address of the Ubuntu VM and attmpt to ping from within the Windows 10 VM
- Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM
  - in cmd, type ping -t and then the private IP address

</p>
<br />

<p>
<img src="https://i.imgur.com/GTnYb5K.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p> 
  
- Open the Network Security Group your Ubuntu VM is using and disable incoming ICMP traffic
  - Azure portal -> search Network Security Group -> Inbound Security Rules -> Add (a rule) -> click ICMP -> Deny -> set priority to 200 -> name the rule (Deny_IMCP_Ping_From_Anywhere) -> Add

</p>
<br />

<p>
<img src="https://i.imgur.com/Z0wE7Et.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity
  - Stop the Ping activity
  
</p>
<br />

<p>
<img src="https://i.imgur.com/mCewOSj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe SSH Traffic
  - Back in WireShark, filter for SSH traffic only
  - From your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address)
    - cmd -> ssh labuser@10.0.0.6 (the private ip) -> enter -> yes -> type the password -> enter

</p>
<br />

<p>
<img src="https://i.imgur.com/xscoDom.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe DHCP Traffic
  - Back in WireShark, filter for DHCP traffic only
  - From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)
    - cmd -> ipconfig /renew -> enter

</p>
<br />

<p>
<img src="https://i.imgur.com/k0E13jN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe DNS Traffic
  - Back in WireShark, filter for DNS traffic only
  - From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com's IP addresses are
    - cmd -> nslookup www.google.com -> enter

</p>
<br />

<p>
<img src="https://i.imgur.com/mrvY5ef.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Observe RDP Traffic
  - Back in WireShark, filter for RDP traffic only (tcp.prt==3389)
  - Observe the immediate non-stop spam of traffic. RDP is constantly showing you a live stream from one ocomputer to another, therefor traffic is always being transmitted

</p>
<br />


