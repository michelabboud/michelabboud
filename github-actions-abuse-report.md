# GitHub Actions Minutes Abuse Report & Refund Request

**Account:** [michelabboud](https://github.com/michelabboud)
**Plan:** GitHub Pro (3,000 included Actions minutes/month)
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
> ### The Scale of the Problem (Verified from GitHub Billing API)
>
> - **96 forked repositories** had Actions enabled by default
> - **5 repositories** actively ran workflows, consuming **7,213 raw billable minutes**
> - After applying GitHub's runner multipliers (macOS 10x, Windows 2x), this equals **8,014 quota-minutes** — equivalent to **2.7 months** of my entire Pro plan allowance
> - **Forks consumed 58% of my total account usage** (7,213 of 12,422 total minutes)
> - In **February 2026 alone**, forks consumed **5,733 quota-minutes** — nearly **2x my monthly allowance** — and represented **82.5%** of all my Actions usage that month
> - **1 repository alone** (`gh-aw`) consumed **6,534 minutes ($22.59)** via 2,749 scheduled cron runs — 63 different automated workflows running multiple times daily without any action on my part
> - These minutes were billed across Linux, Linux Slim, Linux ARM, macOS 3-core, and Windows runners — all inherited from upstream workflow configurations I never chose
> - **Total storage consumed by forks:** 17.2 GB
>
> ### Why This Is a Platform-Level Issue
>
> 1. **No warning at fork time:** GitHub does not warn users that forking a repo with active workflows will consume their Actions minutes
> 2. **No account-level protection:** There is no global setting to disable Actions on forked repos by default
> 3. **No usage alerts:** I received no notification that my minutes were being consumed by forked repo workflows until I hit the hard limit
> 4. **Cron-triggered workflows are especially dangerous:** Repos like `gh-aw` had 60+ scheduled workflows that ran continuously on my fork without any action on my part
> 5. **Runner multipliers amplify the damage silently:** macOS (10x) and Windows (2x) workflows inherited from upstream repos burn through quotas far faster than a user would expect, with no visibility
>
> ### What I Have Done
>
> I have programmatically disabled GitHub Actions on all 96 forked repositories via the API. No further minutes will be consumed. However, the minutes already used cannot be recovered on my end.
>
> ### My Request
>
> 1. **Refund the 8,014 quota-adjusted Actions minutes (7,213 raw minutes) consumed by workflows on forked repositories.** These runs were entirely unintentional and provided zero value to me. As a Pro plan user with 3,000 included minutes/month, this unintended usage exhausted my monthly quota across multiple billing cycles and blocked my own legitimate projects from running Actions.
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

## Billing Data (from GitHub Billing API)

### Account-Level Impact

| Metric | Total Account | From Forks Only | Fork % |
|--------|--------------|-----------------|--------|
| Raw billable minutes | 12,422 min | 7,213 min | **58.1%** |
| Quota-adjusted minutes | — | 8,014 min | — |
| Gross charges | $65.02 | $29.75 | **45.8%** |

### Runner Multiplier Explanation

GitHub counts minutes differently based on runner type against your monthly quota:

| Runner Type | Raw Minutes (Forks) | Multiplier | Quota-Minutes |
|-------------|-------------------|-----------|---------------|
| Linux | 2,372 min | 1x | 2,372 |
| Linux Slim | 4,455 min | 1x | 4,455 |
| Linux ARM | 33 min | 1x | 33 |
| Windows | 297 min | **2x** | **594** |
| macOS 3-core | 56 min | **10x** | **560** |
| **Total** | **7,213 min** | | **8,014 quota-min** |

The 56 macOS minutes alone consumed **560 quota-minutes** — nearly 19% of a full month's allowance.

### Monthly Breakdown

#### February 2026

| Metric | Total Account | From Forks | Fork % |
|--------|--------------|-----------|--------|
| Raw minutes | 6,458 | 5,332 | **82.5%** |
| Quota-adjusted fork minutes | — | 5,733 | **191% of monthly allowance** |

| Fork Repository | Minutes | Cost |
|----------------|---------|------|
| gh-aw | 4,977 | $17.25 |
| luanti | 339 | $3.45 |
| ironclaw | 11 | $0.07 |
| mcp-fusion | 5 | $0.03 |

#### March 2026 (through Mar 6)

| Metric | Total Account | From Forks | Fork % |
|--------|--------------|-----------|--------|
| Raw minutes | 4,920 | 1,881 | **38.2%** |
| Quota-adjusted fork minutes | — | 2,281 | **76% of monthly allowance** |

| Fork Repository | Minutes | Cost |
|----------------|---------|------|
| gh-aw | 1,557 | $5.34 |
| luanti | 253 | $3.19 |
| ironclaw | 34 | $0.20 |
| litellm | 25 | $0.15 |
| mcp-fusion | 12 | $0.07 |

#### January 2026

| Metric | Total Account | From Forks | Fork % |
|--------|--------------|-----------|--------|
| Raw minutes | 1,045 | 0 | 0% |

No forks had been created yet. This establishes a baseline of normal usage.

### Per-Fork Billing Breakdown (exact data from `/users/michelabboud/settings/billing/usage`)

| Repository | Raw Minutes | Quota-Adjusted | Cost (USD) | Runner Types | Period |
|-----------|------------|----------------|-----------|-------------|--------|
| **gh-aw** | **6,534** | **~6,700** | **$22.59** | Linux, Linux Slim, Windows, macOS 3-core | Feb 20 – Mar 1 |
| **luanti** | **592** | **~1,030** | **$6.64** | Linux, Linux ARM, Windows, macOS 3-core | Feb 21 – Mar 2 |
| **ironclaw** | **45** | **45** | **$0.27** | Linux | Feb 20 – Mar 2 |
| **litellm** | **25** | **25** | **$0.15** | Linux | Mar 2 |
| **mcp-fusion** | **17** | **17** | **$0.10** | Linux | Feb 23 – Mar 2 |
| **Total** | **7,213** | **8,014** | **$29.75** | | |

---

## Affected Repositories — Detailed Breakdown

### 1. `michelabboud/gh-aw` — CRITICAL

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-15 |
| Total workflow runs | **2,749** |
| Trigger type | `schedule` (cron — multiple times daily) |
| Billed minutes (raw) | **6,534** |
| Billed minutes (quota-adjusted) | **~6,700** |
| Billed cost | **$22.59** |
| Runner types | Linux (418 min), Linux Slim (1,122 min), Windows (9 min), macOS 3-core (8 min) + earlier billing periods |
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

This repository is an extreme case: **63 different scheduled workflows** ran continuously on the fork, many multiple times per day, from Feb 15 to Mar 6 (19 days). Many of these workflows failed immediately but still consumed runner minutes.

---

### 2. `michelabboud/luanti`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-21 |
| Total workflow runs | **27** |
| Trigger types | `push`, `schedule` |
| Billed minutes (raw) | **592** |
| Billed minutes (quota-adjusted) | **~1,030** (due to macOS 10x and Windows 2x) |
| Billed cost | **$6.64** |
| Runner types | Linux (86 min), Linux ARM (8 min), macOS 3-core (20 min = 200 quota-min), Windows (139 min = 278 quota-min) |
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

### 3. `michelabboud/ironclaw`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-14 |
| Total workflow runs | **6** |
| Trigger type | `push` |
| Billed minutes | **45** |
| Billed cost | **$0.27** |
| Storage | 5.3 MB |

**Workflow breakdown:**

| Workflow | Runs | Duration |
|----------|------|----------|
| Run Tests | 3 | 12.5 min, 5.5 min, 4.8 min |
| Release-plz | 3 | skipped (seconds) |

---

### 4. `michelabboud/litellm`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-27 |
| Total workflow runs | **16** |
| Trigger types | `schedule`, `push` |
| Billed minutes | **25** |
| Billed cost | **$0.15** |
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

### 5. `michelabboud/mcp-fusion`

| Detail | Value |
|--------|-------|
| Forked on | 2026-02-21 |
| Total workflow runs | **4** |
| Trigger type | `push` |
| Billed minutes | **17** |
| Billed cost | **$0.10** |
| Storage | 3.3 MB |

**Workflow breakdown:**

| Workflow | Runs | Status |
|----------|------|--------|
| CI | 2 | 1 success, 1 failure |
| GitHub Pages | 2 | both failure |

---

## Full Fork Inventory (All 96 Repositories)

Total storage consumed by all 96 forks: **17,186,633 KB (~17.2 GB)**

All repositories listed below had GitHub Actions **enabled by default** upon forking. Actions have now been disabled on all of them.

| # | Repository | Storage | Had Workflow Runs |
|---|-----------|---------|-------------------|
| 1 | generative-ai-for-beginners | 6.29 GB | No |
| 2 | ai-agents-for-beginners | 3.56 GB | No |
| 3 | mcp-for-beginners | 2.70 GB | No |
| 4 | litellm | 708 MB | **Yes — 25 min** |
| 5 | gh-aw | 559 MB | **Yes — 6,534 min** |
| 6 | open-webui | 318 MB | No |
| 7 | edgequake | 268 MB | No |
| 8 | prometheus | 264 MB | No |
| 9 | openclaw | 222 MB | No |
| 10 | OpenHands | 211 MB | No |
| 11 | claude-cookbooks | 194 MB | No |
| 12 | redamon | 163 MB | No |
| 13 | PersonaLive | 147 MB | No |
| 14 | Fabric | 143 MB | No |
| 15 | luanti | 128 MB | **Yes — 592 min** |
| 16 | claude-code-templates | 99 MB | No |
| 17 | ComfyUI | 77 MB | No |
| 18 | anthropic-courses | 75 MB | No |
| 19 | rag-time | 71 MB | No |
| 20 | claude-pilot | 69 MB | No |
| 21 | aws_mcp | 69 MB | No |
| 22 | azure-search-openai-demo | 62 MB | No |
| 23 | obsidian-excalidraw-plugin | 59 MB | No |
| 24 | Z-Image | 56 MB | No |
| 25 | onnx | 45 MB | No |
| 26 | gemini-cli | 39 MB | No |
| 27 | azure-devops-mcp | 39 MB | No |
| 28 | strix-halo-testing | 36 MB | No |
| 29 | cc-switch | 34 MB | No |
| 30 | nanobot | 33 MB | No |
| 31 | azure-ai-travel-agents | 30 MB | No |
| 32 | onefetch | 29 MB | No |
| 33 | termopus | 28 MB | No |
| 34 | LMCache | 28 MB | No |
| 35 | excalidraw-mcp | 27 MB | No |
| 36 | tinyfish-cookbook | 25 MB | No |
| 37 | tree-sitter | 21 MB | No |
| 38 | Kaku | 19 MB | No |
| 39 | picoclaw | 19 MB | No |
| 40 | tinyclaw | 18 MB | No |
| 41 | opencrabs | 17 MB | No |
| 42 | codemoss | 16 MB | No |
| 43 | rustfs | 15 MB | No |
| 44 | web-asset-generator | 15 MB | No |
| 45 | get-started-with-ai-agents | 14 MB | No |
| 46 | microsoft_skills | 12 MB | No |
| 47 | nanoclaw | 10 MB | No |
| 48 | shadowsocks-rust | 9 MB | No |
| 49 | desloppify | 8 MB | No |
| 50 | mdBook | 7 MB | No |
| 51 | dotfiles | 7 MB | No |
| 52 | prompt-eng-interactive-tutorial | 6 MB | No |
| 53 | open-source-mac-os-apps | 6 MB | No |
| 54 | ironclaw | 5 MB | **Yes — 45 min** |
| 55 | velo | 5 MB | No |
| 56 | public-apis | 5 MB | No |
| 57 | anthropic-sdk-python | 5 MB | No |
| 58 | claude-agent-sdk-demos | 5 MB | No |
| 59 | ralph | 5 MB | No |
| 60 | ComfyUI-OpenClaw | 4 MB | No |
| 61 | nvm | 4 MB | No |
| 62 | anthropic-sdk-typescript | 4 MB | No |
| 63 | ralphy | 3 MB | No |
| 64 | mcp-fusion | 3 MB | **Yes — 17 min** |
| 65 | airllm | 3 MB | No |
| 66 | claude-quickstarts | 3 MB | No |
| 67 | skills | 3 MB | No |
| 68 | anthropic-sdk-php | 3 MB | No |
| 69 | israeli-bank-scrapers | 3 MB | No |
| 70 | visual-explainer | 2 MB | No |
| 71 | notebooklm-mcp-cli | 2 MB | No |
| 72 | amd-strix-halo-comfyui-toolboxes | 2 MB | No |
| 73 | ClawRouter | 2 MB | No |
| 74 | cli-continues | 2 MB | No |
| 75 | playwright-mcp | 2 MB | No |
| 76 | the-algorithms-rust | 2 MB | No |
| 77 | zclaw | 1 MB | No |
| 78 | windows-1 | 1 MB | No |
| 79 | Winslop | 1 MB | No |
| 80 | claude-code-action | 1 MB | No |
| 81 | trailofbits-skills | 1 MB | No |
| 82 | atlas-operator | 1 MB | No |
| 83 | amd-strix-halo-toolboxes | 1 MB | No |
| 84 | kubernetes-the-hard-way | <1 MB | No |
| 85 | taches-cc-resources | <1 MB | No |
| 86 | claude-agent-sdk-python | <1 MB | No |
| 87 | agentic-ai-systems | <1 MB | No |
| 88 | agent-viewer | <1 MB | No |
| 89 | contextplus | <1 MB | No |
| 90 | Top-AI-repos | <1 MB | No |
| 91 | JavaScript-AI-Buildathon | <1 MB | No |
| 92 | open-terminal | <1 MB | No |
| 93 | portless | <1 MB | No |
| 94 | bore | <1 MB | No |
| 95 | awesome-openclaw-usecases | <1 MB | No |
| 96 | claude-batch-toolkit | <1 MB | No |
| — | gemini-skills | <1 MB | No |
| — | excalidraw-diagram-skill | <1 MB | No |

---

## Remediation Actions Taken

| Action | Status | Date |
|--------|--------|------|
| Disabled Actions on all 96 forks via API | Complete | 2026-03-06 |
| Cancelled active/queued workflow runs | None found (all completed) | 2026-03-06 |
| Verified no further runs possible | Complete | 2026-03-06 |
| Generated this report with full billing data | Complete | 2026-03-06 |

---

## How to Submit This Complaint

1. Go to **https://support.github.com/request**
2. Select **Billing & payments** as the category
3. Copy the complaint section at the top of this document
4. Attach this full report as supporting evidence
5. Reference your account: **michelabboud**

---

*Report generated on 2026-03-06 using data from the GitHub Billing API (`/users/michelabboud/settings/billing/usage`) and GitHub Actions API.*
