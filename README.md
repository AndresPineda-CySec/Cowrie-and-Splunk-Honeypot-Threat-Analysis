# Cowrie and Splunk: Honeypot Threat Analysis
<h2>Description</h2>
This PowerShell script collects and exports critical system information into CSV files for security auditing and incident response. The script is triggered by specified tasks within the Task Scheduler, which will automatically run the script whenever one of the tasks is triggered. This project captures a snapshot of the system's state during potential security incidents for further analysis. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>VirtualBox</b> 
- <b>Cowrie</b>
- <b>Splunk</b>

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

<!--
<p align="center">
The Complete Script:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/images/Full.png?raw=true" height="200%" width="200%"/> <br />
<br />
<br />
<h3 align="center">Script Walk-Through:</h3>
<p align="center">
Lines 1-5:<br />
<img src="https://github.com/AndresPineda-CySec/PowerShell-Incident-Response-Script/blob/main/images/L1.png?raw=true" height="200%" width="200%"/> <br />
Lines 1 and 4 create variables to store the directory paths, simplifying the script by referencing these paths later. Line 3 defines how the folder inside "IR_Reports" will be named using a timestamp. Line 5 then creates the folder at the specified path, ensuring it is a directory, and uses the command "-Force" to overwrite or create the folder no matter what.
<br />
--!>
