# Cowrie and Splunk: Honeypot Threat Analysis
<h2>Description</h2>
In this project, I run Ubuntu in VirtualBox and deploy a honeypot to capture potential threats. All logs are exported to a SIEM for clear visualization and analysis. To enhance security, I implement multiple techniques to protect both the virtual machine and the host system, following a defense-in-depth approach.
<br />


<h2>Utilities Used</h2>

- <b>VirtualBox</b> 
- <b>Cowrie</b>
- <b>Splunk</b>
- <b>Terminal</b>

<h2>Environments Used</h2>

- <b>macOS</b>
- <b>Ubuntu</b>

<h2>Skills Demonstrated</h2>

- <b>Honeypot Deployment and Management</b>
- <b>Threat Detection and Analysis</b>
- <b>Log Collection and fowarding</b>
- <b>Security Information and Event Management</b>
- <b>Data Visualization and Dashboard Creation</b>
- <b>Network Security Monitoring</b>
- <b>Threat Intelligence</b>
- <b>Network Segmentation</b>
- <b>Defense in Depth</b>

<h2>Project Walk-Through:</h2>


<h3 align="center">Securing My Environment:</h3>
<p align="center">
<br />
<br />
Before deploying a honeypot, I want to secure my environment to minimize risks and ensure my project runs safely. The first step is to make sure VirtualBox (VM) is running my Ubuntu virtual machine (VM) through a Network Address Translation (NAT) network. NAT acts as a protective barrier by isolating the VM from direct exposure to my home network while allowing outbound connections. This setup helps safeguard my host machine from potential threats while maintaining the functionality needed for my honeypot deployment.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Ensure%20NAT%20is%20enabled.png?raw=true" height="80%" width="80%"/> <br />
I can verify the network type my virtual machine uses by selecting it in the left column and scrolling down to the Network section. There, I can confirm that NAT is enabled, ensuring my VM is properly isolated while maintaining necessary connectivity.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Stealth_scan.png?raw=true" height="80%" width="80%"/> <br />
Another way i can protect my my host machine is a simple yet effective step: enabling Stealth Mode in the Mac firewall. While Stealth Mode does not actively block threats, it helps obscure my Mac from network discovery by preventing it from responding to pings and port scans, reducing its visibility to potential attackers.<br />
<br />
<br />
Now, I will configure my VM's firewall to block all inbound traffic except for traffic on port 2222, the default port my Cowrie honeypot will use, and port 22, the default SSH port. Keeping port 22 open can be risky, but I will eventually route SSH to a different port and forward any traffic from port 22 to Cowries default 2222. This step reduces the attack surface of my VM by limiting exposure to only the necessary traffic, ensuring that Cowrie processes all successful connections while all other attempts are blocked.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/firewallConfig.png?raw=true" height="80%" width="80%"/> <br />
I open Terminal in my Ubuntu VM and check whether the Ubuntu firewall (UFW) is enabled, but it's not. The first step is to enable the firewall. Once enabled, I deny all inbound traffic while allowing outbound traffic (for now). Next, I create a rule to permit inbound traffic on port 2222 and port 22, ensuring that only connections intended for Cowrie are accepted. Finally, I verify that my firewall rules have been successfully updated to confirm the changes are in effect.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/PortFoward.png?raw=true" height="80%" width="80%"/> <br />
The next step is to forward any traffic for 22 to 2222. Keeping port 22 open will make this honeypot more desirable to potential threat actors and a more realistic target. This command utilizes iptables to reroute any SSH traffic to Cowrie's default port, 2222.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/nano.png?raw=true" height="50%" width="50%"/> <br />
The next step is to access the SSH config file to change the default SSh port through nano.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/NanoPortChange.png?raw=true" height="80%" width="80%"/> <br />
Once in the config file, I uncommented "port 22" to change it to a random unused port number; I chose port 9444. 


