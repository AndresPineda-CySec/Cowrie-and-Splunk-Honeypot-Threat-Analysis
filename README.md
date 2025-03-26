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


<h3 align="center">Securing My Mac Mini:</h3>
<p align="center">
<br />
<br />
Before deploying my honey pot, I want to make sure to secure my host machine, first, and then my virtual machine. I start by accessing the built in firewall macOS offers. I turn on stealth mode, to help prevent detection of my host machine.
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Stealth_scan.png?raw=true" height="80%" width="80%"/> <br />
Stealth scan is now on. keep in my mind, this is does not block any potential threats, it only helps keep the machine hidden from pings or port scans.<br />
<br />
<br />
<!--Next, I will create a segmented virtual network interface using the pf firewall and tuntap virtual network interfaces. This will allow me to isolate my ubuntu honeypot while still providing internet access, effectively simulating VLAN-like seperation.<br /> !-->
<br />
<br />
<img src="" height="80%" width="80%"/> <br />


