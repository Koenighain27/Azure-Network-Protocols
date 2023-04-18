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
- Ubuntu Server 20.04


<h2>Actions and Observations</h2>

First, we'll need to create two VMs using Azure. One running Windows 10 and the other running Ubuntu. When creating them, make sure they are both on the same Vnet. After that, we can check the Network Watcher and observe that both VMs are on the same network. 
<p>
<img src="https://imgur.com/LnJEKd8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

Next, connect to VM1 using RDP. Go to this link: https://www.wireshark.org/download.html and download Wireshark onto VM1. We'll be using this to inspect network traffic between the two VMs. Once downloaded, click Ethernet and then click "start capturing packets". Observe the traffic. 
<p>
<img src="https://imgur.com/Bk8wNL5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

Moving on, we're now going to filter the packet capture to show only ICMP traffic. Type "icmp" in the filter bar. ICMP is a protocol that is used by network devices to communicate connection issues. We're going to ping VM2 from VM1 by obtaining VM2's private IP address. This can be found on the Azure portal. Open the command prompt on VM1 and type, "ping" and then VM2's private IP address. Observe the traffic on Wireshark.
<p>
<img src="https://imgur.com/yWNw2Ep.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://imgur.com/LDliLWc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<br />

Next, initiate a perpetual ping between VM1 and VM2 by typing "ping x.x.x.x -t". Once thats going, we are going to change the firewall settings on VM2 to block ICMP traffic. Search "Network Security Groups" on the Azure portal and choose VM2. Then click "Inbound security rules" and then add the rule. Observe the traffic and notice that the request timed out and on Wireshark, it shows that no response was found. If we delete the rule or allow ICMP traffic again, then the ping will receive a reponse from VM2 and the request will no longer time out. Stop the perpetual ping by typing CTRL+C.
<p>
<img src="https://imgur.com/LTEOIuf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://imgur.com/sgcyX21.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<br />

Great! Lets look at some other types of traffic. Type "SSH" into the filter. Open up Powershell and type "ssh username@x.x.x.x". The username and IP address will be different than mine. Observe the traffic between VM1 and VM2.
<p>
<img src="https://imgur.com/d4Vg0D3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />

Lets now look at DHCP and DNS traffic. Change the filter to DHCP and from VM1's command prompt, type "ipconfig /renew". This will attempt to issue the VM a new IP address. Next, filter for DNS traffic only. DNS translate domain names to IP addresses. We can use the "nslookup" command to ask the DNS server for an IP address of certain domain names.
<p>
<img src="https://imgur.com/PKkTO7s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://imgur.com/huOTp5T.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
</p>
<br />

Finally, lets take a look at RDP traffic. Filter for RDP traffic by typing "tcp.port == 3389". Observe the traffic and notice that packets are continually being captured. This is because we are connected to the VM using RDP, therefore traffic is always being transmitted.
<p>
<img src="https://imgur.com/KAlDYPg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
</p>
<br />
We can play around further and continue to look at different types of traffic and we can see how useful Wireshark is to view packets. Hope you found this tutorial interesting!
