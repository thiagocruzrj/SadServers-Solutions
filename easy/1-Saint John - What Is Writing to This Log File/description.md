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

## Acceptance Test

The log file size doesn't change within a time interval bigger than the rate of change of the log file.

The "Check My Solution" button runs `/home/admin/agent/check.sh`.
