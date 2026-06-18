# "Saint John": What Is Writing to This Log File?

| Field | Value |
|-------|-------|
| **Level** | Easy |
| **Type** | Fix |
| **Tags** | python, bash |
| **Access** | Public |
| **Root (sudo) Access** | Yes |
| **Time to Solve** | 10 minutes |

## Description

A developer created a testing program that is continuously writing to a log file `/var/log/bad.log` and filling up disk. You can check for example with:

```bash
tail -f /var/log/bad.log
```

This program is no longer needed. Find it and terminate it. **Do not delete the log file.**

## General Instructions

You have full access to a real Linux server (ephemeral VM) via SSH. Do whatever is necessary to fix the problem so the test passes within the allotted time.

- No outbound internet access from the server (except when indicated). DNS is available locally.
- Do not interfere with services on ports **:2020** and **:6767**.
- If stuck, click **"Next Clue / Solution"** for hints.
- Click **"Check My Solution"** to verify your fix.

## Acceptance Test

The log file size doesn't change within a time interval bigger than the rate of change of the log file.

The "Check My Solution" button runs `/home/admin/agent/check.sh`.
