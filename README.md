<h1 align="center">SSH & System Security Hardening via Firewall</a></h1>

<div style="background-color: #1e1e1e; padding: 15px; border-radius: 5px; border-left: 5px solid #007acc;">
  <strong>Tools Used:</strong>
  <ul>
    <li><a href="https://help.ubuntu.com/community/UFW"> UFW (Uncomplicated Firewall)</li>
    <li><a href="https://man.openbsd.org/sftp-server.8"> OpenSSH-Server</li>
    <li><a href="https://man.openbsd.org/ssh.1"> OpenSSH-Client</li>
    <li><a href="https://nmap.org/"> Nmap</li>
  </ul>
</div>

<h2>1. Project Objective & Principles</h2>

The goal of this phase was to transition the server from a local installation to a network-ready, hardened instance. The configuration follows a "Deny-by-Default" security posture to minimize the risk of unauthorized access or data exfiltration.

<ul>
  <li><b>Security Baseline:</b> Implemented a strict perimeter defense using UFW (Uncomplicated Firewall).</li>
  <li><b>Principle of Least Privilege:</b> Restricted administrative access by hardening the SSH service configuration.</li>
</ul>

<h2>2. SSH Service Hardening</h2>

To secure remote administration, the OpenSSH server was installed and configured with non-default security parameters.

<ul>
  <li><b>Service Installation:</b> Executed the installation of the openssh-server package.</li>
  <li><b>Configuration Change:</b> Disabled direct root login by setting <code>PermitRootLogin=no</code> in <code>/etc/ssh/sshd_config</code>.</li>
  <li><b>Security Outcome:</b> This ensures an audit trail by forcing administrative tasks through a standard user via sudo.</li>
</ul>

<h2>3. Firewall Architecture (UFW)</h2>

I implemented a Strict Egress Control policy. Unlike standard setups that allow all outgoing traffic, this configuration only permits essential system communication.

<ul>
  <li><b>Inbound Policy:</b> DENY ALL (Default)</li>
  <li><b>Outbound Policy:</b> DENY ALL (Default)</li>
</ul>

<h3>Whitelisted Traffic Rules:</h3>
<ul>
  <li><b>Port 22 (SSH):</b> Allowed for remote CLI management.</li>
  <li><b>Port 53 (DNS):</b> Allowed for hostname resolution during system updates.</li>
  <li><b>Ports 80 & 443 (HTTP/S):</b> Allowed exclusively for apt package management and security patching.</li>
</ul>

<h2>4. Technical Validation & Monitoring</h2>

<ul>
  <li><b>Firewall Logging:</b> Enabled UFW logging to monitor and audit dropped packets in real-time via <code>tail -i /var/log/ufw.log</code>.</li>
  <li><b>Network Verification:</b> Utilized scanning tools from the host machine to confirm that all non-essential ports are correctly filtered by the kernel.</li>
</ul>

<h2>5. Program Walk-through</h2>

- In this section, I will walk you through the manual execution of the hardening process. I began by configuring the SSH environment to secure remote access, followed by the systematic application of firewall rules to lock down both inbound and outbound traffic.
<br />
<h2 align="center"> Phase 1: SSH Service Implementation and Initial Setup </h2>
<br />

<p align="center">
<br />
I started the process by updating the Ubuntu package repositories with the <code>sudo apt update -y</code> command. <br/>
<img width="626" height="244" alt="Screenshot 2026-01-07 205430" src="https://github.com/user-attachments/assets/242f24c8-20ad-4afb-8380-31c963c524bc" />
<br />
<br />
I installed the OpenSSH server using the <code>sudo apt install openssh-server -y</code> command. <br/>

<br />
<br />
I checked the status of the SSH server by running <code>sudo systemctl status ssh</code>. Since it was inactive, I activated and enabled it with the <code>sudo systemctl enable ssh</code> command. <br/>

<br />
<br />
After enabling the service, I ran the <code>ip add</code> command to identify my IP address for connectivity testing. <br/>

<br />
<br />
I verified the SSH server was functional by successfully establishing a remote connection. <br/>

<br />
<br />
<h2 align="center"> Phase 2: The Uncomplicated Firewall (UFW) Setup </h2>
<br />
<p align="center">
<br />
I installed the firewall service by running the <code>sudo apt install ufw</code> command. <br/>

<br />
<br />
I configured the firewall with a strict "Deny-by-Default" posture. I blocked all default incoming and outgoing traffic, and specifically denied SSH access to test the enforcement using these commands:
<code>sudo ufw default deny incoming</code>
<code>sudo ufw default deny outgoing</code>
<code>sudo ufw deny ssh</code>
<code>sudo ufw enable</code> <br/>

<br />
<br />
I tested the firewall by attempting to connect to my server. As expected, the connection timed out, confirming the rules were working. <br/>

<br />
<br />
Now I allowed ssh via <code>sudo ufw allow ssh</code>  <br/>

<br />
<br />
I then restored access by allowing SSH traffic via the <code>sudo ufw allow ssh</code> command. I tested again and confirmed I could connect successfully. <br/>

<br />
<br />
<h2 align="center"> Phase 3: Configuring Essential Services </h2>
<br />
<p align="center">
<br />
I attempted to install the "SL" utility, but the process stayed stuck and timed out. This confirmed that my default deny rules were successfully blocking all outgoing connections. <br/>

<br />
<br />
To allow necessary system functions, I whitelisted the specific ports required for repository communication and DNS resolution:
<code>sudo ufw allow 80/tcp</code>
<code>sudo ufw allow 443/tcp</code>
<code>sudo ufw allow 53/tcp</code> <br/>

<br />
<br />
After updating these configurations, I tried to install the tool again with <code>sudo apt install sl</code>, and this time the installation was successful. <br/>

<br />
<br />
I ran the utility to confirm the installation was complete and the environment was stable. <br/>

<br />
<br />
<h2 align="center"> Phase 4: Final Validation and Audit </h2>
<br />
<p align="center">
<br />
To conclude, I used <code>nmap -p [Desired Port(if-any)] [IP]</code> to perform a network scan on the machine, confirming that all firewall settings were correctly applied and only the intended ports were reachable. <br/>
<br />

<h1>6. Final Reflection</h1>

Building this hardened environment provided me with a hands-on understanding of the "Zero Trust" model. My biggest takeaway was the shift from a standard inbound-only firewall mindset to a strict egress-control policy. By manually whitelisting only essential ports like DNS and APT (Mirror Server), I learned how to significantly neutralize the threat of a system being used for data exfiltration or reaching out to malicious external servers.

This project also reinforced my discipline in administrative auditing; by disabling direct root access, I ensured that every action I took left a clear trail via sudo. Overall, this process taught me that true system security is found in the details of restrictive configuration and the continuous validation of active rules.

Thank you for taking the time to review my project and for your consideration of my work.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
