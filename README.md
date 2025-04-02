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
Before deploying a honeypot, I want to secure my environment to minimize risks and ensure my project runs safely. The first step is to make sure VirtualBox (VM) is running my Ubuntu virtual machine (VM) through a Network Address Translation (NAT) network. NAT acts as a protective barrier by isolating the VM from direct exposure to my home network while allowing outbound connections. This setup helps safeguard my host machine from potential threats while maintaining the functionality needed for my honeypot deployment.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Ensure%20NAT%20is%20enabled.png?raw=true" height="80%" width="80%"/> <br />
I can verify the network type my virtual machine uses by selecting it in the left column and scrolling down to the Network section. There, I can confirm that NAT is enabled, ensuring my VM is properly isolated while maintaining necessary connectivity.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/Stealth_scan.png?raw=true" height="60%" width="60%"/> <br />
Another way I can protect my host machine is by enabling Stealth Mode in the Mac firewall settings. While Stealth Mode does not actively block threats, it helps obscure my Mac from network discovery by preventing it from responding to pings and port scans, reducing its visibility to potential attackers.<br />
<br />
<br />
Now, I will move on to my Ubuntu VM.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/NotRoot.png?raw=true" height="80%" width="80%"/> <br />
Before proceeding, I want to ensure that the only user on my VM is not root. This adds an extra layer of security, helping protect my VM in case my honeypot is compromised and reducing the risk of a VM escape affecting my host machine. To verify this, I ran the "id" command and confirmed that my UID and GID are both 1000. This indicates that the account is a standard user with limited system access, reinforcing the principle of least privilege.<br />
<br />
<br />
After confirming that my user is not root, I proceeded to update the OS and all installed packages. I do this by first running the command "sudo apt update" and then running the command "sudo apt upgrade." :
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/checkUpdate.png?raw=true" height="100%" width="100%"/> <br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/update.png?raw=true" height="100%" width="100%"/> <br />
<br />
<br />  
Now, I will configure my VM's firewall to block all inbound traffic except for traffic on port 2222, the default port my Cowrie honeypot will use, and port 22, the default SSH port. Keeping port 22 open can be risky, but I will eventually route SSH to a different port and forward any traffic from port 22 to Cowries default 2222. This step reduces the attack surface of my VM by limiting exposure to only the necessary traffic, ensuring that Cowrie processes all successful connections while all other attempts are blocked.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/UFWConfig.png?raw=true" height="50%" width="50%"/> <br />
I open Terminal in my Ubuntu VM and check whether the Uncomplicated firewall (UFW) is enabled: it's not. The first step is to enable the firewall. Once enabled, I deny all inbound traffic while allowing outbound traffic (for now). Next, I create a rule to permit inbound traffic on port 2222 and port 22, ensuring that only connections intended for Cowrie are accepted. Finally, I verify that my firewall rules have been successfully updated to confirm the changes are in effect.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/PortFoward.png?raw=true" height="100%" width="100%"/> <br />
The next step is to forward any traffic for 22 to 2222. Keeping port 22 open will make this honeypot more desirable to potential threat actors and a more realistic target. This command utilizes iptables to reroute any SSH traffic to Cowrie's default port, 2222.<br />
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
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/createVenv.png?raw=true"/> <br />
I set up a Python virtual environment for Cowrie to isolate it from the system. First, I navigated to the Cowrie directory and created a virtual environment using "python3 -m venv cowrie-env." Then, I activated it with "source cowrie-env/bin/activate," ensuring that installed packages remain contained within this environment. This helps improve security by preventing dependency conflicts and limiting the impact of a potential compromise.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/pip.png?raw=true"/> <br />
I upgraded "pip" within the virtual environment using this command. "install --upgrade" ensures "pip" is installed and updates it to the latest version if an older one exists. This helps prevent compatibility issues and keeps the environment secure with the latest fixes and features.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/pipReq.png?raw=true"/> <br />
I then install the "requirements.txt" file to ensure all necessary packages are up to date and properly configured within the virtual environment.<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/copyCowrieConfigFile.png?raw=true"/> <br />
After updating Cowrie, I need to edit its configuration file. I navigate to the "etc" directory, list the files, and create a copy of "cowrie.cfg.dist," renaming it to "cowrie.cfg." This allows me to customize settings while preserving the default configuration as a backup.<br />
<br />
<br />
Staying in the same directory, I edit the "cowrie.cfg" file using the command nano cowrie.cfg."<br />
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/CowrieHostName.png?raw=true"/> <br />
I edit the cowrie.cfg file and change the hostname to something more realistic, naming it "ubuntu-server-08" to better mimic a real server setup.<br /> 
<br />
<br />
<img src="https://github.com/AndresPineda-CySec/Cowrie-and-Splunk-Honeypot-Threat-Analysis/blob/main/Images/SSH-SSL-UbuntuVers.png?raw=true"/> <br />
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










<h3 align="center">Exporting Cowrie Logs to splunk for monitoring:</h3>
<p align="center">











<h3 align="center">Results:</h3>
<p align="center">
