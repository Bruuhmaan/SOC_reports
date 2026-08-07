# SOC L1 Reports — CyberDefenders Labs

Отчёты по лабораториям на сайте cyberdefenders.org / Laboratory reports on the website cyberdefenders.org

**Analyst:** Kirill
**Goal:** Demonstrate practical SOC L1 skills through real‑world attack scenarios
___
## ✅ Completed laboratory work

| **Name** | **Type** | **Short Description** | **Key Techniques / Tools** |
| :--- | :--- | :--- | :--- |
| **[ WebStrike](./01_Network_Forensics/WebStrike_Lab.md )** | 🟢 **Network Forensics** | PCAP file analysis: PHP shell download via obfuscation (double extension). | Wireshark, CVE-2025-24813, T1027, T1190 |
| **[Tomcat Takeover](./01_Network_Forensics/Tomcat_Takeover_Lab.md)** | 🟢 **Network Forensics** | Compromise of Apache Tomcat through default credentials and downloads.the war file. | Wireshark, CVE-2018-1336, T1505.005, T1059.004 |
| **[Ramnit_Lab](./02_Endpoint_Forensics/Ramnit_Lab_(Windows_log_Analysis).md)** | 🟡 **Endpoint Forensics** | Memory dump analysis: detection of a masked process, C2 communication. | Volatility3, T1036, T1204, T1071 |

## 📄 What each report contains

| **Section** | **Description** |
| :--- | :--- |
| **Scenario** | Description of the incident in a format close to the real SOC |
| **Timeline** | The exact time of the attacker's activity |
| **Affected entities** | IP addresses, ports, resources, protocols |
| **Classification** | Why the event is a True Positive |
| **MITRE ATT&CK** | Tactics and techniques used by the attacker |
| **Escalation** | Why the incident requires an immediate response |
| **IOCs** | Indicators of compromise (hashes, IP, file names) |
| **Recommendations** | Steps for localization, investigation and elimination of consequences |
| **Screenshots** | Visual confirmations from the analysis |


## 🛠 Used tools

| **Tool** | **Purpose** |
| :--- | :--- |
| ** Wireshark** | Network Traffic Analysis (PCAP) |
| **Volatility 3** | Memory congestion (dumps) |
| **CyberChef** | Base64 decoding / Payload analysis |
| **VirusTotal** | IOC enrichment (hashes, IP, domains) |
___
My Profiles:

TryHackMe: https://tryhackme.com/p/Gutsexe

CyberDefenders: https://cyberdefenders.org/p/Guts

Telegram: @unusual_dreamguy


---

## 📎 License / Usage

The materials were created for educational purposes to demonstrate SOC L1 skills.  
When copying, please, provide a link to the original repository.
