# **WebStrike Lab**

[English](#english) | [Русский](#русский)

---

### English

## **Scenario**

A suspicious file was identified on a company web server. The Development team flagged the anomaly, suspecting potential malicious activity. To address the issue, the network team captured critical network traffic and prepared a PCAP file for review. Your task is to analyze the provided PCAP file to uncover how the file appeared and determine the extent of any unauthorized activity.

## **SOC report**

**Analyst: Kirill / Telegram - @unusual_dreamguy**  
Date: 2026-02-06 
Ticket ID: SOC-2026-0206-001

### **Time of activity**
UTC Arrival Time: Nov 30, 2023 18:44:52.446 UTC

### **List of Affected Entities**
- Source IP: 117.11.88.124 Source Port: 46658;
- Destination IP: 24.49.63.79 Destination port: 80 (HTTP);
- Target resource: URL http://shoporoma.com/reviews/uploads/image.jpg.php;
- Application/service: web browsing;
- Protocol: HTTP.

### **Reason for Classifying as True Positive**
- **T1027.009 — Embedded Payloads.** The attacker embedded PHP code in a file that mimics an image (for example, image.jpg.php). This made it possible to hide malicious content from security systems that could rely on file extension verification or signature analysis. Obfuscation of the file name with a double extension makes it difficult to automatically identify the threat.
- **T1190 — Exploitation for Client Execution.** The malicious file was uploaded through a vulnerability in a web application. Specifically, due to insufficient verification of file extensions on the server. This made it possible to bypass the control mechanisms and place the executable code in the /reviews/uploads/ directory.

### **Reason for Escalating the Alert**
A malicious file containing a reverse shell PHP payload was successfully uploaded to the server. The incident poses a critical risk due to the following consequences:
- Remote system access. The reverse shell establishes a connection to the attacker’s server, granting them interactive command‑line access to the compromised system.
- Data compromise. Critical files may be accessed, modified, exfiltrated, or deleted.
- Lateral movement. The compromised server can be used as a foothold to attack other systems within the company network.
- Malware deployment. The attacker can install additional malicious software (e.g., ransomware, spyware, backdoors) via the command line.

### **Recommended Remediation Actions**
1. *Immediate Containment:*  
   - Isolate the affected server from the network to prevent further lateral movement and data exfiltration.  
   - Block all inbound/outbound traffic to and from the attacker’s IP address (117.11.88.124) at the firewall level.  
   - Disable the compromised user account (if applicable) used for the file upload.

2. *Investigation and Forensics:*  
   - Conduct a thorough malware scan of the entire server using up‑to‑date antivirus/EDR tools. Pay special attention to the /reviews/uploads/ directory and recently modified files.  
   - Analyze network traffic logs from the past 72 hours for connections to and from IP 24.49.63.79. Look for:  
     - Outbound connections to suspicious domains or IPs;  
     - Large data transfers;  
     - Unusual protocols or ports.  
   - Examine web server logs to identify the exact time and method of file upload, as well as any subsequent access to image.jpg.php.  
   - Collect forensic artifacts: memory dumps, process lists, and the malicious file itself for further analysis.

3. *Threat Intelligence and Blacklisting:*  
   - Add the attacker’s IP address (117.11.88.124) and the domain (if it’s malicious) to the organization’s threat intelligence databases and firewall blacklists.  
   - Submit the malicious file (image.jpg.php) to threat intelligence platforms (e.g., VirusTotal) to generate and share IOCs.

4. *Also, given that the attacker has read **/etc/passwd**, the response plan should include:*  
   - Changing passwords of all users whose records were accessible (especially administrators).  
   - Audit of access rights to critical files (/etc/passwd, /etc/shadow).  
   - Blocking LFI/RFI on the web server (for example, via .htaccess or PHP settings).  
   - Monitoring attempts to access /etc/passwd and /etc/shadow in the future.

### **List of Attack Indicators**
- **Indicator:** Unauthorized read access to /etc/passwd.  
- **Source:** Web server logs (HTTP request with Key: /etc/passwd).  
- **Severity:** High.  
- scanning the site for vulnerabilities, download locations of the reverse shell file.

---

### **Additional information**
Attacker IP geolocation - Tianjin, China:

<img src="https://drive.google.com/uc?export=view&id=1WYo3gp9xUjJGsyPPPkw4Pexs2cTMUzCD" alt="location of the malicious server" width="1000" />

Stage - enumeration:

<img src="https://drive.google.com/uc?export=view&id=1t0lj3qEgoUOUzDvgg6Ug85Br-juihpd8" alt="enumeration" width="1000" />

The presence of a reverse shell in a malicious file:

<img src="https://drive.google.com/uc?export=view&id=1leatojU0Y8JDAF8tl4kMBkejXjcfsaY1" alt="reverse shell" width="1000" />

Access to /etc/passwd:

<img src="https://drive.google.com/uc?export=view&id=1ju-aetAwK0rPNMYE95n3wZQOgaPaT3ty" alt="/etc/passwd" width="1000" />

---

### Русский

## **Сценарий WebStrike Lab**

На веб-сервере компании был обнаружен подозрительный файл. Команда разработчиков зафиксировала аномалию, заподозрив возможную вредоносную активность. Для решения проблемы сетевая команда захватила критический сетевой трафик и подготовила PCAP-файл для анализа. Ваша задача — проанализировать предоставленный PCAP-файл, чтобы выяснить, как появился этот файл, и определить масштаб несанкционированной активности.

## **Отчёт SOC**

**Аналитик: Кирилл / Telegram - @unusual_dreamguy**  
Дата: 2026-02-06  
ID заявки: SOC-2026-0206-001

### **Время активности**
Время активности (UTC): 30 ноября 2023 г., 18:44:52.446 UTC

### **Список затронутых объектов**
- IP-адрес источника: 117.11.88.124, порт источника: 46658;
- IP-адрес назначения: 24.49.63.79, порт назначения: 80 (HTTP);
- Целевой ресурс: URL http://shoporoma.com/reviews/uploads/image.jpg.php;
- Приложение/сервис: web browsing;
- Протокол: HTTP.

### **Основание для классификации как True Positive**
- **T1027.009 — Встраивание полезных нагрузок.** Злоумышленник внедрил PHP-код в файл, имитирующий изображение (например, image.jpg.php). Это позволило скрыть вредоносное содержимое от систем безопасности, которые могли полагаться на проверку расширения файла или сигнатурный анализ. Обфускация имени файла с помощью двойного расширения затрудняет автоматическое выявление угрозы.
- **T1190 — Использование уязвимости для выполнения на стороне клиента.** Вредоносный файл был загружен через уязвимость в веб-приложении, а именно из-за недостаточной проверки расширений файлов на сервере. Это позволило обойти механизмы контроля и разместить исполняемый код в каталоге /reviews/uploads/.

### **Основание для эскалации оповещения**
Вредоносный файл, содержащий PHP-полезную нагрузку для реверс-шелла, был успешно загружен на сервер. Инцидент представляет критический риск в связи со следующими последствиями:
- Удалённый доступ к системе. Реверс-шелл устанавливает соединение с сервером злоумышленника, предоставляя ему интерактивный доступ к командной строке скомпрометированной системы.
- Компрометация данных. Критические файлы могут быть прочитаны, изменены, выгружены или удалены.
- Горизонтальное перемещение. Скомпрометированный сервер может использоваться как плацдарм для атак на другие системы в сети компании.
- Развёртывание вредоносного ПО. Злоумышленник может установить дополнительное вредоносное программное обеспечение (например, вымогатели, шпионские программы, бэкдоры) через командную строку.

### **Рекомендуемые меры по устранению последствий**
1. *Немедленная изоляция:*  
   - Изолировать затронутый сервер от сети, чтобы предотвратить дальнейшее горизонтальное перемещение и утечку данных.  
   - Заблокировать весь входящий/исходящий трафик с IP-адресом злоумышленника (117.11.88.124) на уровне межсетевого экрана.  
   - Отключить скомпрометированную учётную запись пользователя (если применимо), использованную для загрузки файла.

2. *Расследование и криминалистика:*  
   - Провести тщательное сканирование на вредоносное ПО всего сервера с использованием актуальных антивирусных/EDR-средств. Уделить особое внимание каталогу /reviews/uploads/ и недавно изменённым файлам.  
   - Проанализировать журналы сетевого трафика за последние 72 часа на предмет соединений с IP 24.49.63.79 и от него. Обратить внимание на:  
     - исходящие соединения с подозрительными доменами или IP-адресами;  
     - большие объёмы передаваемых данных;  
     - необычные протоколы или порты.  
   - Изучить журналы веб-сервера, чтобы определить точное время и метод загрузки файла, а также последующие обращения к image.jpg.php.  
   - Собрать криминалистические артефакты: дампы памяти, списки процессов и сам вредоносный файл для дальнейшего анализа.

3. *Разведка угроз и внесение в чёрные списки:*  
   - Добавить IP-адрес злоумышленника (117.11.88.124) и домен (если он вредоносный) в базы данных разведки угроз организации и чёрные списки межсетевого экрана.  
   - Проверить вредоносный файл (image.jpg.php) на платформах разведки угроз (например, VirusTotal) для генерации и обмена индикаторами компрометации.

4. *Кроме того, учитывая, что злоумышленник прочитал **/etc/passwd**, план реагирования должен включать:*  
   - Смену паролей всех пользователей, чьи записи были доступны (особенно администраторов).  
   - Аудит прав доступа к критическим файлам (/etc/passwd, /etc/shadow).  
   - Блокировку LFI/RFI на веб-сервере (например, через .htaccess или настройки PHP).  
   - Мониторинг попыток доступа к /etc/passwd и /etc/shadow в будущем.

### **Список индикаторов атаки**
- **Индикатор:** Несанкционированный доступ на чтение к /etc/passwd.  
- **Источник:** Журналы веб-сервера (HTTP-запрос с ключом: /etc/passwd).  
- **Серьёзность:** Высокая.  
- сканирование сайта на уязвимости, места загрузки файла реверс-шелла.

---

### **Дополнительная информация**
Геолокация IP-адреса злоумышленника — Тяньцзинь, Китай:

<img src="https://drive.google.com/uc?export=view&id=1WYo3gp9xUjJGsyPPPkw4Pexs2cTMUzCD" alt="местоположение вредоносного сервера" width="1000" />

Этап — перебор (enumeration):

<img src="https://drive.google.com/uc?export=view&id=1t0lj3qEgoUOUzDvgg6Ug85Br-juihpd8" alt="перебор" width="1000" />

Наличие реверс-шелла во вредоносном файле:

<img src="https://drive.google.com/uc?export=view&id=1leatojU0Y8JDAF8tl4kMBkejXjcfsaY1" alt="реверс-шелл" width="1000" />

Доступ к /etc/passwd:

<img src="https://drive.google.com/uc?export=view&id=1ju-aetAwK0rPNMYE95n3wZQOgaPaT3ty" alt="/etc/passwd" width="1000" />
