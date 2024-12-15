
<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Linux Ubuntu Server 20.04

<h2>High-Level Steps</h2>

Create Resources
Observe ICMP Traffic
Observe SSH Traffic)
Observe DHCP Traffic
Observe RDP Traffic

<h2>Actions and Observations</h2>

<p>Resource Groups (similar to a file system) logical collections of virtual machines, storage accounts, virtual networks, web apps, databases, and/or database servers.  
Virtual Machines (VM) allow you to more easily scale your applications by adding more physical or virtual servers to distribute the workload across multiple VMs. We created two Virtual Machines (pictured below) of differing Operating Systems (Windows 10 21H2 & Linux Ubuntu Server 20.04) that will be used for Remote Deskop and to observe network traffic between the two devices. 
</p>

<p align="center">
<img src="https://i.imgur.com/BifIhxG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>Remote desktop allows the user to connect to a computer in another location, see that computer's desktop and interact with it as if it were local. A quick search for "remote desktop connection" will allow the user to access the VM. Here we will be entering the details of the public IP address for VM1 (Windows 10 21H2) to install Wireshark (packet analysis software) instead of using our local machine. (below pictured search of remote desktop and the result to enter IP address)</p>

<p align="center">
<img src="https://i.imgur.com/PEeQyYV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

On the virtual machine with Windows 10, download Wireshark (Windows Installer 64-bit) and continue with all the default options.

Npcap will pop up to install, go ahead and install that with defaults and wireshark will continue to install after.

Open Wirehsark in the VM, click Ethernet and then click the blue fin at the top left under 'File' to begin capturing packets. Notice all the traffic already happening that happens in the background.

<br />
<p>
After retrieving the private IP address from VM2 (Linux Ubutu Server 20.04) we can now ping that private IP address from VM1 (Windows 10 21H2) that we've used to remote into. We can use the ping command to test the connection between machines for connectivity. So we can now view the traffic travel from VM1 to VM2 by filtering the ICMP packets in Wireshark. We can also ping other IP address or a domain names (www.google.com). The filtered traffic that is requested and its corresponding reply is shown below in Wireshark is pictured (left) and Powershell (right):
</p>
<p align="center">
<img src="https://i.imgur.com/SriHAg2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>If we want to deny the ping request we can add this rule to our Network Security Group inside the Virtual Machine and once we've added this rule to VM2, we can see that the traffic times out in PowerShell along with Wireshark longer displaying a reply to this request. 
  </p>
  <p align="center">
  <img src="https://i.imgur.com/mTtkBrg.png" height="80%" width="80%" alt="ping traffic"/>
  </p>
  <br/>
  <p>Wireshark and PowerShell timed out after denying icmp (ping) traffic</p>
  <p align="center">
<img src="https://i.imgur.com/8epnq3H.png" height="80%" width="80%" alt="icmp traffic deny"/>
</p>
</br>
<p> We can filter in Wireshark, with a the filter functionality we can filter SSH only traffic. From the Windows 10 VM, we enter SSH into Linux Virtual Machine (VM2) (using "ssh username@ip address" its private IP address). When we use commands such as touch, pwd (print working directory) or ls (list), into the linux SSH was used to connect. SSH traffic is observed spamming in WireShark. The SSH connection can be exited, by typing ‘exit’ and pressing [Enter].
</p>
<p align="center">
<img src="https://i.imgur.com/DpJdcZl.png" height="80%" width="80%" alt="ssh traffic"/>
</br>
<p> We can filter in Wireshark for "DHCP traffic only". From VM1 (Windows 10 21H2), a new IP address was issued from the command line (ipconfig /renew). Now DHCP traffic can be observed in WireShark.</p>
<p align="center">
<img src="https://i.imgur.com/GLm7cMT.png" height="80%" width="80%" alt="dhcp traffic"/>
</p>
</br>
<p> In Wireshark, we can filter for RDP traffic only (tcp.port == 3389) because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefore traffic is consistently being transmitted </p>
<p align="center">
<img src="https://i.imgur.com/rB07Fw7.png" height="80%" width="80%" alt="tcp 3389"/>
</p>
