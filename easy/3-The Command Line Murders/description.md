# "The Command Line Murders"

| Field | Value |
|-------|-------|
| **Level** | Easy |
| **Type** | Do |
| **Tags** | bash |
| **Access** | Public |
| **Root (sudo) Access** | No |

## Description

This is the Command Line Murders with a small twist — the solution (murderer) is different from the original puzzle.

Hints are at the base of the `/home/admin/clmystery` directory. A murder has been committed. Your job is to investigate the crime scene, follow the clues through witness interviews, membership lists, and vehicle registrations, and identify the murderer.

Once you know who did it, write the full name to `/home/admin/mysolution`:

```bash
echo "First Last" > ~/mysolution
```

## General Instructions

You have full access to a real Linux server (ephemeral VM) via SSH. Do whatever is necessary to fix the problem so the test passes within the allotted time.

- No outbound internet access from the server (except when indicated). DNS is available locally.
- Do not interfere with services on ports **:2020** and **:6767**.
- If stuck, click **"Next Clue / Solution"** for hints.
- Click **"Check My Solution"** to verify your fix.

## Acceptance Test

`md5sum ~/mysolution` returns `9bba101c7369f49ca890ea96aa242dd5`.
