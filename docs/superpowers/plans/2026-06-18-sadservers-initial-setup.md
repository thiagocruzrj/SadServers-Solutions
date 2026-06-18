# SadServers Solutions Repository — Initial Setup Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the full repository structure with a root README and the first scenario ("Saint John") fully documented across three files.

**Architecture:** Three difficulty-level folders (easy/medium/hard), each containing numbered scenario subfolders. Every scenario folder holds exactly three markdown files: description, troubleshoot guide, and runbook. The root README serves as the index.

**Tech Stack:** Markdown only — no build tools or dependencies.

---

### Task 1: Create root README.md

**Files:**
- Create: `README.md`
- Create: `medium/.gitkeep`
- Create: `hard/.gitkeep`

- [ ] **Step 1: Create placeholder files for medium/ and hard/ folders**

Git does not track empty directories. Create `.gitkeep` files so the folders exist in the repo:

```bash
mkdir -p medium hard
touch medium/.gitkeep hard/.gitkeep
```

- [ ] **Step 2: Create README.md with repo overview and scenario index**

Create `README.md` at the repository root with this exact content:

```markdown
# SadServers Solutions

A collection of solutions for [SadServers](https://sadservers.com) scenarios — a platform for practicing Linux troubleshooting skills.

## What is SadServers?

SadServers is a site where you can practice fixing broken Linux servers. Each scenario presents a realistic problem on a live server, and you have a time limit to diagnose and fix it.

## What is this repo?

This repo documents solutions to each scenario with three files per scenario:

| File | Purpose |
|------|---------|
| `description.md` | Official scenario details and metadata |
| `troubleshoot.md` | Investigation process — how to diagnose the problem |
| `runbook.md` | Fix steps — exact commands to resolve the scenario |

## Scenarios

### Easy

| # | Scenario | Type | Tags |
|---|----------|------|------|
| 1 | [Saint John - What Is Writing to This Log File](easy/1-Saint%20John%20-%20What%20Is%20Writing%20to%20This%20Log%20File/) | Fix | python, bash |

### Medium

_No scenarios yet._

### Hard

_No scenarios yet._
```

- [ ] **Step 3: Verify files exist**

```bash
ls README.md medium/.gitkeep hard/.gitkeep
```

Expected: all three paths echoed back.

- [ ] **Step 4: Commit**

```bash
git add README.md medium/.gitkeep hard/.gitkeep
git commit -m "docs: add root README and scaffold easy/medium/hard folders"
```

---

### Task 2: Create Saint John — description.md

**Files:**
- Create: `easy/1-Saint John - What Is Writing to This Log File/description.md`

- [ ] **Step 1: Create the scenario folder and description.md**

Create `easy/1-Saint John - What Is Writing to This Log File/description.md` with this exact content:

```markdown
# "Saint John": What Is Writing to This Log File?

| Field | Value |
|-------|-------|
| **Level** | Easy |
| **Type** | Fix |
| **Tags** | python, bash |
| **Access** | Public |
| **Root (sudo) Access** | Yes |
| **Time to Solve** | 10 minutes |

## Description

A developer created a testing program that is continuously writing to a log file `/var/log/bad.log` and filling up disk. You can check for example with:

```bash
tail -f /var/log/bad.log
```

This program is no longer needed. Find it and terminate it. **Do not delete the log file.**

## Acceptance Test

The log file size doesn't change within a time interval bigger than the rate of change of the log file.

The "Check My Solution" button runs `/home/admin/agent/check.sh`.
```

- [ ] **Step 2: Verify file exists**

```bash
ls "easy/1-Saint John - What Is Writing to This Log File/description.md"
```

Expected: file path echoed back.

- [ ] **Step 3: Commit**

```bash
git add "easy/1-Saint John - What Is Writing to This Log File/description.md"
git commit -m "docs(easy): add Saint John scenario description"
```

---

### Task 3: Create Saint John — troubleshoot.md

**Files:**
- Create: `easy/1-Saint John - What Is Writing to This Log File/troubleshoot.md`

- [ ] **Step 1: Create troubleshoot.md**

Create `easy/1-Saint John - What Is Writing to This Log File/troubleshoot.md` with this exact content:

```markdown
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
```

- [ ] **Step 2: Verify file exists**

```bash
ls "easy/1-Saint John - What Is Writing to This Log File/troubleshoot.md"
```

Expected: file path echoed back.

- [ ] **Step 3: Commit**

```bash
git add "easy/1-Saint John - What Is Writing to This Log File/troubleshoot.md"
git commit -m "docs(easy): add Saint John troubleshoot guide"
```

---

### Task 4: Create Saint John — runbook.md

**Files:**
- Create: `easy/1-Saint John - What Is Writing to This Log File/runbook.md`

- [ ] **Step 1: Create runbook.md**

Create `easy/1-Saint John - What Is Writing to This Log File/runbook.md` with this exact content:

```markdown
# Runbook: Terminate the Process Writing to /var/log/bad.log

**Prerequisites:** sudo (root) access.

## Steps

### 1. Find the PID of the process writing to the log file

```bash
lsof /var/log/bad.log
```

Note the `PID` value from the output.

### 2. Terminate the process

```bash
sudo kill <PID>
```

Replace `<PID>` with the process ID from Step 1.

If the process does not stop, force-kill it:

```bash
sudo kill -9 <PID>
```

### 3. Verify the log file has stopped growing

```bash
tail -f /var/log/bad.log
```

No new lines should appear. Press `Ctrl+C` to exit.

You can also confirm no process has the file open anymore:

```bash
lsof /var/log/bad.log
```

This should return no output.

## Expected Result

The file `/var/log/bad.log` exists but its size no longer increases. Running `/home/admin/agent/check.sh` returns a passing result.
```

- [ ] **Step 2: Verify file exists**

```bash
ls "easy/1-Saint John - What Is Writing to This Log File/runbook.md"
```

Expected: file path echoed back.

- [ ] **Step 3: Commit**

```bash
git add "easy/1-Saint John - What Is Writing to This Log File/runbook.md"
git commit -m "docs(easy): add Saint John runbook"
```
