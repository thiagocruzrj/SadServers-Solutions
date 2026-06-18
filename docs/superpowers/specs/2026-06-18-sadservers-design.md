# SadServers Solutions Repository — Design Spec

**Date:** 2026-06-18
**Status:** Approved

---

## Purpose

This repository documents solutions to [SadServers](https://sadservers.com) scenarios. Each scenario gets its own folder containing three files: a description, a troubleshooting guide, and a runbook. The goal is to serve as a reference for learning Linux troubleshooting techniques.

---

## Repository Structure

```
README.md
easy/
  <N>-<Scenario Name - Subtitle>/
    description.md
    troubleshoot.md
    runbook.md
medium/
  <N>-<Scenario Name - Subtitle>/
    description.md
    troubleshoot.md
    runbook.md
hard/
  <N>-<Scenario Name - Subtitle>/
    description.md
    troubleshoot.md
    runbook.md
docs/
  superpowers/
    specs/
```

### Naming conventions

- Difficulty folders: `easy/`, `medium/`, `hard/`
- Scenario folders: `<N>-<Scenario Name - Subtitle>` where `N` is a sequential number within the difficulty level, name matches the official SadServers scenario title, colons replaced with ` - `
- Example: `1-Saint John - What Is Writing to This Log File`

---

## Root README.md

Contains:
- Brief explanation of what SadServers is
- Brief explanation of what this repo is for
- Scenario index table per difficulty level (name, type, tags, link to folder)

---

## Per-Scenario Files

### `description.md`

Captures the official scenario metadata provided for each scenario:

- Scenario name and link
- Level (Easy / Medium / Hard)
- Type (Fix / Break / etc.)
- Tags
- Access (Public / etc.)
- Description paragraph
- Root (sudo) access: Yes/No
- Test / acceptance criteria
- Time to solve

### `troubleshoot.md`

Documents the **investigation process** — how to identify the problem:

- Goal statement (what we're trying to find)
- Step-by-step commands to run, each with:
  - The command itself
  - What it does and what to look for in the output
- How to interpret findings to pinpoint the root cause

### `runbook.md`

Documents the **fix** — exact steps to resolve the scenario:

- Prerequisites (e.g., sudo access required)
- Numbered fix steps with exact commands
- Verification step confirming the problem is resolved

---

## First Scenario

**Scenario:** Saint John - What Is Writing to This Log File
**Difficulty:** Easy
**Folder:** `easy/1-Saint John - What Is Writing to This Log File/`
