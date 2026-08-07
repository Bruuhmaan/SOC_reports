# **Ramnit_Lab**
[English](#english) | [Русский](#русский)

---

### English

## **Scenario** 
Our intrusion detection system has alerted us to suspicious behavior on a workstation, pointing to a likely malware intrusion. A memory dump of this system has been taken for analysis. Your task is to analyze this dump, trace the malware’s actions, and report key findings.

## **SOC Report**
Analyst: Kirill / Telegram - @unusual_dreamguy
Date: 2026-08-07
Ticket ID: SOC-2026-0807-001

Time of Activity
UTC Arrival Time: 2024-02-01 19:48:50.0 UTC (start the process)

**Network Connection Detail:**
- Source: 192.168.19.133:49682;
- Destination: 58.64.204.181:5202;
- Protocol: TCPv4;
- State: **CLOSED** (successful connection, then terminated);
- Associated Process: ChromeSetup.exe (PID 4628);
- Type: C2 Communication (Virus.Win32.Nimnul.f).

### Suspicious Process Analysis / Reason for Classifying as True Positive 
| **Process** | **PID** | **Source IP:Port** | **Destination IP:Port** | **State** | **Analysis** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| ChromeSetup.exe | 4628 | 192.168.19.133:49682 | 58.64.204.181:5202 | **CLOSED** | **Successful C2 communication** — the connection was fully established and then properly closed by the malware. |

### **MITRE ATT&CK:**
T1036 (Masquerading), T1204 (User Execution), T1071 (C2 Communication).

### **Recommended Remediation Actions**
1. *Immediate Containment:*
   - Isolate the affected server from the network to prevent further lateral movement and data exfiltration.  
   - Block all inbound/outbound traffic to and from the IP addresses (58.64.204.181, 192.168.19.133) at the firewall level.  
   - Disable the compromised user account (if applicable) used for the file upload.  
2. *Investigation and Forensics:* 
   - Perform a thorough scan of the entire workstation for malware using modern antivirus software and endpoint detection and response tools. Pay special attention to the /Downloads directory and recently modified files.
   - Analyze network traffic logs from the past 72 hours for connections to and from IP 58.64.204.181. Look for:  
     - Outbound connections to suspicious domains or IPs;  
     - Large data transfers;  
     - Unusual protocols or ports.  
   - Examine web server logs and all subsequent access to ChromeSetup.exe, as well as the creation of new files, users, and startup tasks.  
3. *Threat Intelligence and Blacklisting:*  
   - Add the attacker's IP addresses (58.64.204.181) and the domain (if it's malicious) to the organization's threat intelligence databases and firewall blacklists, also add another malicious IP addresses from analysing on VirusTotal.
   
The host was compromised by a malware dropper masquerading as ChromeSetup.exe. The malware successfully connected to its C2 server and may have executed additional payloads. However, the absence of persistence does not reduce the severity — the malware successfully communicated with C2 and may have received additional instructions.
### **Additional Information**

Attacker IP geolocation - Hong Kong, China:

<img src="https://drive.google.com/uc?export=view&id=1f1TtIL2iqtPe58XlvsDOwP07F7WDA3-8" alt="location of the malicious server" width="1000" />

Malicious process started:

<img src="https://drive.google.com/uc?export=view&id=1lyZPP47Pf-LWVtexGLkqxF-phw_6RwNn" alt="Malicious process started" width="1000" />

Sha1sum hash ChromeSetup.exe: 

<img src="https://drive.google.com/uc?export=view&id=1m9rs62A_jBtnwisf0a4NeQFvnKv6B26u" alt="Sha1sum hash" width="1000" />

1ac890f5fa78c857de42a112983357b0892537b73223d7ec1e1f43f8fc6b7496

The connection to 58.64.204.181:

<img src="https://drive.google.com/uc?export=view&id=1WysemoYjcnGebAkVpTnez86COeIjrtWQ" alt="connection to 58.64.204.181" width="1000" />

---

### Русский

## **Сценарий Ramnit_Lab**
Наша система обнаружения вторжений предупредила нас о подозрительной активности на рабочей станции, указывающей на вероятное вторжение вредоносного ПО. Для анализа был снят дамп памяти этой системы. Ваша задача — проанализировать этот дамп, проследить действия вредоносной программы и сообщить о ключевых выводах.

## **Отчет SOC**
Аналитик: Кирилл / Telegram - @unusual_dreamguy
Дата: 2026-08-07
ID заявки: SOC-2026-0807-001

Время активности
Время активности (UTC): 2024-02-01 19:48:50.0 UTC (запуск процесса)

**Детали сетевого подключения:**
- IP-адрес источника: 192.168.19.133:49682;
- IP-адрес назначения: 58.64.204.181:5202;
- Протокол: TCPv4;
- Состояние: **CLOSED** (успешное подключение, затем завершено);
- Связанный процесс: ChromeSetup.exe (PID 4628);
- Тип: C2-взаимодействие (Virus.Win32.Nimnul.f).

### Анализ подозрительного процесса / Причина классификации как истинное срабатывание
| **Процесс** | **PID** | **IP-адрес источника:Порт** | **IP-адрес назначения:Порт** | **Состояние** | **Анализ** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| ChromeSetup.exe | 4628 | 192.168.19.133:49682 | 58.64.204.181:5202 | **ЗАКРЫТО** | **Успешное C2-взаимодействие** — соединение было полностью установлено, а затем корректно закрыто вредоносной программой. |

### **MITRE ATT&CK:**
T1036 (Маскировка), T1204 (Выполнение пользователем), T1071 (C2-взаимодействие).

### **Рекомендованные действия по устранению последствий**
1. *Немедленная изоляция:*
   - Изолируйте затронутый сервер от сети, чтобы предотвратить дальнейшее боковое перемещение и кражу данных.
   - Заблокируйте весь входящий/исходящий трафик к IP-адресам (58.64.204.181, 192.168.19.133) и от них на уровне межсетевого экрана.
   - Отключите скомпрометированную учетную запись пользователя (если применимо), использовавшуюся для загрузки файла.
2. *Расследование и криминалистика:*
   - Выполните тщательную проверку всей рабочей станции на наличие вредоносных программ с использованием современного антивирусного ПО и инструментов обнаружения и реагирования на конечных точках. Уделите особое внимание каталогу /Downloads и недавно измененным файлам.
   - Проанализируйте журналы сетевого трафика за последние 72 часа на предмет подключений к IP-адресу 58.64.204.181 и от него. Ищите:
     - Исходящие подключения к подозрительным доменам или IP-адресам;
     - Большие передачи данных;
     - Необычные протоколы или порты.
   - Проверьте журналы веб-сервера и все последующие обращения к ChromeSetup.exe, а также создание новых файлов, пользователей и задач автозагрузки.
3. *Разведка угроз и занесение в черный список:*
   - Добавьте IP-адреса злоумышленника (58.64.204.181) и домен (если он вредоносный) в базы данных разведки угроз организации и черные списки межсетевого экрана, а также добавьте другие вредоносные IP-адреса, полученные в результате анализа на VirusTotal.

Хост был скомпрометирован дроппером вредоносного ПО, маскирующимся под ChromeSetup.exe. Вредоносная программа успешно подключилась к своему C2-серверу и, возможно, выполнила дополнительные полезные нагрузки. Однако отсутствие закрепления не снижает критичности — вредонос успешно взаимодействовал с C2 и мог получить дополнительные инструкции.

### **Дополнительная информация**

Геолокация IP-адреса злоумышленника — Гонконг, Китай:

<img src="https://drive.google.com/uc?export=view&id=1f1TtIL2iqtPe58XlvsDOwP07F7WDA3-8" alt="местоположение вредоносного сервера" width="1000" />

Запущен вредоносный процесс:

<img src="https://drive.google.com/uc?export=view&id=1lyZPP47Pf-LWVtexGLkqxF-phw_6RwNn" alt="Запущен вредоносный процесс" width="1000" />

Хеш Sha1sum ChromeSetup.exe:

<img src="https://drive.google.com/uc?export=view&id=1m9rs62A_jBtnwisf0a4NeQFvnKv6B26u" alt="Хеш Sha1sum" width="1000" />

1ac890f5fa78c857de42a112983357b0892537b73223d7ec1e1f43f8fc6b7496

**VirusTotal Analysis:** https://www.virustotal.com/gui/file/1ac890f5fa78c857de42a112983357b0892537b73223d7ec1e1f43f8fc6b7496/detection

Подключение к 58.64.204.181:

<img src="https://drive.google.com/uc?export=view&id=1WysemoYjcnGebAkVpTnez86COeIjrtWQ" alt="подключение к 58.64.204.181" width="1000" />
