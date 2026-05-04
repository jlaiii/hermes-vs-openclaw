# Hermes Agent vs OpenClaw: Stability & "It Just Works" Deep Dive

> **TL;DR — Rankings (overall "just works" score):**
> 1. **OpenClaw** — Lower normalized defect density, dramatically higher bug/issue close rates, massive plugin ecosystem. Heavy bot-driven triage inflates some metrics.
> 2. **Hermes Agent** — Simpler install, zero stale issues, strong learning loop, but lower bug close rate and higher open-issue density per user.

---

## Quick Links

- **Live Website:** https://jlaiii.github.io/hermes-vs-openclaw/
- **Hermes Agent:** https://hermes-agent.nousresearch.com/ | [GitHub](https://github.com/NousResearch/hermes-agent)
- **OpenClaw:** https://openclaw.ai/ | [GitHub](https://github.com/openclaw/openclaw)

---

## Objective

This project collects and normalizes real-world data to answer one question for people choosing an autonomous AI agent:

**"Which one is more stable and just works, taking community size into account?"**

All data is pulled directly from the GitHub API and verified. Where the API returns misleading numbers (e.g. `open_issues_count` includes PRs), we use the Search API to get the real counts.

---

## Verified GitHub Stats (as of 2026-05-04)

| Metric | Hermes Agent | OpenClaw |
|--------|-------------|----------|
| Stars | 132,529 | 368,249 |
| Forks | 20,139 | 75,827 |
| Open Issues (actual) | **2,944** | **3,427** |
| Open PRs | 5,218 | 3,442 |
| Total Issues (all time) | 4,686 | 33,848 |
| Closed Issues | 1,742 | 30,421 |
| Issue Close Rate | 37.2% | **89.9%** |
| Bug Issues (all time) | 2,232 | 10,399 |
| Closed Bug Issues | 746 | 9,899 |
| Bug Close Rate | 33.4% | **95.2%** |
| Contributors | 375 | 366 |
| Commits (last 30 days) | 3,969 | **14,565** |
| New Issues (last 7 days) | 717 | 2,018 |
| Closed Issues (last 7 days) | 289 | **2,170** |
| Median Issue Close Time | **9.2 days** | 0.1 days* |
| Stale Open Issues (>90 days) | **0%** | 49% |
| Oldest Open Issue | 67 days | 123 days |
| Latest Release | v2026.4.30 | v2026.5.4-beta.1 |
| Last Commit | 2026-05-04 | 2026-05-04 |
| Created | 2025-07-22 | 2025-11-24 |

*OpenClaw's 0.1 day median is heavily driven by `clawsweeper[bot]` auto-closing issues. Human-triaged issues may differ.

### Key Discovery: `open_issues_count` Lies

GitHub's `open_issues_count` field includes **open pull requests**. For Hermes, the API reported 8,161 "open issues" but only **2,944** were actual issues — the other 5,218 were open PRs. For OpenClaw, 6,865 API count = 3,427 issues + 3,442 PRs. We use the Search API (`is:issue`) for accurate counts.

---

## Normalized Stability Metrics

Normalizing by stars gives a fairer picture of defect density and maintenance health.

| Metric | Hermes Agent | OpenClaw | Better |
|--------|-------------|----------|--------|
| Open Issues per 1,000 Stars | 22.2 | **9.3** | OpenClaw |
| Open PRs per 1,000 Stars | **39.4** | 9.3 | Hermes |
| Issue Close Rate | 37.2% | **89.9%** | OpenClaw |
| Bug Close Rate | 33.4% | **95.2%** | OpenClaw |
| Commits per 1k Stars (30d) | 29.9 | **39.6** | OpenClaw |
| Stale Open Issues | **0%** | 49% | Hermes |
| Median Close Time | 9.2 days | 0.1 days* | Mixed |

*OpenClaw's close time is bot-inflated.

---

## Stability Rankings

### Score Methodology

Weighted scoring model (higher = more stable / better maintained):

- **Open Issues per 1k Stars** (weight −40): Fewer open issues relative to user base is better.
- **Issue Close Rate** (weight +30): Higher close rate signals responsive maintenance.
- **Bug Close Rate** (weight +25): Higher bug resolution rate signals quality focus.
- **Median Close Time Bonus**: <1 day +20, <7 days +10, <30 days +5
- **Stale Penalty** (−2 per % stale): Old unclosed issues drag stability down.
- **7-Day Throughput Bonus** (+15): Closing more than you open is healthy.
- **Commit Activity** (+5 per 1k stars): Active development is a stability signal.

| Project | Score | Verdict |
|---------|-------|---------|
| **OpenClaw** | **4,838.6** | 🏆 Winner |
| Hermes Agent | 1,217.0 | Runner-up |

The gap is driven primarily by OpenClaw's dramatically higher issue/bug close rates and lower normalized open issue density. Hermes has zero stale issues, which is a strong signal of active curation, but its bug close rate (33.4%) is notably low — many `type/bug` issues may be support requests rather than verified defects.

---

## Feature Comparison

| Feature | Hermes Agent | OpenClaw |
|---------|-------------|----------|
| **Core runtime** | Python + Node gateway | Node.js gateway + multi-agent isolation |
| **Install ease** | One-line shell installer (`curl ... | bash`) | `npm` / `npx`, daemon setup optional |
| **Platforms** | 17+ messaging, Termux, Vim, Neovim, Tmux | 30+ messaging, Web UI, iOS/Android |
| **Models** | OpenAI, Anthropic, Bedrock, Kimi, OpenRouter, Ollama, vLLM | Claude, GPT-4, Gemini, DeepSeek, OpenRouter, Ollama, vLLM |
| **Memory** | Persistent FTS5, user profiles, auto skill creation | Markdown files, Mem0 plugin, per-agent isolated state |
| **Plugins / Skills** | 40+ built-in tools + autonomous skill loop | 700+ skills, 3,200+ plugins, marketplace |
| **MCP Support** | 6,000+ external apps via MCP | Supported with per-run cleanup |
| **Voice** | Built-in voice memos, TTS | Realtime transcription, speech plugins |
| **Security** | Sandboxed code execution, no telemetry | OpenSandbox for risky tools; CVE advisories tracked |

### Where Each Wins

- **Choose Hermes if:** You want the simplest one-command install, a Python-native stack, a built-in self-improving learning loop, and zero stale issues.
- **Choose OpenClaw if:** You want the biggest plugin ecosystem (700+ skills), multi-agent isolation, a polished web UI, and the highest throughput maintenance engine — but accept bot-heavy triage.

---

## Verified Pain Points (from Community Reports)

### Hermes Agent
1. **Install/Setup Failures** — PATH issues, Python 3.12 conflicts, Windows unsupported (WSL2 only)
2. **Gateway Disconnects** — WSL2 disconnects, port conflicts, Telegram/Discord bot unresponsiveness
3. **Memory Leaks** — Residual gateway memory under heavy cron sessions, context loss
4. **Local Model Failures** — Ollama GPU detection, GGUF errors, malformed JSON in multi-step chains
5. **Tool-Call Loops** — Agent hangs, sub-agent context loss, MCP load problems
6. **Patch Parser Bugs (Critical)** — Files >2000 lines silently truncated on UPDATE (GitHub #6831)
7. **Docker Permission Errors** — `chown` failures in v2026.4.23 image
8. **Security** — 9 CVEs disclosed March 2026; memory and replay capabilities were primary attack surface
9. **Bug Close Rate** — Only 33.4% of labeled bugs are closed; 20 open P0 issues, 36 open P1 issues

### OpenClaw
1. **Gateway Crash Loops** — Bonjour plugin, Mac mini M4 launchd respawn cycles, 2026.4.26 mass crashes
2. **Plugin Regressions** — Discord/Telegram channel crashes after version bumps (2026.4.29 → 2026.5.2)
3. **Installation Failures** — Node v22 required, npm install failures, node-gyp/Sharp issues
4. **Performance** — `openclaw memory index --force` pegs CPU cores; memory leak risk into group chats
5. **Security (Severe)** — CVE-2026-25253 token exfiltration, CVE-2026-33579 privilege escalation, 135,000+ instances exposed to RCE
6. **Update Regressions** — Minor bumps repeatedly break integrations (Discord plugin, JSON5 dependency)
7. **Bot-Driven Triage** — `clawsweeper[bot]` auto-closes many issues within hours; may mask real problems
8. **Stale Issues** — 49% of open issues are >90 days old despite high close rate

---

## Critical Caveats

1. **OpenClaw's close rate is inflated by bots.** `clawsweeper[bot]` closes many issues within hours. Human-triaged resolution times may be closer to Hermes' 9.2 days.
2. **Hermes labels support tickets as bugs.** The 68% `type/bug` rate in our sample includes environment issues, setup problems, and unverified reports. The actual verified defect rate is likely lower.
3. **Stars are not users.** They're the best public proxy for community size, but both projects likely have many casual stargazers who never installed.
4. **OpenClaw is younger but larger.** OpenClaw launched November 2025 vs Hermes July 2025, yet has 2.8x the stars and processes 3.7x more commits per month.
5. **Both are actively maintained.** Both had commits on 2026-05-04. Both release frequently (Hermes ~weekly, OpenClaw almost daily).

---

## Files

- `README.md` — this document
- `index.html` — GitHub Pages website
- `data.json` — structured, machine-readable dataset

---

## License

MIT — use the data, fork the analysis, improve the weights.

---

*Built by researching public APIs, docs, and community reports. No affiliation with Nous Research or the OpenClaw team. Data verified via GitHub API and cross-checked with multiple sources.*
