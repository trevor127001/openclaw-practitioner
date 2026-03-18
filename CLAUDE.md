# CLAUDE.md — AI Assistant Guide

This file is for AI assistants (Claude Code, Codex, etc.) working in this repository. It explains what this repo is, how it's organized, and what conventions to follow.

---

## What This Repository Is

This is a **documentation and configuration repository** — not a software project. It contains no source code, build artifacts, or executables.

The repo documents a real-world [OpenClaw](https://github.com/openclaw/openclaw) AI agent setup built by a practitioner (not a developer). It captures configuration decisions, operational procedures, agent behavior guidelines, and a running journal of changes as the setup evolves.

The agent persona documented here is named **Sage 🧭** — an AI assistant/mentor running on OpenClaw, primarily accessed via Telegram, with a secondary Discord presence.

---

## Repository Structure

```
openclaw-practitioner/
├── README.md                   # Project overview and philosophy
├── AGENTS.md                   # Agent operating instructions (the agent reads this)
├── SOUL.md                     # Agent persona and tone guidelines
├── IDENTITY.md                 # Name, role, emoji (Sage 🧭)
├── model-routing-policy.md     # Model selection rules and decision log
├── journal/                    # Timestamped session logs
│   └── YYYY-MM-DD HHMM--topic.md
└── setup/                      # Configuration templates (secrets replaced with placeholders)
    ├── openclaw.json.example   # Gateway config template
    └── key-rotation.md         # API key rotation procedures
```

### Key Files

| File | Purpose |
|------|---------|
| `AGENTS.md` | Authoritative operating instructions for Sage — read every session |
| `SOUL.md` | Persona guidance — tone, values, approach |
| `IDENTITY.md` | Identity declaration — name, emoji, vibe |
| `model-routing-policy.md` | Active model routing rules and decision log |
| `setup/openclaw.json.example` | Redacted gateway config template |
| `setup/key-rotation.md` | API key rotation steps, security rules, known traps |
| `journal/` | Chronological session log entries |

---

## Technology Stack

| Component | Details |
|-----------|---------|
| Platform | [OpenClaw](https://github.com/openclaw/openclaw), self-hosted on Windows 11 |
| Primary model | Anthropic Claude Sonnet 4.6 (complex work) |
| Lightweight model | Anthropic Claude Haiku 4.5 (casual/cheap tasks) |
| Fallback/embeddings | OpenAI API |
| Primary channel | Telegram (@trevor_assistant_bot) |
| Secondary channel | Discord (Sage bot, read-only community participation) |
| Remote peer | Scout (🔍) — separate OpenClaw instance on Kali Linux |
| Restart automation | Windows Task Scheduler + PowerShell scripts |
| Version control | Git (this repo) |

---

## Agent Architecture

The setup runs multiple agents within OpenClaw:

| Agent ID | Model | Purpose |
|----------|-------|---------|
| `main` | Sonnet | Primary orchestrator — Sage. Full tool access. Main workspace. |
| `lite` | Haiku | Lightweight tasks and casual chat |
| `jobsearch` | Sonnet | Job applications, recruiter tracking, interview prep |
| `finance` | Sonnet | Budget, payroll, HR, Kugaaruk operations |
| `process` | Sonnet | BPMN, flowcharts, policy documentation |
| `platform` | Haiku | OpenClaw maintenance, version tracking |

**Scout (🔍)** runs as a separate peer agent on a Kali Linux instance — not a sub-agent of Sage, but a peer that coordinates via Discord and cross-context messaging.

Agent-to-agent messaging is enabled. Cross-provider messaging uses `[from {channel}]` markers.

---

## Key Conventions

### Model Routing

Sage chooses the model — the human doesn't manage this. The active policy (see `model-routing-policy.md`):

- **Default:** Haiku at session start
- **Escalate to Sonnet when:** code/scripts, complex analysis, system design, anything with external consequences, mid-session shift from casual to strategic
- **Sub-agents:** Haiku for simple file/data tasks, Sonnet for code writing/debugging

Haiku is ~20x cheaper than Sonnet. The default matters.

### Session Scoping

Two modes:
- **Thinking mode** — exploring, designing. No scoping required.
- **Execution mode** — before building anything, confirm: (1) output, (2) boundary, (3) done condition.

### Sub-Agents

Spawn a sub-agent (rather than inline execution) when a task is well-scoped, self-contained, and failure/iteration would pollute the main session context. Keep inline for 1-2 tool calls or exploratory work.

### Memory

The agent has no persistent memory between sessions. These files provide continuity:
- `memory/YYYY-MM-DD.md` — daily session notes (raw log, with `## Retain` blocks)
- `MEMORY.md` — curated long-term memory for the main session (kept under ~4KB)
- `memory/<project-name>.md` — project-specific detail archived from MEMORY.md

**Retain block format** (appended at end of session to daily notes):
```
## Retain
- W: [World fact — technical gotcha, config discovery]
- B: [Build fact — what was built/done, first-person]
- O(c=0.8): [Opinion with confidence score and entity tag]
- S: [Summary observation]
```

### Self-Review

Weekly (Sundays), the agent reviews session patterns and proposes improvements to `AGENTS.md` or `model-routing-policy.md`. Proposals are logged in `journal/`. Nothing changes without human approval — no silent self-modification.

### Session Startup Ritual

Before anything else, Sage reads:
1. `SOUL.md` — who it is
2. `USER.md` — who it's helping (if present)
3. `memory/YYYY-MM-DD.md` (today + yesterday)
4. `MEMORY.md` (main session only)

---

## Journal Conventions

Journal files live in `journal/` with the naming pattern:

```
YYYY-MM-DD HHMM--topic-slug.md
```

Examples:
- `2026-03-14 2200--setup day 1.md`
- `2026-03-16 2300--specialist agents and scout.md`

Entries document decisions made, what was built, what failed, and what to remember. New entries are appended (not edited) — the log is append-only.

---

## Safety Rules

**Never:**
- Exfiltrate private data
- Run destructive commands without asking
- Paste `openclaw.json` or `auth-profiles.json` — they contain live secrets
- Restart the gateway directly from the agent (it self-interrupts mid-execution)
- Update OpenClaw while the gateway is running (causes partial install corruption)
- Treat a question as a command ("Can you X?" means answer, not act)

**Always ask before:**
- Sending emails, messages, or public posts
- API calls with external side effects
- Anything irreversible

**Use `trash` over `rm`** — recoverable beats gone forever.

---

## Operational Notes

### Gateway Restart
Never call `openclaw gateway restart` directly from the agent. The canonical method: trigger the Windows scheduled task (`Start-ScheduledTask` in PowerShell → `scripts/restart-gateway.ps1`).

### Billing Lockout Trap
If a billing error occurs, OpenClaw writes a backoff timestamp to `auth-profiles.json`. **Adding credit does not clear it** — lockout lasts up to 5 hours. Fix: manually edit `auth-profiles.json` to remove the backoff entry, then restart.

### Scout (Kali peer)
Scout cannot self-repair if his API key is revoked before a replacement is in place. Always update Scout's config and confirm he's responding via @sage_too_bot *before* revoking the old key.

---

## What AI Assistants Should Do in This Repo

1. **Read before editing.** Every file here is intentional. Understand what it contains before proposing changes.
2. **Preserve tone.** These files are written in a specific voice — direct, no filler, genuinely helpful. Match it.
3. **Keep the journal append-only.** Add new entries; don't rewrite history.
4. **Don't add code.** This is a documentation repo. Don't introduce scripts, code files, or build systems unless explicitly asked.
5. **Proposals, not silent changes.** For changes to `AGENTS.md` or `model-routing-policy.md`, log the proposal in `journal/` first. Don't apply without noting it.
6. **Keep MEMORY.md under ~4KB.** Archive detail to project-specific memory files when it grows.
7. **Secrets stay redacted.** `setup/` files use `YOUR_*` placeholders. Never populate with real values.

---

## Current Status

🟡 **Active build** — self-organization layer in progress:

- Model routing policy: LIVE
- Specialist agent architecture: DEPLOYED (5 specialists + Scout peer)
- Memory architecture (Retain convention): STANDARDIZED
- SQLite/semantic memory layer: PENDING
- Weekly self-review: SCHEDULED (Sundays 9am MDT)
