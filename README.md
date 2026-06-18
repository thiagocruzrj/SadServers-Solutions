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

## General Instructions

These apply to every scenario:

- You have full access to a real Linux server (ephemeral VM) via SSH. The VM is terminated after the allotted time.
- No outbound internet access from the server (except when indicated). DNS is available locally.
- Do not interfere with services unrelated to the issue — specifically services running on ports **:2020** and **:6767**.
- If stuck, click **"Next Clue / Solution"** to reveal hints one at a time.
- Once the issue is fixed, click **"Check My Solution"** to verify against the test.

## Scenarios

### Easy

| # | Scenario | Type | Tags |
|---|----------|------|------|
| 1 | [Saint John - What Is Writing to This Log File](easy/1-Saint%20John%20-%20What%20Is%20Writing%20to%20This%20Log%20File/) | Fix | python, bash |

### Medium

_No scenarios yet._

### Hard

_No scenarios yet._
