# Troubleshoot: What Is Writing to /var/log/bad.log?

**Goal:** Identify the process that is writing to `/var/log/bad.log`.

## Step 1 — Confirm the file is actively growing

```bash
tail -f /var/log/bad.log
```

You should see new lines appearing continuously. Press `Ctrl+C` to stop watching.

## Step 2 — Find the process with an open handle on the file

```bash
lsof /var/log/bad.log
```

`lsof` (list open files) shows all processes that currently have the file open. Look for:

- `PID` — the process ID you will need to kill
- `COMMAND` — the name of the program writing to the file
- `USER` — the user running it

Example output:

```
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
python3  1234 admin    3w   REG    8,1    12345  678 /var/log/bad.log
```

## Step 3 — Inspect the process (optional)

```bash
ps aux | grep <PID>
```

Replace `<PID>` with the process ID from Step 2. This shows the full command line that launched the process, which helps confirm it is the right target.

You can also inspect the exact script being run:

```bash
cat /proc/<PID>/cmdline | tr '\0' ' '
```
