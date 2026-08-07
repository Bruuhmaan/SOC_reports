# SOC L1 Reports — CyberDefenders Labs

Laboratory reports on the website cyberdefenders.org

[English](#english) | [Русский](#русский)

---

### English

**Analyst:** Kirill
**Goal:** Demonstrate practical SOC L1 skills through real‑world attack scenarios
___
## ✅ Completed laboratory work

| **Name** | **Type** | **Short Description** | **Key Techniques / Tools** |
| :--- | :--- | :--- | :--- |
| **[ WebStrike](./01_Network_Forensics/WebStrike_Lab.md )** | 🟢 **Network Forensics** | PCAP file analysis: PHP shell download via obfuscation (double extension). | Wireshark, CVE-2025-24813, T1027, T1190 |
| **[Tomcat Takeover](./01_Network_Forensics/Tomcat_Takeover_Lab.md)** | 🟢 **Network Forensics** | Compromise of Apache Tomcat through default credentials and downloads.the war file. | Wireshark, CVE-2018-1336, T1505.005, T1059.004 |
| **[Ramnit_Lab](./02_Endpoint_Forensics/Ramnit_Lab.md)** | 🟡 **Endpoint Forensics** | Memory dump analysis: detection of a masked process, C2 communication. | Volatility3, T1036, T1204, T1071 |

| **Color** | **Type of laboratory** |
| :--- | :--- |
| 🟢 | **Network Forensics** — PCAP analysis, network traffic, Wireshark |
| 🟡 | **Endpoint Forensics** — memory dump analysis, Volatility, processes |
| 🔵 | **Log Analysis** — analysis of Event Logs, Syslog, SIEM logs (in the future) |
| 🟣 | **Threat Hunting** — threat hunting, active search (in the future) |
| 🟠 | **Malware Analysis** — basic analysis of malicious files (in the future) |

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
| **Wireshark** | Network Traffic Analysis (PCAP) |
| **Volatility 3** | Memory congestion (dumps) |
| **CyberChef** | Base64 decoding / Payload analysis |
| **VirusTotal** | IOC enrichment (hashes, IP, domains) |
___

## My Profiles:

TryHackMe: https://tryhackme.com/p/Gutsexe

CyberDefenders: https://cyberdefenders.org/p/Guts

Telegram: @unusual_dreamguy


---

## 📎 License / Usage

The materials were created for educational purposes to demonstrate SOC L1 skills.  
When copying, please, provide a link to the original repository.



---

### Русский

Отчёты по лабораториям на сайте cyberdefenders.org 

**Аналитик:** Kirill
**Цель:** Продемонстрировать практические навыки SOC L1 на основе реальных сценариев атак
___
## ✅ Выполненные лабораторные работы

| **Название** | **Тип** | **Краткое описание** | **Ключевые техники / Инструменты** |
| :--- | :--- | :--- | :--- |
| **[ WebStrike](./01_Network_Forensics/WebStrike_Lab.md )** | 🟢 **Сетевая криминалистика** | Анализ PCAP-файла: загрузка PHP-шелла через обфускацию (двойное расширение). | Wireshark, CVE-2025-24813, T1027, T1190 |
| **[Tomcat Takeover](./01_Network_Forensics/Tomcat_Takeover_Lab.md)** | 🟢 **Сетевая криминалистика** | Компрометация Apache Tomcat через стандартные учетные данные и загрузка .war-файла. | Wireshark, CVE-2018-1336, T1505.005, T1059.004 |
| **[Ramnit_Lab](./02_Endpoint_Forensics/Ramnit_Lab.md)** | 🟡 **Криминалистика конечных точек** | Анализ дампа памяти: обнаружение замаскированного процесса, C2-взаимодействие. | Volatility3, T1036, T1204, T1071 |

| **Цвет** | **Тип лабораторной работы** |
| :--- | :--- |
| 🟢 | **Сетевая криминалистика** — анализ PCAP, сетевой трафик, Wireshark |
| 🟡 | **Криминалистика конечных точек** — анализ дампов памяти, Volatility, процессы |
| 🔵 | **Анализ журналов** — анализ журналов событий, Syslog, журналов SIEM (в будущем) |
| 🟣 | **Охота за угрозами** — активный поиск угроз (в будущем) |
| 🟠 | **Анализ вредоносного ПО** — базовый анализ вредоносных файлов (в будущем) |

## 📄 Что содержит каждый отчет

| **Раздел** | **Описание** |
| :--- | :--- |
| **Сценарий** | Описание инцидента в формате, приближенном к реальному SOC |
| **Хронология** | Точное время активности злоумышленника |
| **Затронутые сущности** | IP-адреса, порты, ресурсы, протоколы |
| **Классификация** | Почему событие является истинным срабатыванием (True Positive) |
| **MITRE ATT&CK** | Тактики и техники, использованные злоумышленником |
| **Эскалация** | Почему инцидент требует немедленного реагирования |
| **Индикаторы компрометации** | Индикаторы компрометации (хеши, IP, имена файлов) |
| **Рекомендации** | Шаги по локализации, расследованию и устранению последствий |
| **Скриншоты** | Визуальные подтверждения из анализа |


## 🛠 Используемые инструменты

| **Инструмент** | **Назначение** |
| :--- | :--- |
| **Wireshark** | Анализ сетевого трафика (PCAP) |
| **Volatility 3** | Анализ дампов памяти |
| **CyberChef** | Декодирование Base64 / Анализ полезной нагрузки |
| **VirusTotal** | Обогащение индикаторов (хеши, IP, домены) |
___

## Мои профили:

TryHackMe: https://tryhackme.com/p/Gutsexe

CyberDefenders: https://cyberdefenders.org/p/Guts

Telegram: @unusual_dreamguy


---

## 📎 Лицензия / Использование

Материалы созданы в образовательных целях для демонстрации навыков SOC L1.  
При копировании, пожалуйста, указывайте ссылку на исходный репозиторий.
