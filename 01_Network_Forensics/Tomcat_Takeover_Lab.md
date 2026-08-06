# **Tomcat Takeover Lab**

[English](#english) | [Русский](#русский)

---

### English

## **Scenario**

The SOC team has identified suspicious activity on a web server within the company's intranet. To better understand the situation, they have captured network traffic for analysis. The PCAP file may contain evidence of malicious activities that led to the compromise of the Apache Tomcat web server. Your task is to analyze the PCAP file to understand the scope of the attack.

## **SOC Report**

**Analyst: Kirill / Telegram - @unusual_dreamguy**  
Date: 2026-08-06  
Ticket ID: SOC-2026-0806-001

### **Time of Activity**
UTC Arrival Time: Sep 10, 2023 18:18:52.961026000 UTC

### **List of Affected Entities**
- Source IP: 14.0.0.120 Source Port: 37736 (brute force), 44062 (malicious file upload);
- Destination IP: 10.0.0.112 Destination Port: 80 (HTTP);
- Target resource: 10.0.0.112:8080/manager/html/upload;jsessionid=0DE586F27B2F48D0CA045F731E0E9E71?org.apache.catalina.filters.CSRF_NONCE=83EDF4E2462ECC725BAF342DD7A46974;
- Application/service: web browsing;
- Protocol: HTTP.

### **Reason for Classifying as True Positive | MITRE ATT&CK**
The server was running Tomcat 7.0.88, released in 2017. At the time of the attack (2023), this version had been unsupported for over 6 years and contained numerous known CVEs (CVE-2018-1336, CVE-2018-1304, CVE-2017-15698). However, the primary cause of compromise was the use of default credentials (admin:tomcat) for manager access, which allowed the attacker to upload a malicious .war file without exploiting complex vulnerabilities (the Authorization header: Basic YWRtaW46dG9tY2F0 decodes to admin:tomcat, confirming the use of default credentials for Tomcat Manager login). The presence of a CSRF Nonce (org.apache.catalina.filters.CSRF_NONCE=...) in the request indicates that the attacker successfully authenticated and obtained a valid session token. Nevertheless, the use of outdated software increases the attack surface and represents a critical violation of security policy.
- **T1505.005 — Server Software Component:** .war file uploaded to Tomcat.
- **T1059.004 — Command and Scripting Interpreter:** Execution of bash reverse shell.

### **Reason for Escalating the Alert**
A malicious file (JXQOZY.war) containing a bash reverse shell was successfully uploaded to the server. The incident poses a critical risk due to the following consequences:
- Remote system access. The reverse shell establishes a connection to the attacker's server, granting them interactive command-line access to the compromised system.
- Data compromise. Critical files may be accessed, modified, exfiltrated, or deleted.
- Lateral movement. The compromised server can be used as a foothold to attack other systems within the company network.

### **Recommended Remediation Actions**
1. *Immediate Containment:*  
   - Isolate the affected server from the network to prevent further lateral movement and data exfiltration.  
   - Block all inbound/outbound traffic to and from the attacker's IP address (14.0.0.120) at the firewall level.  
   - Disable the compromised user account (if applicable) used for the file upload.  
   - Update the server version and its components.

2. *Investigation and Forensics:*  
   - Conduct a thorough malware scan of the entire server using up-to-date antivirus/EDR tools. Pay special attention to the /manager/html/upload directory and recently modified files.  
   - Analyze network traffic logs from the past 72 hours for connections to and from IP 10.0.0.112. Look for:  
     - Outbound connections to suspicious domains or IPs;  
     - Large data transfers;  
     - Unusual protocols or ports.  
   - Examine web server logs and all subsequent access to JXQOZY.war, as well as the creation of new files, users, and startup tasks.  
   - Collect forensic artifacts: memory dumps, process lists, and the malicious file itself for further analysis.

3. *Threat Intelligence and Blacklisting:*  
   - Add the attacker's IP address (14.0.0.120) and the domain (if it's malicious) to the organization's threat intelligence databases and firewall blacklists.  
   - Submit the malicious file (JXQOZY.war) to threat intelligence platforms (e.g., VirusTotal) to generate and share IOCs.

### **List of Attack Indicators**
- **Indicator:** Brute force attempts against Tomcat Manager.  
- **Source:** Web server logs (multiple failed login attempts with default credentials).  
- **Severity:** High.  
- **Indicator:** Upload of malicious .war file (JXQOZY.war).  
- **Source:** HTTP POST request to /manager/html/upload.  
- **Severity:** Critical.  
- **Indicator:** Execution of reverse shell via the uploaded malicious file.  
- **Source:** Outbound connection from compromised server to attacker's IP.  
- **Severity:** Critical.

---

### **Additional Information**
Attacker IP geolocation - Guangzhou, Guangdong, China:

<img src="https://drive.google.com/uc?export=view&id=1VCU_7QsdY1QNa7ljOigIyrfyP8cdwTPx" alt="location of the malicious server" width="1000" />

Stage - enumeration:

<img src="https://drive.google.com/uc?export=view&id=1c_OVUZSJhM4SjTIOZEYiMKfgOAXPEUj3" alt="enumeration" width="1000" />

Upload of a malicious file:

<img src="https://drive.google.com/uc?export=view&id=14c_hCp2dd0OrR6AsfPWftjpg1u1CKtyy" alt="malicious file upload" width="1000" />

The presence of a reverse shell in the malicious file:

<img src="https://drive.google.com/uc?export=view&id=1bfuUCDDkC9ahiPhBb22i6tnA3oB9SnLI" alt="reverse shell" width="1000" />

---

### Русский

## **Сценарий Tomcat Takeover Lab**

Команда SOC обнаружила подозрительную активность на веб-сервере во внутренней сети компании. Для лучшего понимания ситуации был захвачен сетевой трафик для анализа. Файл PCAP может содержать доказательства вредоносных действий, которые привели к компрометации веб-сервера Apache Tomcat. Ваша задача — проанализировать предоставленный PCAP-файл, чтобы понять масштаб атаки.

## **Отчёт SOC**

**Аналитик: Кирилл / Telegram - @unusual_dreamguy**  
Дата: 2026-08-06  
ID заявки: SOC-2026-0806-001

### **Время активности**
Время активности (UTC): 10 сентября 2023 г., 18:18:52.961026000 UTC

### **Список затронутых объектов**
- IP-адрес источника: 14.0.0.120, порт источника: 37736 (брут), 44062 (загрузка вредоносного файла);
- IP-адрес назначения: 10.0.0.112, порт назначения: 80 (HTTP);
- Целевой ресурс: 10.0.0.112:8080/manager/html/upload;jsessionid=0DE586F27B2F48D0CA045F731E0E9E71?org.apache.catalina.filters.CSRF_NONCE=83EDF4E2462ECC725BAF342DD7A46974;
- Приложение/сервис: web browsing;
- Протокол: HTTP.

### **Основание для классификации как True Positive | MITRE ATT&CK**
Сервер использовал Tomcat 7.0.88, выпущенный в 2017 году. На момент атаки (2023) эта версия уже более 6 лет не поддерживалась и содержала множество известных CVE (CVE-2018-1336, CVE-2018-1304, CVE-2017-15698). Однако основная причина компрометации — использование стандартных учетных данных (admin:tomcat) для доступа к менеджеру, что позволило атакующему загрузить вредоносный .war файл без необходимости эксплуатации сложных уязвимостей (заголовок Authorization: Basic YWRtaW46dG9tY2F0 декодируется в admin:tomcat, что подтверждает использование стандартных учетных данных для входа в Tomcat Manager). Наличие CSRF Nonce (org.apache.catalina.filters.CSRF_NONCE=...) в запросе указывает на то, что атакующий успешно прошел аутентификацию и получил корректный токен сессии. Тем не менее, сам факт использования устаревшего ПО увеличивает поверхность атаки и является критическим нарушением политики безопасности.
- **T1505.005 — Server Software Component:** .war файл загружен в Tomcat.
- **T1059.004 — Command and Scripting Interpreter:** Выполнение bash reverse shell.

### **Основание для эскалации оповещения**
На сервер был успешно загружен вредоносный файл (JXQOZY.war), содержащий bash reverse shell. Инцидент представляет критический риск из-за следующих последствий:
- Удаленный доступ к системе. Обратная оболочка устанавливает соединение с сервером злоумышленника, предоставляя ему интерактивный доступ к командной строке скомпрометированной системы.
- Компрометация данных. Злоумышленник может получить доступ к критически важным файлам, изменить их, похитить или удалить.
- Горизонтальное перемещение. Компрометированный сервер можно использовать как плацдарм для атаки на другие системы в сети компании.

### **Рекомендуемые меры по устранению последствий**
1. *Немедленная изоляция:*  
   - Изолировать затронутый сервер от сети, чтобы предотвратить дальнейшее горизонтальное перемещение и утечку данных.  
   - Заблокировать весь входящий/исходящий трафик с IP-адресом злоумышленника (14.0.0.120) на уровне межсетевого экрана.  
   - Отключить скомпрометированную учётную запись пользователя (если применимо), использованную для загрузки файла.  
   - Обновить версию сервера и его компонентов.

2. *Расследование и криминалистика:*  
   - Провести тщательное сканирование на вредоносное ПО всего сервера с использованием актуальных антивирусных/EDR-средств. Уделить особое внимание каталогу /manager/html/upload и недавно изменённым файлам.  
   - Проанализировать журналы сетевого трафика за последние 72 часа на предмет соединений с IP 10.0.0.112 и от него. Обратить внимание на:  
     - исходящие соединения с подозрительными доменами или IP-адресами;  
     - большие объёмы передаваемых данных;  
     - необычные протоколы или порты.  
   - Изучить журналы веб-сервера и все последующие обращения к JXQOZY.war, а также создание новых файлов, пользователей и задач автозагрузки.  
   - Собрать криминалистические артефакты: дампы памяти, списки процессов и сам вредоносный файл для дальнейшего анализа.

3. *Разведка угроз и внесение в чёрные списки:*  
   - Добавить IP-адрес злоумышленника (14.0.0.120) и домен (если он вредоносный) в базы данных разведки угроз организации и чёрные списки межсетевого экрана.  
   - Проверить вредоносный файл (JXQOZY.war) на платформах разведки угроз (например, VirusTotal) для генерации и обмена индикаторами компрометации.

### **Список индикаторов атаки**
- **Индикатор:** Попытки брутфорса Tomcat Manager.  
- **Источник:** Журналы веб-сервера (множественные неудачные попытки входа со стандартными учетными данными).  
- **Серьёзность:** Высокая.  
- **Индикатор:** Загрузка вредоносного .war файла (JXQOZY.war).  
- **Источник:** HTTP POST запрос к /manager/html/upload.  
- **Серьёзность:** Критическая.  
- **Индикатор:** Выполнение reverse shell через загруженный вредоносный файл.  
- **Источник:** Исходящее соединение с скомпрометированного сервера на IP-адрес злоумышленника.  
- **Серьёзность:** Критическая.

---

### **Дополнительная информация**
Геолокация IP-адреса злоумышленника — Гуанчжоу, Гуандун, Китай:

<img src="https://drive.google.com/uc?export=view&id=1VCU_7QsdY1QNa7ljOigIyrfyP8cdwTPx" alt="местоположение вредоносного сервера" width="1000" />

Этап — перебор (enumeration):

<img src="https://drive.google.com/uc?export=view&id=1c_OVUZSJhM4SjTIOZEYiMKfgOAXPEUj3" alt="перебор" width="1000" />

Загрузка вредоносного файла:

<img src="https://drive.google.com/uc?export=view&id=14c_hCp2dd0OrR6AsfPWftjpg1u1CKtyy" alt="загрузка вредоносного файла" width="1000" />

Наличие реверс-шелла во вредоносном файле:

<img src="https://drive.google.com/uc?export=view&id=1bfuUCDDkC9ahiPhBb22i6tnA3oB9SnLI" alt="реверс-шелл" width="1000" />
