# Troubleshoot: Find the Port Knock for localhost:80

**Goal:** Identify the port that must receive a SYN packet to unlock port 80.

---

## Step 1 — Confirm port 80 is currently blocked

```bash
curl localhost
```

Expected: `curl: (7) Failed to connect to localhost port 80: Connection refused`. Port 80 is protected by a firewall rule that only opens after the correct knock.

---

## Step 2 — Identify the port knocking daemon

```bash
ps aux | grep knock
```

Output reveals a `knockd` process listening on the loopback interface:

```
root  605  ...  /usr/sbin/knockd -i lo
```

`-i lo` means knockd is watching the **loopback interface** — so the knock must be sent to `localhost`.

---

## Step 3 — Try to read the knockd configuration

```bash
cat /etc/knockd.conf
```

Returns `Permission denied`. The config is root-readable only. Standard fallbacks also yield nothing:

- `/etc/default/knockd` — only contains `KNOCKD_OPTS="-i lo"`, no port info
- `journalctl -u knockd` — startup log does not include the monitored port
- `/proc/<PID>/fd/` — permission denied

---

## Step 4 — Discover the knock ports with nmap

```bash
nmap -Pn --max-retries 0 -p 1-65535 localhost
```

- `-Pn` — skip host discovery; treat the host as up
- `--max-retries 0` — send each probe exactly once and move on (fast scan)
- `-p 1-65535` — scan all TCP ports

Normal closed ports respond with a TCP RST. The key output is ports reported as **`filtered`**:

```
PORT      STATE    SERVICE
22/tcp    open     ssh
6767/tcp  open     bmc-perf-agent
8080/tcp  open     http-proxy
35816/tcp filtered unknown
57524/tcp filtered unknown
```

`filtered` means the firewall is silently dropping packets to those ports — no RST, no response. This is exactly how knockd hides its trigger ports: it intercepts the SYN at the packet capture level before the kernel can reject it, so the port appears neither open nor closed.

Everything that shows as `closed` is irrelevant — knockd is not watching those. Only `filtered` ports are candidates.

---

## Step 5 — Send the knock and curl immediately

```bash
knock localhost 35816 && curl localhost
```

`knock` sends a single TCP SYN to the specified port. `knockd` detects it, runs its configured `iptables` command to allow the source IP to reach port 80, and `curl` connects before the rule can expire.

Both filtered ports (35816 and 57524) are valid knocks in this scenario — the knockd config has rules on both. Either opens port 80.

---

## Key insight

The difference between `closed` and `filtered` in an nmap scan is the tell:

| nmap state | Meaning | knockd relevance |
|------------|---------|-----------------|
| `open` | Port is listening | Not a knock port |
| `closed` | Port reachable, nothing listening (kernel sends RST) | Not a knock port |
| `filtered` | Firewall drops packets silently (no RST) | **Likely a knock port** |
