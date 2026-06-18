# Troubleshoot: What Is Writing to /var/log/bad.log?

**Goal:** Identify the process that is writing to `/var/log/bad.log`.

## Step 1 — Confirm the file is actively growing

```bash
tail -f /var/log/bad.log
```

`tail -f` displays the last lines of a file and keeps watching it in real time (`-f` = follow). New lines print to the terminal as they are written. Press `Ctrl+C` to stop watching.

## Step 2 — Find the process with an open handle on the file

```bash
lsof /var/log/bad.log
```

`lsof` (List Open Files) shows every process that currently has the file open. On Linux, all I/O — including log files — is represented as open file handles. Look for:

- `PID` — the Process ID (unique number the OS assigns to every running process) you will need to kill
- `COMMAND` — the name of the program writing to the file
- `USER` — the user running it
- `FD` — the file descriptor mode; `w` means the process has it open for writing

Example output:

```
COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
python3  1234 admin    3w   REG    8,1    12345  678 /var/log/bad.log
```

## Step 3 — Inspect the process (optional)

```bash
ps aux | grep <PID>
```

`ps aux` lists all running processes on the system (`a` = all users, `u` = user-friendly format showing CPU/memory, `x` = include processes not attached to a terminal). Piped with `grep` to filter by PID, it shows the full command line that launched the process, which helps confirm it is the right target.

You can also inspect the exact script being run:

```bash
cat /proc/<PID>/cmdline | tr '\0' ' '
```

`/proc` is a virtual filesystem Linux exposes for inspecting running processes at runtime. `/proc/<PID>/cmdline` holds the exact command that launched the process, but arguments are separated by null bytes (`\0`) instead of spaces. `tr '\0' ' '` translates those null bytes into spaces so the output is human-readable.
