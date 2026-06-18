# "Saskatoon": Counting IPs

| Field | Value |
|-------|-------|
| **Level** | Easy |
| **Type** | Do |
| **Tags** | bash |
| **Access** | Email |
| **Root (sudo) Access** | Yes |

## Description

There's a web server access log file at `/home/admin/access.log`. The file consists of one line per HTTP request, with the requester's IP address at the beginning of each line (first column).

Find the IP address that has the most requests in this file (there is no tie; the IP is unique). Write the solution into a file `/home/admin/highestip.txt`.

For example, if your solution is `1.2.3.4`:

```bash
echo "1.2.3.4" > /home/admin/highestip.txt
```

**Note:** The solution IP appears 482 times. You can verify with:

```bash
grep -c -F -f /home/admin/highestip.txt /home/admin/access.log
```

If the result is lower than 482, you have the wrong IP.

## General Instructions

You have full access to a real Linux server (ephemeral VM) via SSH. Do whatever is necessary to fix the problem so the test passes within the allotted time.

- No outbound internet access from the server (except when indicated). DNS is available locally.
- Do not interfere with services on ports **:2020** and **:6767**.
- If stuck, click **"Next Clue / Solution"** for hints.
- Click **"Check My Solution"** to verify your fix.

## Acceptance Test

The file `/home/admin/highestip.txt` exists and contains the IP address that appears most frequently in `/home/admin/access.log` (482 times).
