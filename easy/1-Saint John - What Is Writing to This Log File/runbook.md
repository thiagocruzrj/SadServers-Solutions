# Runbook: Terminate the Process Writing to /var/log/bad.log

**Prerequisites:** sudo (root) access.

## Steps

### 1. Find the PID of the process writing to the log file

```bash
lsof /var/log/bad.log
```

`lsof` (List Open Files) shows every process that currently has the file open. Note the `PID` (Process ID — the unique number the OS assigns to every running process) from the output.

### 2. Terminate the process

```bash
sudo kill <PID>
```

`kill` sends a signal to the process. By default it sends **SIGTERM** (signal 15) — a polite termination request that allows the process to catch the signal and shut down gracefully. `sudo` is required because the process may be owned by a different user.

Replace `<PID>` with the process ID from Step 1.

If the process does not stop, force-kill it:

```bash
sudo kill -9 <PID>
```

`-9` sends **SIGKILL** (signal 9) — an uncatchable signal enforced directly by the OS kernel. The process gets no chance to clean up; it is terminated immediately. Use this only when SIGTERM is ignored.

### 3. Verify the log file has stopped growing

```bash
tail -f /var/log/bad.log
```

`tail -f` watches the file in real time (`-f` = follow). No new lines should appear. Press `Ctrl+C` to exit.

You can also confirm no process has the file open anymore:

```bash
lsof /var/log/bad.log
```

This should return no output — meaning no process holds an open file handle to the log anymore.

## Expected Result

The file `/var/log/bad.log` exists but its size no longer increases. Running `/home/admin/agent/check.sh` returns a passing result.
