# GitHub Actions Minutes Abuse Report & Refund Request

**Account:** [michelabboud](https://github.com/michelabboud)
**Date:** March 6, 2026
**Issue:** Unintended GitHub Actions workflow execution on forked repositories

---

## Formal Complaint to GitHub Support

> **Subject:** Request for GitHub Actions minutes refund — unintended workflow execution on 96 forked repositories
>
> Dear GitHub Support,
>
> My GitHub username is **michelabboud**. I am writing to formally request a refund of GitHub Actions minutes that were consumed without my knowledge or consent.
>
> ### What Happened
>
> Over the past several weeks, I forked approximately **96 repositories** for personal reference, learning, and bookmarking purposes. I had absolutely no intention of running any CI/CD workflows on these forks. However, GitHub Actions workflows inherited from the upstream repositories — particularly those with `schedule` (cron) and `push` triggers — executed automatically on my forks and consumed my Actions minutes.
>
> **The only way I discovered this problem was when I hit the 3,000-minute limit on my account.** There was no prior warning, no notification about unusual usage, and no prompt during the fork process alerting me that workflows would run and consume my minutes.
>
> ### The Scale of the Problem
>
> - **96 forked repositories** had Actions enabled by default
> - **5 repositories** actively ran workflows, consuming a verified **639+ minutes** (actual total likely higher — API only returns ~1,000 of 2,749 runs for `gh-aw`)
> - **1 repository alone** (`gh-aw`) executed **2,749 workflow runs** via scheduled cron jobs — including "Issue Monster", "Agentic Maintenance", "Bot Detection", "Copilot Agent PR Analysis", and 60+ other automated workflows running multiple times daily
> - **Total estimated storage consumed by forks:** 17.2 GB
>
> ### Why This Is a Platform-Level Issue
>
> 1. **No warning at fork time:** GitHub does not warn users that forking a repo with active workflows will consume their Actions minutes
> 2. **No account-level protection:** There is no global setting to disable Actions on forked repos by default
> 3. **No usage alerts:** I received no notification that my minutes were being consumed by forked repo workflows until I hit the hard limit
> 4. **Cron-triggered workflows are especially dangerous:** Repos like `gh-aw` had 60+ scheduled workflows that ran continuously on my fork without any action on my part
>
> ### What I Have Done
>
> I have programmatically disabled GitHub Actions on all 96 forked repositories via the API. No further minutes will be consumed. However, the minutes already used cannot be recovered on my end.
>
> ### My Request
>
> 1. **Refund all Actions minutes consumed by workflows on forked repositories.** These runs were entirely unintentional and provided zero value to me.
> 2. **Consider implementing safeguards** for other users:
>    - A warning dialog when forking repos with active workflows
>    - An account-level setting to auto-disable Actions on forks
>    - Usage notifications before hitting the hard limit (e.g., at 50%, 75%, 90%)
>
> The detailed breakdown of affected repositories, workflow runs, durations, and storage usage is provided below.
>
> Thank you for your time and consideration.
>
> Best regards,
> Michel Abboud (@michelabboud)

---

## Affected Repositories — Detailed Breakdown

### Summary

| Metric | Value |
|--------|-------|
| Total forked repos | 96 |
| Repos with workflow runs | 5 |
| Total workflow runs | 2,802 |
| Verified minutes consumed (API-visible runs) | 639+ min |
| Estimated true total (incl. non-paginated gh-aw runs) | ~870+ min |
| Total fork storage | 17.2 GB |
| Account minutes limit | 3,000 min |

---

### 1. `michelabboud/gh-aw` — CRITICAL

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-15 |
| Total workflow runs | **2,749** |
| Trigger type | `schedule` (cron — multiple times daily) |
| Verified minutes consumed | **316 min** (from ~1,000 API-visible completed runs) |
| Estimated true total | **~870 min** (extrapolated to all 2,749 runs) |
| Storage | 559 MB |
| Unique workflows | **63** |

**Workflow samples (all scheduled cron):**

| Workflow | Runs (sampled) | Status |
|----------|---------------|--------|
| Issue Monster | 23 | success/failure |
| Agentic Maintenance | 6 | skipped |
| Discussion Task Miner | 3 | startup_failure |
| Contribution Check | 3 | failure |
| Chroma Issue Indexer | 3 | failure |
| PR Triage Agent | 2 | failure |
| Bot Detection | 2 | success |
| Copilot Agent PR Analysis | 1 | failure |
| Daily Semgrep Scan | 1 | failure |
| Daily Security Red Team Agent | 1 | failure |
| + 53 more workflows | ... | mostly failure/startup_failure |

This repository is an extreme case: **63 different scheduled workflows** ran continuously on the fork, many multiple times per day, from Feb 15 to Mar 6 (19 days).

---

### 2. `michelabboud/luanti`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-21 |
| Total workflow runs | **27** |
| Trigger types | `push`, `schedule` |
| Total duration | **278 minutes** |
| Storage | 131 MB |

**Workflow breakdown:**

| Workflow | Runs | Trigger | Notable durations |
|----------|------|---------|-------------------|
| macos | 2 | push | ~75 min each |
| windows | 2 | push | ~17 min each |
| linux | 3 | push | ~11 min each |
| docker_image | 4 | push | ~10 min each |
| security | 4 | push, schedule | varies |
| android | 2 | push | ~8 min each |
| cpp_lint | 4 | push | ~1 min each |
| whitespace_checks | 3 | push | seconds |
| png_file_checks | 2 | push | seconds |
| lua_api_deploy | 1 | push | seconds |

---

### 5. `michelabboud/ironclaw`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-14 |
| Total workflow runs | **6** |
| Trigger type | `push` |
| Total duration | **22 minutes** |
| Storage | 5.3 MB |

**Workflow breakdown:**

| Workflow | Runs | Duration |
|----------|------|----------|
| Run Tests | 3 | 12.5 min, 5.5 min, 4.8 min |
| Release-plz | 3 | skipped (seconds) |

---

### 3. `michelabboud/litellm`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-27 |
| Total workflow runs | **16** |
| Trigger types | `schedule`, `push` |
| Total duration | **15 minutes** |
| Storage | 708 MB |

**Workflow breakdown:**

| Workflow | Runs | Trigger |
|----------|------|---------|
| Create Daily Staging Branch | 9 | schedule (daily cron) |
| Stale Issue Management | 4 | schedule |
| CodeQL | 1 | push |
| Helm unit test | 1 | push |
| Read Version from pyproject.toml | 1 | push |

---

### 4. `michelabboud/mcp-fusion`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-21 |
| Total workflow runs | **4** |
| Trigger type | `push` |
| Total duration | **8 minutes** |
| Storage | 3.3 MB |

**Workflow breakdown:**

| Workflow | Runs | Status |
|----------|------|--------|
| CI | 2 | 1 success, 1 failure |
| GitHub Pages | 2 | both failure |

---

## Full Fork Storage Inventory

Total storage consumed by all 96 forks: **17,186,633 KB (~17.2 GB)**

### Top 20 Forks by Storage

| # | Repository | Storage |
|---|-----------|---------|
| 1 | generative-ai-for-beginners | 6.29 GB |
| 2 | ai-agents-for-beginners | 3.56 GB |
| 3 | mcp-for-beginners | 2.70 GB |
| 4 | litellm | 708 MB |
| 5 | gh-aw | 559 MB |
| 6 | open-webui | 318 MB |
| 7 | edgequake | 268 MB |
| 8 | prometheus | 264 MB |
| 9 | openclaw | 222 MB |
| 10 | OpenHands | 211 MB |
| 11 | claude-cookbooks | 194 MB |
| 12 | redamon | 163 MB |
| 13 | PersonaLive | 147 MB |
| 14 | Fabric | 143 MB |
| 15 | luanti | 128 MB |
| 16 | claude-code-templates | 99 MB |
| 17 | ComfyUI | 77 MB |
| 18 | anthropic-courses | 75 MB |
| 19 | rag-time | 71 MB |
| 20 | claude-pilot | 69 MB |

### Remaining 76 Forks (no workflow runs, but Actions were enabled)

All 91 remaining forked repos had GitHub Actions **enabled by default** even though none triggered workflow runs. These repos were at risk of consuming minutes if any upstream workflow used `schedule` or `push` triggers.

---

## Remediation Actions Taken

| Action | Status | Date |
|--------|--------|------|
| Disabled Actions on all 96 forks | Complete | 2026-03-06 |
| Cancelled active/queued runs | None found (all completed) | 2026-03-06 |
| Generated this report | Complete | 2026-03-06 |

---

## How to Submit This Complaint

1. Go to **https://support.github.com/request**
2. Select **Billing & payments** as the category
3. Copy the complaint section at the top of this document
4. Attach this full report as supporting evidence
5. Reference your account: **michelabboud**

---

*Report generated on 2026-03-06 by automated analysis of the GitHub API.*
