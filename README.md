<p align="center">
<img src="https://i.imgur.com/Pe5GT2M.png" height="40%" width="60%"/>
</p>

<h1>Inspecting Network Protocols (ICMP, SSH, DHCP, DNS, and RDP) with Wireshark</h1>
In this project, I observed various network traffic to and from Azure Virtual Machines using Wireshark and experimented with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Network Security Groups)
- Remote Desktop
- PowerShell/Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create 2 virtual machines, one with Windows 10 and the other with Ubuntu
-	Connect to the Windows 10 VM using Remote Desktop
-	Install Wireshark within the Windows 10 VM
-	Initiate a non-stop ping to the Ubuntu VM, add an inbound rule in the Ubuntu VM’s Network Security Group to deny ICMP traffic, and observe the effect in Wireshark
-	Re-enable ICMP traffic in the Ubuntu VM’s Network Security Group, observe the effect in Wireshark, then end the ping activity
-	SSH into the Ubuntu VM and observe the effect in Wireshark, then exit SSH
-	In the Windows 10 VM, attempt to issue the VM a new IP address using ipconfig /renew and observe DHCP traffic in Wireshark
-	Use the nslookup command and observe DNS traffic in Wireshark
-	Observe ongoing RDP traffic in Wireshark
-	Delete Resource Groups in Azure


<h2>Synopsis</h2>

<p>
<img src="https://i.imgur.com/PuaPt3D.png" height="70%" width="70%"/>
</p>
<p>
In order to do this lab, I first needed to create two virtual machines, one with Windows 10 and another with Ubuntu. After I created the virtual machines, I viewed them in Network Watcher to make sure they were in the same vnet (virtual network) and subnet. VM1 is the virtual machine with Windows 10 and VM2 is the virtual machine with Ubuntu.
</p>
<br />

<p>
<img src="https://i.imgur.com/Ti3apo4.png" height="70%" width="70%"/>
</p>
<p>
In the top right corner is the public IP address, which I copied and pasted into Remote Desktop in order to connect to VM1 and use it for the lab.
</p>
<br />

<p>
<img src="https://i.imgur.com/qZAaOKM.png" height="70%" width="70%"/>
</p>
<p>
Once I was in VM1, I used Microsoft Edge to install Wireshark, opened it, and also opened Windows PowerShell. I then filtered for ICMP in Wireshark (in the green line) and initiated a non-stop ping to VM2 using its private IP address. For each ping, Wireshark shows the request from VM1 to VM2 and the reply from VM2 to VM1!
</p>
<br />

<p>
<img src="https://i.imgur.com/r2sTBjV.png" height="70%" width="70%"/>
</p>
<p>
After making sure that I could ping VM2, I went to the Network Security Group for VM2 and added an inbound security rule preventing VM2 from receiving ICMP traffic, meaning that it would not be able to receive pings.
</p>
<br />

<p>
<img src="https://i.imgur.com/hqFeBvp.png" height="70%" width="70%"/>
</p>
<p>
Well, look at that! Now that VM2 cannot receive ICMP traffic, the ping requests are timing out. In Wireshark under “Info,” we can see that there are still requests being sent by VM1, but there are no more replies from VM2, along with a “no response found!” message.
</p>
<br />

<p>
<img src="https://i.imgur.com/jDPXYbg.png" height="15%" width="15%"/>
</p>
<p>
I went back to VM2’s Network Security Group and clicked on the rule that I had created and selected the “Allow” option under “Action” so that incoming ICMP traffic would be allowed to VM2 again.
</p>
<br />

<p>
<img src="https://i.imgur.com/rSfEOeL.png" height="70%" width="70%"/>
</p>
<p>
Now that ICMP traffic was allowed again, both the PowerShell terminal and Wireshark showed that replies were again being sent by VM2. I was done observing ICMP traffic in Wireshark, so I ended the ping activity in PowerShell.
</p>
<br />

<p>
<img src="https://i.imgur.com/0gWpLSn.png" height="70%" width="70%"/>
</p>
<p>
I then filtered for SSH traffic in Wireshark and used the PowerShell terminal to “SSH into” VM2. Connecting to VM2 using SSH, along with typing and executing commends, generated SSH packets that could be observed in Wireshark. Using the “exit” command, I ended the SSH session and returned to the original terminal.
</p>
<br />

<p>
<img src="https://i.imgur.com/4jzRJ1P.png" height="70%" width="70%"/>
</p>
<p>
In order to view DHCP traffic, I filtered for DHCP traffic in Wireshark and used the “ipconfig /renew” command to attempt to issue a new IP address to VM1. Although the private IP address did not change, Wireshark shows that there was a request and acknowledgement, so DHCP traffic was generated.
</p>
<br />

<p>
<img src="https://i.imgur.com/wrRxGbT.png" height="70%" width="70%"/>
</p>
<p>
In Wireshark, I then filtered for DNS traffic and used the “nslookup” command for google.com. In the terminal, we can see that it returned both IPv4 and IPv6 addresses; in Wireshark under “Info,” we can see that there are A and AAAA records, which corresponds to the types of IP addresses we see!
</p>
<br />

<p>
<img src="https://i.imgur.com/mFVXr7c.png" height="70%" width="70%"/>
</p>
<p>
Lastly, I used Wireshark to view RDP traffic, which I did not need to use the PowerShell terminal for since RDP traffic was already being generated by my physical computer being connected to the VM. Since this was an active connection, RDP traffic was continually generated, and Wireshark showed each packet being sent in the moment. For the previous protocols, I filtered for them in Wireshark using its name (e.g. “DNS”); for RDP traffic, I decided to filter using its port number (tcp.port == 3389), and either way it works, as long as you know the port!
</p>
<br />

<p>
😄 All done! This lab was the first time I used Wireshark, and it was pretty fun. I definitely recommend those new to IT to give it a try, it’s pretty cool to see all the packets you capture!
</p>
