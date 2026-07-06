# Troubleshooting Notes

## Problem

No Sysmon Event ID 1 appears.

### Cause

Sysmon service is not running.

### Verify

```powershell
Get-Service Sysmon64
```

---

## Problem

No events appear in Wazuh.

### Cause

Wazuh Agent stopped.

### Verify

```powershell
Get-Service WazuhSvc
```

---

## Problem

Threat Hunting returns no results.

### Solution

Try broader searches:

```
rundll32.exe
```

or

```
data.win.eventdata.image:*rundll32.exe*
```

or

```
data.win.eventdata.commandLine:*Control_RunDLL*
```

---

## Problem

Only Sysmon detects the activity.

### Cause

Wazuh Agent may not have forwarded the latest events.

### Solution

Wait a few seconds and refresh Threat Hunting.

---

## Problem

Control Panel does not open.

### Cause

Incorrect command syntax.

### Correct Command

```cmd
rundll32.exe shell32.dll,Control_RunDLL
```

---

## Problem

Multiple Rundll32 events appear.

### Cause

Windows frequently uses Rundll32 for legitimate background tasks.

### Solution

Identify the event generated during the lab by matching:

- Timestamp
- Command Line
- Parent Process
- User

---

## Validation Checklist

✔ Sysmon service running

✔ Wazuh Agent running

✔ Rundll32 executed successfully

✔ Sysmon Event ID 1 generated

✔ Wazuh ingested the event

✔ Command line visible

✔ Parent process identified

✔ Investigation completed successfully
