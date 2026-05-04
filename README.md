# Hermes Agent vs OpenClaw: Stability & "It Just Works" Deep Dive

> **TL;DR — Rankings (overall "just works" score):**
> 1. **OpenClaw** — Lower normalized defect density, faster issue triage at scale, massive plugin ecosystem, but higher setup complexity.
> 2. **Hermes Agent** — Simpler one-line install, stronger self-improving learning loop, but higher open-issue density per user and heavier bug label ratio in sample.

---

## Quick Links

- **Project Home:** https://jlaiii.github.io/hermes-vs-openclaw/
- **Hermes Agent:** https://hermes-agent.nousresearch.com/ | [GitHub](https://github.com/NousResearch/hermes-agent)
- **OpenClaw:** https://openclaw.ai/ | [GitHub](https://github.com/openclaw/openclaw)

---

## Objective

This project collects and normalizes real-world data to answer one question for people choosing an autonomous AI agent:

**"Which one is more stable and just works, taking community size into account?"**

---

## Dataset

### Raw GitHub Stats (as of 2026-05-04)

| Metric | Hermes Agent | OpenClaw |
|--------|-------------|----------|
| Stars | 132,529 | 368,249 |
| Forks | 20,139 | 75,827 |
| Open Issues | 8,150 | 6,865 |
| Total Historic Issues | 4,683 | 33,837 |
| Closed Historic Issues | 1,742 | 30,410 |
| Issue Close Rate | ~37.2% | ~89.9% |
| Merged PRs (sample of 100 closed) | 85% | 49% |
| Last Commit | 2026-05-04 | 2026-05-04 |
| Latest Release | v2026.4.30 | v2026.5.4-beta.1 |
| Created | 2025-07-22 | 2025-11-24 |

### Normalized Stability Metrics

Stars are a rough proxy for user base. Normalizing by stars gives a fairer picture of defect density and maintenance health.

| Metric | Hermes Agent | OpenClaw | Better |
|--------|-------------|----------|--------|
| Open Issues per 1,000 Stars | 61.5 | 18.6 | OpenClaw |
| Forks per 100 Stars | 15.2 | 20.6 | OpenClaw |
| Issue Close Rate | 37.2% | 89.9% | OpenClaw |
| Bug Label Rate (in 100-issue sample) | 68% | 17%* | OpenClaw |
| PR Merge Rate | 85% | 49% | Hermes |

*OpenClaw bug+regression labels = 17% of the 100-issue sample.

**Important context:** Hermes labels a much broader set of incoming issues as `type/bug` (including support tickets and environment issues), while OpenClaw triages many support topics into `docs`, `enhancement`, or size tags. Always read the methodology.

---

## Stability Rankings

### Score Methodology

We use a weighted scoring model where higher is better (more stable / better maintained).

- **Open Issues per 1k Stars** (weight −50): Fewer open issues relative to user base is better.
- **PR Merge Rate** (weight +2): Higher merge rate signals healthier contributor workflow.
- **Bug Label Rate in Sample** (weight −5): Lower unhandled bug density is better.
- **Forks per 100 Stars** (weight +10): More forks relative to stars signals deeper engagement.

| Project | Score | Verdict |
|---------|-------|---------|
| OpenClaw | −713.20 | **Winner** |
| Hermes Agent | −3,092.84 | Runner-up |

The raw gap is large because Hermes carries significantly more open-issue volume per star. That said, Hermes has a much higher PR merge rate (85% vs 49%), indicating smaller, more focused contributions that land quickly.

---

## Feature Comparison

| Feature | Hermes Agent | OpenClaw |
|---------|-------------|----------|
| **Core runtime** | Python + Node gateway | Node.js gateway + multi-agent isolation |
| **Install ease** | One-line shell installer (`curl ... | bash`) | `npm` / `npx`, daemon setup optional |
| **Platforms** | 17+ messaging platforms, Termux, Vim, Neovim, Tmux | 30+ messaging platforms, Web UI, iOS/Android |
| **Models** | OpenAI, Anthropic, Bedrock, Kimi, OpenRouter, Ollama, vLLM | Claude, GPT-4, Gemini, DeepSeek, OpenRouter, Ollama, vLLM |
| **Memory** | Persistent FTS5 memory, user profiles, auto skill creation | Markdown files, Mem0 plugin, per-agent isolated state |
| **Plugins / Skills** | 40+ built-in tools + autonomous skill loop | 700+ skills, 3,200+ plugins, marketplace install |
| **MCP Support** | 6,000+ external apps via MCP | Supported with per-run cleanup |
| **Voice** | Built-in voice memos, TTS | Realtime transcription, speech plugins |
| **Security** | Sandboxed code execution, no telemetry | OpenSandbox for risky tools, CVE advisories tracked |

### Where Each One Wins

- **Choose Hermes if:** You want a Python-native stack, a built-in self-improving learning loop, and the easiest one-command install on Linux/macOS.
- **Choose OpenClaw if:** You want the biggest plugin ecosystem, multi-agent isolation, a polished web UI, and the highest community scale.

---

## Known Issues & Common Pain Points

### Hermes Agent
- High `type/bug` label density in issues (68% of sampled issues). Many are environment/setup related.
- Open issue count is high relative to stars; close rate is lower (~37%).
- Some users report Python environment and PATH issues during first install.
- No built-in web UI; interaction is primarily CLI/TUI or messaging platforms.

### OpenClaw
- Crossed 50,000 installs in 48 hours early in its lifecycle, leading to predictable scaling pains:
  - Token usage spikes
  - Repeated scanning of large codebases
  - Default credential guardrails were initially weak
- Setup can be more involved (Node.js, daemon registration, Docker Compose).
- CVE-2026-25593 was reported (security advisory). Fixed in subsequent releases.
- Resource footprint is higher (~2 GB RAM for Docker deployment).

---

## Methodology & Caveats

- **GitHub stars are not users**, but they are the best public proxy for community size.
- **Issue labels are not uniform** across projects. Hermes uses `type/bug` broadly; OpenClaw uses `bug` narrowly and `regression` for regressions.
- **PR merge rate** is sampled from the first 100 closed PRs returned by the API; actual lifetime rates may differ.
- **Issue close rate** uses the GitHub Search API `is:issue` counts, which are approximate.
- Data was collected on **2026-05-04** and will drift as both projects are actively developed.

---

## Files

- `README.md` — this file
- `index.html` — GitHub Pages site
- `data.json` — structured dataset for programmatic use

---

## License

MIT — use the data, fork the analysis, improve the weights.

---

*Built by researching public APIs, docs, and community reports. No affiliation with Nous Research or the OpenClaw team.*
