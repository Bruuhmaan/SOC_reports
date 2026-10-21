# Incident Report: Alert Storm - Windows Cleanmgr & sdiagnhost
[English](#english) | [Русский](#русский)

---

### English

## 1. Alert Details
*   **Rule ID:** 92213
*   **Severity:** Level 15 (Critical)
*   **Description:** Executable file dropped in folder commonly used by malware
*   **MITRE ATT&CK:** T1105 - Ingress Tool Transfer
*   **Affected Agent:** WIN10-PC (192.168.56.110)

## 2. Incident Overview
A high volume of Level 15 alerts was triggered within a short time window. Sysmon Event ID 11 (FileCreate) captured the creation of multiple `.exe` and `.dll` files in the user's `AppData\Local\Temp` directory.

## 3. Investigation & Analysis
Analysis of the `Image` field revealed that the files were created by legitimate Windows binaries:
*   `C:\Windows\system32\cleanmgr.exe` (Disk Cleanup Utility)
*   `C:\Windows\System32\sdiagnhost.exe` (Diagnostic Service Host)

These OS components extract temporary DLLs/EXEs to the Temp folder during normal execution (e.g., Windows Update cleanup or diagnostic checks). The Wazuh rule 92213 is designed to catch executables dropped in Temp folders, which is a common malware behavior (T1105). However, in this case, it is a **False Positive** caused by OS noise.

## 4. Remediation
To prevent alert fatigue a targeted suppression rule was implemented in `/var/ossec/etc/rules/local_rules.xml`.

*   **Action Taken:** Created Rule ID `100101` (level="0") to ignore events originating from `cleanmgr.exe` and `sdiagnhost.exe` when writing to Temp folders.

```xml
<rule id="100101" level="0">
  <if_sid>92213</if_sid>
  <field name="win.eventdata.image" type="pcre2">(?i)(cleanmgr|sdiagnhost)\.exe</field>
  <description>Ignored: Windows built-in utility creating temp files (False Positive).</description>
</rule>
```

### Rule Verification Challenge:
Verifying a Level 0 suppression rule can be tricky because Wazuh intentionally drops these events from the alerts.json and the Dashboard to reduce noise.
To prove the rule works, logall_json was temporarily enabled, but this did not help and the rules with level 0 were not displayed. Therefore, I believe that the absence of Phase 3 in the wazuh-logtest response to the json request can serve as confirmation of the rule's operation.

before:

<img src="https://drive.google.com/uc?export=view&id=1UAlJjWBimoFIaDLT5zTmrCkZcKat8Y_p" alt="false-positive-critical-alert" width="500" />
<img src="https://drive.google.com/uc?export=view&id=1cynWDyjztgxsk-ZAVjpgwTOEoDWI-G8A " alt="false-positive-critical-alert" width="1000" />

after:

<img src="https://drive.google.com/uc?export=view&id=1CIDmxVSc2rETe-iliOQtObMI9TRCXwFb " alt="false-positive-critical-alert" width="1000" />

---

### Русский
# Отчет об инциденте: Штормовое предупреждение - Windows Cleanmgr и sdiagnhost

## 1. Детали алерта
*   **ID правила:** 92213
*   **Уровень критичности:** Уровень 15 (Критический)
*   **Описание:** Исполняемый файл создан в папке, часто используемой вредоносным ПО
*   **MITRE ATT&CK:** T1105 — Ingress Tool Transfer (Передача инструментов извне)
*   **Затронутый агент:** WIN10-PC (192.168.56.110)

## 2. Обзор инцидента
За короткий промежуток времени сработало большое количество алертов уровня 15. Sysmon Event ID 11 (FileCreate) зафиксировал создание множества файлов `.exe` и `.dll` в каталоге `AppData\Local\Temp` пользователя.

## 3. Расследование и анализ
Анализ поля `Image` показал, что файлы были созданы легитимными бинарными файлами Windows:
*   `C:\Windows\system32\cleanmgr.exe` (средство очистки диска)
*   `C:\Windows\System32\sdiagnhost.exe` (узел службы диагностики)

Эти компоненты ОС извлекают временные DLL/EXE в папку Temp при нормальной работе (например, при очистке в рамках Windows Update или диагностических проверках). Правило Wazuh 92213 предназначено для обнаружения исполняемых файлов, создаваемых в папках Temp, что является типичным поведением вредоносного ПО (T1105). Однако в данном случае это **ложное срабатывание**, вызванное штатным шумом ОС.

## 4. Устранение
Чтобы предотвратить усталость от алертов, в `/var/ossec/etc/rules/local_rules.xml` было добавлено целевое правило подавления.

*   **Предпринятое действие:** Создано правило с ID `100101` (level="0") для игнорирования событий, источником которых являются `cleanmgr.exe` и `sdiagnhost.exe`, при записи в папки Temp.

```xml
<rule id="100101" level="0">
  <if_sid>92213</if_sid>
  <field name="win.eventdata.image" type="pcre2">(?i)(cleanmgr|sdiagnhost)\.exe</field>
  <description>Ignored: Windows built-in utility creating temp files (False Positive).</description>
</rule>
```

### Сложность проверки правила:
Проверка правила подавления уровня 0 может быть непростой, поскольку Wazuh намеренно исключает такие события из alerts.json и Dashboard для снижения шума.
Чтобы доказать работоспособность правила, logall_json был временно включён, но это не помогло, и правила с уровнем 0 не отображались. Поэтому я считаю, что отсутствие Phase 3 в ответе wazuh-logtest на json-запрос может служить подтверждением работы правила.

до:

<img src="https://drive.google.com/uc?export=view&id=1UAlJjWBimoFIaDLT5zTmrCkZcKat8Y_p" alt="false-positive-critical-alert" width="500" />
<img src="https://drive.google.com/uc?export=view&id=1cynWDyjztgxsk-ZAVjpgwTOEoDWI-G8A " alt="false-positive-critical-alert" width="1000" />

после:

<img src="https://drive.google.com/uc?export=view&id=1CIDmxVSc2rETe-iliOQtObMI9TRCXwFb " alt="false-positive-critical-alert" width="1000" />

