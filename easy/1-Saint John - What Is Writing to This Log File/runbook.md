# Runbook: Terminate the Process Writing to /var/log/bad.log

**Prerequisites:** sudo (root) access.

## Steps

### 1. Find the PID of the process writing to the log file

```bash
lsof /var/log/bad.log
```

Note the `PID` value from the output.

### 2. Terminate the process

```bash
sudo kill <PID>
```

Replace `<PID>` with the process ID from Step 1.

If the process does not stop, force-kill it:

```bash
sudo kill -9 <PID>
```

### 3. Verify the log file has stopped growing

```bash
tail -f /var/log/bad.log
```

No new lines should appear. Press `Ctrl+C` to exit.

You can also confirm no process has the file open anymore:

```bash
lsof /var/log/bad.log
```

This should return no output.

## Expected Result

The file `/var/log/bad.log` exists but its size no longer increases. Running `/home/admin/agent/check.sh` returns a passing result.
