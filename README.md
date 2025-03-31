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


<h3 align="center">Securing My Enviornments:</h3>
<p align="center">
<br />
<br />
Before deploying my honeypot, I want to secure my environment to minimize risks and ensure a controlled setup. The first step is to configure my virtual machine to run through a Network Address Translation (NAT) network. NAT acts as a protective barrier by isolating the VM from direct exposure to my home network while still allowing outbound connections. This setup helps safeguard my host machine from potential threats while maintaining the functionality needed for my honeypot deployment.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Ensure%20NAT%20is%20enabled.png?raw=true" height="80%" width="80%"/> <br />
I can verify the network type my virtual machine is using by selecting it in the left column and scrolling down to the Network section. There, I can confirm that NAT is enabled, ensuring my VM is properly isolated while maintaining necessary connectivity.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Stealth_scan.png?raw=true" height="80%" width="80%"/> <br />
To implement Defense in Depth, I will start by securing my host machine with a simple yet effective step: enabling Stealth Mode. While Stealth Mode does not actively block threats, it helps obscure my Mac from network discovery by preventing it from responding to pings and port scans, reducing its visibility to potential attackers.<br />
<br />
<br />
Next, I will block any inbound traffic from VirtualBox while still allowing outbound traffic. While NAT already provides this layer of protection, adding an explicit firewall rule ensures an extra safeguard against any misconfigurations or unforeseen vulnerabilities. In cybersecurity, it’s always better to be safe than sorry, and this further aligns with the principles of Defense in Depth.<br />
<!--Next, I will create a segmented virtual network interface using the pf firewall and tuntap virtual network interfaces. This will allow me to isolate my ubuntu honeypot while still providing internet access, effectively simulating VLAN-like seperation.<br /> !-->
<br />
<br />
<img src="" height="80%" width="80%"/> <br />


