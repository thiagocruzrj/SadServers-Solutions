# Runbook: Write the Most Frequent IP to highestip.txt

**Prerequisites:** Read access to `/home/admin/access.log`, write access to `/home/admin/`.

## Steps

### 1. Find the most frequent IP and write it to the output file

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -rn | head -1 | awk '{print $2}' > /home/admin/highestip.txt
```

Breaking down the full pipeline:

| Stage | Command | What it does |
|-------|---------|--------------|
| 1 | `awk '{print $1}'` | Extracts the first field (IP address) from every line |
| 2 | `sort` | Sorts IPs alphabetically so duplicates are consecutive |
| 3 | `uniq -c` | Counts each unique IP; prefixes lines with the count (`-c` = count) |
| 4 | `sort -rn` | Sorts by count, highest first (`-r` = reverse, `-n` = numeric) |
| 5 | `head -1` | Keeps only the top result (the most frequent IP) |
| 6 | `awk '{print $2}'` | Extracts the IP address, discarding the count prefix |
| `>` | Redirect | Writes the output to `/home/admin/highestip.txt` instead of the terminal |

### 2. Verify the file was written correctly

```bash
cat /home/admin/highestip.txt
```

This should print a single IP address.

### 3. Confirm the IP appears 482 times in the log

```bash
grep -c -F -f /home/admin/highestip.txt /home/admin/access.log
```

- `-c` — print only the count of matching lines instead of the lines themselves
- `-F` — treat the pattern as a fixed string (not a regex), avoiding issues with dots in IP addresses being interpreted as regex wildcards
- `-f` — read the search pattern from a file (here, `highestip.txt`)

Expected output: `482`. If you see a lower number, the wrong IP was captured.

## Expected Result

`/home/admin/highestip.txt` exists, contains a single IP address, and `grep -c -F -f /home/admin/highestip.txt /home/admin/access.log` returns `482`.
