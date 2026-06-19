# Runbook: Unlock Port 80 via Port Knocking

**Prerequisites:** No sudo required. `nmap` and `knock` must be available.

---

## Step 1 — Scan all ports to find knockd's filtered ports

```bash
nmap -Pn --max-retries 0 -p 1-65535 localhost
```

In the output, look for ports listed as **`filtered`** (not `open` or `closed`). These are the knock candidates — knockd intercepts the SYN before the kernel can reject it, making them appear filtered rather than closed.

Expected output (relevant lines):

```
35816/tcp filtered unknown
57524/tcp filtered unknown
```

---

## Step 2 — Send the knock and curl immediately

```bash
knock localhost 35816 && curl localhost
```

`knock` sends a TCP SYN to port 35816. `knockd` detects it and adds an iptables rule allowing the source IP to reach port 80. `curl` runs immediately after via `&&`.

Expected response: `Who is there?`

Both filtered ports work — either one opens port 80:

```bash
knock localhost 57524 && curl localhost
```

---

## Step 3 — Verify the checksum

```bash
echo $(curl localhost) | md5sum
```

Expected: `fe474f8e1c29e9f412ed3b726369ab65`
