<h1><b>Scenario</b></h1>
A suspicious file was identified on a company web server. The Development team flagged the anomaly, suspecting potential malicious activity. To address the issue, the network team captured critical network traffic and prepared a PCAP file for review. Your task is to analyze the provided PCAP file to uncover how the file appeared and determine the extent of any unauthorized activity.

<h1><b>SOC report</b></h1>
<h3><b>Time of activity</b></h3> UTC Arrival Time: Nov 30, 2023 18:44:52.446 UTC

<h3><b>List of Affected Entities</b></h3>

Source IP: 117.11.88.124 Source Port: 46658;

Destination IP: 24.49.63.79 Destination port: 80 (HTTP); 

Target resource: URL http://shoporoma.com/reviews/uploads/image.jpg.php;

Application/service: web browsing;

Protocol: HTTP.

<h3><b>Reason for Classifying as True Positive</b></h3>

T1027.009 — Embedded Payloads. The attacker embedded PHP code in a file that mimics an image (for example, image.jpg.php). This made it possible to hide malicious content from security systems that could rely on file extension verification or signature analysis. Obfuscation of the file name with a double extension makes it difficult to automatically identify the threat.

T1190 — Exploitation for Client Execution. The malicious file was uploaded through a vulnerability in a web application. Specifically, due to insufficient verification of file extensions on the server. This made it possible to bypass the control mechanisms and place the executable code in the /reviews/uploads/ directory.


<h3><b>Reason for Escalating the Alert</h3></b>

A malicious file containing a reverse shell PHP payload was successfully uploaded to the server. The incident poses a critical risk due to the following consequences:

Remote system access. The reverse shell establishes a connection to the attacker’s server, granting them interactive command‑line access to the compromised system.

Data compromise. Critical files may be accessed, modified, exfiltrated, or deleted.

Lateral movement. The compromised server can be used as a foothold to attack other systems within the company network.

Malware deployment. The attacker can install additional malicious software (e.g., ransomware, spyware, backdoors) via the command line.


<h3><b>Recommended Remediation Actions</h3></b>
  
1. <i>Immediate Containment:</i>

Isolate the affected server from the network to prevent further lateral movement and data exfiltration.

Block all inbound/outbound traffic to and from the attacker’s IP address (117.11.88.124) at the firewall level.

Disable the compromised user account (if applicable) used for the file upload.

2. <i>Investigation and Forensics:</i>

Conduct a thorough malware scan of the entire server using up‑to‑date antivirus/EDR tools. Pay special attention to the /reviews/uploads/ directory and recently modified files.

Analyze network traffic logs from the past 72 hours for connections to and from IP 24.49.63.79. Look for:

outbound connections to suspicious domains or IPs;

large data transfers;

unusual protocols or ports.

Examine web server logs to identify the exact time and method of file upload, as well as any subsequent access to image.jpg.php.

Collect forensic artifacts: memory dumps, process lists, and the malicious file itself for further analysis.

3. <i>Threat Intelligence and Blacklisting:</i>

Add the attacker’s IP address (117.11.88.124) and the domain (if it’s malicious) to the organization’s threat intelligence databases and firewall blacklists.

Submit the malicious file (image.jpg.php) to threat intelligence platforms (e.g., VirusTotal) to generate and share IOCs (Indicators of Compromise).

4. <i>Also, given that the attacker has read <b>/etc/passwd</b>, the response plan should include:</i>

Changing passwords of all users whose records were accessible (especially administrators).

Audit of access rights to critical files (/etc/passwd, /etc/shadow).

Blocking LFI/RFI on the web server (for example, via .htaccess or PHP settings).

Monitoring attempts to access /etc/passwd and /etc/shadow in the future.


<h3><b>List of Attack Indicators</h3></b>

Indicator: Unauthorized read access to /etc/passwd.

Source: Web server logs (HTTP request with Key: /etc/passwd).

Severity: High.

scanning the site for vulnerabilities, download locations of the reverse shell file


<h3><b>Additional information</h3></b>

location of the malicious server - Tianjin:

<img src="https://drive.google.com/uc?export=view&id=1WYo3gp9xUjJGsyPPPkw4Pexs2cTMUzCD" alt="location of the malicious server" width="1000" />

Stage - enumeration:

<img src="https://drive.google.com/uc?export=view&id=1t0lj3qEgoUOUzDvgg6Ug85Br-juihpd8" alt="enumeration" width="1000" />

The presence of a reverse shell in a malicious file:

<img src="https://drive.google.com/uc?export=view&id=1leatojU0Y8JDAF8tl4kMBkejXjcfsaY1" alt="reverse shell" width="1000" />

Access to /etc/passwd: 

<img src="https://drive.google.com/uc?export=view&id=1ju-aetAwK0rPNMYE95n3wZQOgaPaT3ty" alt="/etc/passwd" width="1000" />

