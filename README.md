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
- <b>Principle of Least Privilege</b>

<h2>Project Walk-Through:</h2>


<h3 align="center">Securing My Environment:</h3>
<p align="center">
<br />
<br />
Before deploying my honeypot, I want to secure my environment to minimize risks and ensure the project runs safely. The first step is configuring VirtualBox to run my Ubuntu virtual machine (VM) using a Bridged Network adapter. This setup allows the VM to connect directly to the router, making it WAN-facing and accessible from the internet. Since this can introduce potential security risks, I’ve isolated the VM within a Demilitarized Zone (DMZ) on my network. This provides an extra layer of security by segmenting the VM from the rest of my internal devices, helping to contain any threats and prevent lateral movement across my home network.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/BridgeAdapter.png?raw=true" height="80%" width="80%"/> <br />
To change the network adapter settings in VirtualBox, I start by selecting my Ubuntu virtual machine and navigating to the "Settings" menu. From there, I go to the "Network" tab, where I change the "Attached to" option from the default NAT setting to "Bridged Adapter." This configuration allows the VM to communicate directly with devices on the network and receive its own IP address from the router. Additionally, I enable "Promiscuous Mode" to allow the VM to capture all network traffic on the interface, which is useful for monitoring and logging potential attacks.<br />
<br />
<br />
Next, I must configure my router's firewall to complete two tasks: First, port forwarding 22 (SSH) and 23 (Telnet) to my Ubuntu VM. placing my Ubuntu VM into a DMZ. To complete this setup, I must start my Ubuntu VM and run the "ifconfig" command to identify its local IP address. While my router should automatically detect the VM as a connected device, I use the command to verify the connection and confirm the correct IP address before configuring port forwarding and placing the VM in the DMZ.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/ifconfig.png?raw=true" height="80%" width="80%"/> <br />
I run the "ifconfig" command and note my VM's IP address. While my router should automatically display the correct hostname and IP address in the firewall settings, it's always best to manually verify that the IP addresses match to avoid misconfiguration.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/DMZ.png?raw=true" height="80%" width="80%"/> <br />
Now I can log in to my router. Once Logged in, I place my VM into a DMZ...<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/PortFowarding.png?raw=true" height="80%" width="80%"/> <br />
... and I create two port forwarding rules: forwarding 22 and 23 to my Ubuntu VM.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Stealth_scan.png?raw=true" height="60%" width="60%"/> <br />
Another way I can protect my host machine is by enabling Stealth Mode in the Mac firewall settings. While Stealth Mode does not actively block threats, it helps obscure my Mac from network discovery by preventing it from responding to pings and port scans, reducing its visibility to potential attackers.<br />
<br />
<br />
Now, I will move on to securing my Ubuntu VM.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/NotRoot.png?raw=true" height="80%" width="80%"/> <br />
Before proceeding, I want to ensure that the only user on my VM is not root. This adds an extra layer of security, helping protect my VM in case my honeypot is compromised and reducing the risk of a VM escape affecting my host machine. To verify this, I ran the "id" command and confirmed that my UID and GID are both 1000. This indicates that the account is a standard user with limited system access, reinforcing the principle of least privilege.<br />
<br />
<br />
After confirming that my user is not root, I updated the OS and all installed packages. I do this by first running the command "sudo apt update" and then running the command "sudo apt upgrade." :
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/checkUpdate.png?raw=true" height="100%" width="100%"/> <br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/update.png?raw=true" height="100%" width="100%"/> <br />
<br />
<br />  
Now, I will configure my VM's firewall to block all inbound traffic except for traffic on ports 2222 and 2223, the default ports Cowrie listens on (emulating 22 and 23), and ports 22 and 23, the default SSH port. Keeping port 22 open can be risky, but I will eventually route SSH to a different port and forward any traffic from port 22 to Cowries default 2222; I will repeat this with 23 just to be safe. This step reduces the attack surface of my VM by limiting exposure to only the necessary traffic, ensuring that Cowrie processes all successful connections while all other attempts are blocked.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/FireWallConfig.png?raw=true" height="50%" width="50%"/> <br />
I open Terminal in my Ubuntu VM and check whether the Uncomplicated firewall (UFW) is enabled: it's not. The first step is to enable the firewall. Once enabled, I deny all inbound traffic while allowing outbound traffic. Next, I create a rule to permit inbound traffic on ports 2222, 2223, 22, and port 23, ensuring that only connections intended for Cowrie are accepted. Finally, I verify that my firewall rules have been successfully updated to confirm the changes are in effect.<br />
<br />
<br />
The next step is to forward any traffic for 22 to 2222 and 23 to 2223. Keeping ports 22 and 23 open will make this honeypot more desirable to potential threat actors and a more realistic target. This command utilizes "iptables" to reroute any SSH or TELNET traffic to Cowrie's default ports, 2222 and 2223:<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/PortFoward.png?raw=true" height="100%" width="100%"/> <br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Route23.png?raw=true" width="100%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/nano.png?raw=true" height="50%" width="50%"/> <br />
The next step is to access the SSH config file to change the default SSh port through nano.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/NanoPortChange.png?raw=true" height="80%" width="80%"/> <br />
Once in the config file, I uncommented "port 22" to change it to a random unused port number; I chose port 9444. 
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/DefaultCreds.png?raw=true" height="80%" width="80%"/> <br />
The final step in hardening my environment before installing Cowrie is creating a new user specifically for running Cowrie. I achieved this by running the command "sudo adduser --disabled-password cowrie," which creates the "cowrie" user as a restricted account. The "--disabled-password" flag disables password authentication, minimizing the risk of unauthorized access. This action improves security by enforcing least privilege, preventing VM escapes, and isolating services, ensuring that even if the honeypot is compromised, my system remains protected.
<br />
<br />
<br />
<br />
<h3 align="center">Installing Cowrie:</h3>
<p align="center">
<br />
<br />
Before installing Cowrie, I have to make sure I install a few dependencies to ensure Cowrie can run efficiently.
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/installDependency.png?raw=true" height="80%" width="80%"/> <br />
This command installs essential dependencies for setting up Cowrie. It installs "git" for version control, "python3-venv" for creating isolated Python environments, "libssl-dev" for cryptographic functions, "libffi-dev" for interfacing with C libraries, "build-essential" for compiling software, "libpython3-dev" for Python development headers, and "python3-minimal" for the minimal Python 3 installation required to run Python applications. These packages ensure that Cowrie runs securely and has all the necessary tools for building and interacting with Python code.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/useCowrieUser.png?raw=true" height="50%" width="50%"/> <br />
Now that the dependencies are installed, the next step is to switch to the Cowrie user. I’ll know I’m using the Cowrie user because the terminal prompt will update to reflect the change in the username. This confirms I’m operating within the restricted Cowrie account and ready to continue with the installation process.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/downloadCowire.png?raw=true" height="80%" width="80%"/> <br />
I can now install Cowrie using this command.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/createVenv.png?raw=true"height="80%" width="80%"/> <br />
I set up a Python virtual environment for Cowrie to isolate it from the system. First, I navigated to the Cowrie directory and created a virtual environment using "python3 -m venv cowrie-env." Then, I activated it with "source cowrie-env/bin/activate," ensuring that installed packages remain contained within this environment. This helps improve security by preventing dependency conflicts and limiting the impact of a potential compromise.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/pip.png?raw=true"height="80%" width="80%"/> <br />
I upgraded "pip" within the virtual environment using this command. "install --upgrade" ensures "pip" is installed and updates it to the latest version if an older one exists. This helps prevent compatibility issues and keeps the environment secure with the latest fixes and features.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/pipReq.png?raw=true"height="80%" width="80%"> <br />
I then install the "requirements.txt" file to ensure all necessary packages are up to date and properly configured within the virtual environment.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/copyCowrieConfigFile.png?raw=true"height="80%" width="80%"/> <br />
After updating Cowrie, I need to edit its configuration file. I navigate to the "etc" directory, list the files, and create a copy of "cowrie.cfg.dist," renaming it to "cowrie.cfg." This allows me to customize settings while preserving the default configuration as a backup.<br />
<br />
<br />
In the same directory, I edit the "cowrie.cfg" file using the command nano cowrie.cfg."<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/CowrieHostName.png?raw=true"height="80%" width="80%"/> <br />
I edit the cowrie.cfg file and change the hostname to something more realistic, naming it "ubuntu-server-08" to better mimic a real server setup.<br /> 
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/SSH-SSL-UbuntuVers.png?raw=true"height="80%" width="80%"/> <br />
Since the config file originally listed a Debian OS and I changed the hostname to "ubuntu-server-08," I now have to update the OS to Ubuntu to match. Additionally, I updated the OpenSSH and OpenSSL versions to slightly newer ones than initially listed, ensuring the configuration reflects a more current system setup.<br />
<br />
<br />
With these changes in the config file, my honeypot will appear more authentic, making it more likely to attract potential attackers.<br />
<br />
<br />
<br />
<br />
<h3 align="center">Configuring Splunk:</h3>
<p align="center">
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/splunkAddData.png?raw=true" height="80%" width="80%"/> <br />
To integrate Cowrie with Splunk, I must first create an HTTP Event Collector (HEC) in Splunk. I start by navigating to "Settings" and selecting "Add Data" to begin the setup process.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/SplunkAddMonitor.png?raw=true" height="80%" width="80%"/> <br />
This takes me to the data input page, where I select the "Monitor" option to continue setting up the HTTP Event Collector.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/SplunkHTTPEventCollector.png?raw=true" height="80%" width="80%"/> <br />
After selecting "Monitor," I arrive at the "Add Data" page. Here, I choose "HTTP Event Collector" from the left panel and set the new HEC name to "Cowrie." I keep the default settings for the remaining configurations and click "Next" to proceed through the setup steps.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/SplunkToken.png?raw=true" height="80%" width="80%"/> <br />
Once the HEC is configured, I reach the "Done" page, where my HEC token value is displayed. I make sure to take note of the token, as I will need to add it to my "cowrie.cfg" file in my Ubuntu VM to enable log forwarding to Splunk.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Indexes.png?raw=true" height="80%" width="80%"/> <br />
Next, I need to create a new index for my Cowrie integration to ensure that logs are stored separately and can be easily queried within Splunk. To do this, I go to "Settings" and select "Indexes."<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/NewIndex.png?raw=true" height="80%" width="80%"/> <br />
On the "Indexes" page, I created a new index named "Cowrie." I keep the default settings for the remaining options and save the new index.<br />
<br />
<br />
Now that the index is created, I need to assign it to the Cowrie HEC to ensure that logs from Cowrie are correctly stored in the newly created index.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/backToDataInput.jpeg?raw=true" height="80%" width="80%"/> <br />
I go back to "Data Inputs" found within "Settings."<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/SelectHEC.png?raw=true" height="80%" width="80%"/> <br />
On the "Data Inputs" page, I select "HTTP Event Collector" to view the "Cowrie HEC" I created earlier.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/assignIndex.png?raw=true" height="80%" width="80%"/> <br />
On the "HTTP Event Collector" page, I select my Cowrie HEC to edit it. I then assign the Cowrie index I created to ensure that logs are properly stored in the correct location.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/EnableHEC.png?raw=true" height="80%" width="80%"/> <br />
By default, the HEC is disabled, so I need to enable it. I do this by selecting "Global Settings" and selecting "Enabled."<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/ScConfirmHECisEnabkled.png?raw=true" height="80%" width="80%"/> <br />
Once saved, I can see that the HEC is enabled.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/ManageApps.png?raw=true" height="80%" width="80%"/> <br />
Now I need to install the <a href="https://www.dropbox.com/scl/fi/jupaef16uhgvvvufkc6vz/ManukaHoneyPot.tar.gz?rlkey=k6hy5tyxxhggxttvv0j3frmjt&e=1&dl=0">ManukaHoneyPot</a> app into Spunk, which will allow me to visualize Cowrie logs and create a dashboard. To begin, I select "Apps" at the top left of the screen, then choose "Manage Apps" from the drop-down menu.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/InstallAppByFile.png?raw=true" height="80%" width="80%"/> <br />
I am now on the "Apps" page, where I can install the ManukaHoneyPot app by selecting "Install app from file."<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/ChooseManukaHoneyPot.png?raw=true" height="80%" width="80%"/> <br />
Once selected, I can Select the app by browsing my local files.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/AppinstallNoti.png?raw=true" height="80%" width="80%"/> <br />
Once the installation is complete, I am redirected to the "Apps" page, where a confirmation message indicates that the ManukaHoneyPot app was successfully installed. Here I can select the newly installed app.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/EmptyDashBoard.png?raw=true" height="80%" width="80%"/> <br />
After selecting the ManukaHoneyPot app, I am brought to an empty dashboard. Once Cowrie is up and running, the dashboard will populate with logs. These are the main steps in configuring and preparing Splunk for integration with Cowrie.<br />
<br />
<br />
<br />
<br />
<h3 align="center">Exporting Cowrie Logs to Splunk:</h3>
<p align="center">
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Cowrie.cfg.png?raw=true" height="80%" width="80%"/> <br />
To link my honeypot to Splunk, I first access my Ubuntu VM and switch to the "cowrie" user through the terminal. From there, I navigate the directory to get to the Cowrie configuration file and use the command "nano cowrie.cfg" to access cowrie.cfg.<br /> 
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/OriginalCowrieSplunkconfig.png?raw=true" height="80%" width="80%"/> <br />
Inside the config file, I located the Splunk output section, which was initially disabled and configured with the incorrect URL and token; both need to be updated for proper integration.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/updated%20cowrie:splunk.png?raw=true" height="80%" width="80%"/> <br />
I update the "Output_Splunk" section by replacing the default URL with my host machine's IP address and inserting the correct token from my Cowrie HEC in Splunk. This ensures that Cowrie logs are sent to the right destination for analysis.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/virusTotalAPIKey.png?raw=true" height="80%" width="80%"/> <br />
The ManukaHoneyPot app includes a feature that allows integration with VirusTotal directly within the dashboard. To link VirusTotal with Cowrie, I must retrieve my API key from my VirusTotal account.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/OgVirusTotalConfig.png?raw=true" height="80%" width="80%"/> <br />
Once I retrieve my API key, I can return to the "cowrie.cfg" file and configure the "output_virustotal" section. By default, this output is disabled and contains a placeholder API key.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/UpdatedVTConfigCowrie.png?raw=true" height="80%" width="80%"/> <br />
I enabled the section and replaced the default placeholder API key with my personal API key from my VirusTotal account.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/EnableTelnet.png?raw=true" height="80%" width="80%"/> <br />
Last but not least, I'm finally going to enable the telnet function in Cowrie within the "cowrie.cfg" file.
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Start%20Cowrie.png?raw=true" height="80%" width="80%"/> <br />
That’s it! With everything configured, I can now start Cowrie and leave the honeypot running for a day or so to begin collecting data. I use this command to start Cowrie.<br />
<br />
<br />
<br />
<br />
<h3 align="center">Results:</h3>
<p align="center">
After Allowing Cowrie to run for two days straight, these are the results:
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/CowrieCommandLog.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/CowrieLog.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man1.png?raw=truer" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man2.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man3.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man%204.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man5.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man6.png?raw=true" height="80%" width="80%"/> <br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Man7.png?raw=true" height="80%" width="80%"/> <br />
