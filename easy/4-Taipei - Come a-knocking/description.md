# "Taipei": Come a-knocking

| Field | Value |
|-------|-------|
| **Level** | Easy |
| **Type** | Hack |
| **Tags** | hack |
| **Access** | Email |
| **Root (sudo) Access** | No |
| **Time to Solve** | 15 minutes |

## Description

There is a web server on port `:80` protected with Port Knocking. Find the one "knock" needed (sending a SYN to a single port, not a sequence) so you can `curl localhost`.

**Reference:** [Port knocking — Wikipedia](https://en.wikipedia.org/wiki/Port_knocking)

## General Instructions

You have full access to a real Linux server (ephemeral VM) via SSH. Do whatever is necessary to fix the problem so the test passes within the allotted time.

- No outbound internet access from the server (except when indicated). DNS is available locally.
- Do not interfere with services on ports **:2020** and **:6767**.
- If stuck, click **"Next Clue / Solution"** for hints.
- Click **"Check My Solution"** to verify your fix.

## Acceptance Test

`curl localhost` returns a message whose output (including the newline) has md5sum `fe474f8e1c29e9f412ed3b726369ab65`.

Verify with:

```bash
echo $(curl localhost) | md5sum
```
