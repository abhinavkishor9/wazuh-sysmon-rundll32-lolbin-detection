# wazuh-sysmon-rundll32-lolbin-detection
## Objective

This lab demonstrates how to detect and investigate **Rundll32.exe** execution using Sysmon Event ID 1 and Wazuh Threat Hunting.

Rundll32 is a legitimate Windows utility used to execute functions exported by DLL files. Because it is a trusted Microsoft binary, attackers frequently abuse it to execute malicious code while blending into normal system activity.

The objective is to safely generate Rundll32 activity, collect telemetry, and investigate the event from a SOC analyst perspective.

---

## MITRE ATT&CK

Technique:
**T1218.011 – Rundll32**

Tactic:
**Defense Evasion**

---

## Lab Environment

- Windows 11
- Sysmon
- Wazuh Agent
- Wazuh Manager
- PowerShell

---

## Scenario

A user launches **rundll32.exe** to open the Windows Control Panel using the following command:

```cmd
rundll32.exe shell32.dll,Control_RunDLL
```

Sysmon records the process creation event.

Wazuh ingests the event.

The SOC analyst investigates:

- Process Creation
- Parent Process
- Command Line
- Executed DLL
- User Context
- Process Hashes

---

## Commands Used

Verify Sysmon

```powershell
Get-Service Sysmon64
```

Verify Wazuh Agent

```powershell
Get-Service WazuhSvc
```

Execute Rundll32

```cmd
rundll32.exe shell32.dll,Control_RunDLL
```

Verify Sysmon Event

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=1)]]" `
-MaxEvents 50 |
Where-Object {$_.Message -match "rundll32.exe"}
```

Display Complete Event

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" `
-FilterXPath "*[System[(EventID=1)]]" `
-MaxEvents 50 |
Where-Object {$_.Message -match "rundll32.exe"} |
Select-Object -First 1 |
Format-List
```

---

## Wazuh Threat Hunting

Example searches:

```
data.win.eventdata.image:*rundll32.exe*
```

or

```
data.win.eventdata.commandLine:*Control_RunDLL*
```

or

```
rundll32.exe
```

---

## Investigation Workflow

1. Verify Sysmon service
2. Verify Wazuh Agent
3. Execute Rundll32
4. Confirm Event ID 1
5. Hunt the event in Wazuh
6. Review Document Details
7. Analyze JSON fields
8. Record findings

---

## Detection Highlights

Event ID

**1 – Process Creation**

Interesting Fields

- Image
- ParentImage
- CommandLine
- User
- ProcessGuid
- Hashes

Detection Indicators

- rundll32.exe
- shell32.dll
- Control_RunDLL

---

## MITRE Mapping

| Tactic | Technique |
|---------|-----------|
| Defense Evasion | T1218.011 – Rundll32 |

---

## Learning Outcomes

✔ Process Creation Analysis

✔ LOLBin Detection

✔ Parent-Child Process Investigation

✔ Command-Line Analysis

✔ Wazuh Threat Hunting

✔ MITRE ATT&CK Mapping

✔ Windows Native Binary Investigation

---

