# Troubleshoot: Find the Most Frequent IP in access.log

**Goal:** Understand the log file format and identify the right tools to count and rank IP addresses.

## Step 1 — Inspect the log file format

```bash
head /home/admin/access.log
```

`head` displays the first 10 lines of a file (default). This confirms the format: each line is one HTTP request with the IP address as the first whitespace-separated column.

Example output:

```
83.149.9.216 - - [17/May/2015:10:05:03 +0000] "GET /presentations/logstash-..."
83.149.9.216 - - [17/May/2015:10:05:43 +0000] "GET /presentations/logstash-..."
83.149.9.216 - - [17/May/2015:10:05:47 +0000] "GET /favicon.ico HTTP/1.1" ...
```

## Step 2 — Extract the first column (IP addresses)

```bash
awk '{print $1}' /home/admin/access.log
```

`awk` is a text-processing tool that operates on each line of input. `{print $1}` prints the first whitespace-separated field of every line — in this case, the IP address. `$2` would be the second field, `$NF` the last, and so on.

## Step 3 — Count how many times each IP appears

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c
```

This pipeline has three stages:

- `sort` — sorts the list of IPs alphabetically. This is required before `uniq` because `uniq` only collapses **consecutive** duplicate lines.
- `uniq -c` — counts consecutive duplicate lines and prefixes each unique line with its count (`-c` = count).

Example output:

```
    482 11.22.33.44
    310 55.66.77.88
     97 99.00.11.22
```

## Step 4 — Find the IP with the highest count

```bash
awk '{print $1}' /home/admin/access.log | sort | uniq -c | sort -rn | head -1
```

Two more stages added:

- `sort -rn` — sorts numerically (`-n`) in reverse order (`-r`), putting the highest count first.
- `head -1` — prints only the first line (the IP with the most requests).

The output will be: `  482 <IP address>`
