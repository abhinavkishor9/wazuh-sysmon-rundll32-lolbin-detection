# Investigation Notes

## Alert Summary

A Process Creation event was generated after execution of **rundll32.exe**.

The activity was successfully captured by Sysmon and forwarded to Wazuh.

---

## Event Details

Event ID

1

Provider

Microsoft-Windows-Sysmon

---

## Process Information

Process

rundll32.exe

Location

C:\Windows\System32\rundll32.exe

Command Line

```
rundll32.exe shell32.dll,Control_RunDLL
```

Executed DLL

shell32.dll

Exported Function

Control_RunDLL

---

## Parent Process

Record the following from Sysmon:

- Parent Image
- Parent Process ID
- Process GUID

---

## User Context

Record:

- Username
- Integrity Level
- Current Directory

---

## File Hashes

Collect:

- SHA1
- SHA256
- MD5

These hashes help validate the executable and support threat intelligence lookups.

---

## Why Attackers Abuse Rundll32

Attackers commonly use Rundll32 because:

- It is digitally signed by Microsoft.
- It is trusted by Windows.
- It bypasses simple application allowlisting.
- It executes DLL exports.
- It blends into legitimate Windows activity.

---

## Detection Opportunities

SOC analysts should investigate when:

- Rundll32 launches from unusual directories.
- Unknown DLLs are loaded.
- Rundll32 is launched by Office applications.
- Rundll32 executes from temporary folders.
- Suspicious network activity follows Rundll32 execution.

---

## MITRE ATT&CK

Technique

T1218.011

Rundll32

Tactic

Defense Evasion

---

## Severity

Medium

Severity increases if:

- Unsigned DLLs are loaded.
- Rundll32 launches PowerShell or CMD.
- External network connections occur.
- Persistence is established.

---

## Analyst Conclusion

The activity generated in this lab was legitimate and expected.

The investigation demonstrated how Sysmon Event ID 1 and Wazuh Threat Hunting can identify and analyze Rundll32 execution.

Understanding normal Rundll32 behavior helps SOC analysts recognize malicious LOLBin abuse during incident investigations.
