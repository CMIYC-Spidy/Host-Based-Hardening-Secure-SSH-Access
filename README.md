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
<h2 align="center"> Phase 1: ... </h2>
<br />

<p align="center">
<br />
To Be Written (TBW) <br/>
<img width="626" height="244" alt="Screenshot 2026-01-07 205430" src="https://github.com/user-attachments/assets/242f24c8-20ad-4afb-8380-31c963c524bc" />

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
<h2 align="center"> Phase 2: ... </h2>
<br />
<p align="center">
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
<h2 align="center"> Phase 3: ... </h2>
<br />
<p align="center">
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />
TBW <br/>

<br />
<br />







<h1>6. Final Reflection</h1>

Building this hardened environment provided me with a hands-on understanding of the "Zero Trust" model. My biggest takeaway was the shift from a standard inbound-only firewall mindset to a strict egress-control policy. By manually whitelisting only essential ports like DNS and APT, I learned how to significantly neutralize the threat of a system being used for data exfiltration or reaching out to malicious external servers.

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
