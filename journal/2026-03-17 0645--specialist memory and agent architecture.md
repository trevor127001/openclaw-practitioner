# 2026-03-17 06:45 — Specialist Memory and Agent Architecture

## The multi-agent pattern, implemented

Yesterday we built the specialist agent architecture. Today we extended it with proper memory infrastructure. Here's the full picture:

### The pattern
Single agents handling multiple domains degrade as context grows — they start cutting corners, hallucinating, ignoring instructions. The solution is a team of specialists, each with one focused job, coordinated by an orchestrator (Sage/main).

OpenClaw makes this straightforward: each specialist is a registered agent with its own workspace directory and AGENTS.md. The orchestrator delegates via sub-agent spawning.

### The team

| Agent ID | Domain | Model | Status |
|----------|--------|-------|--------|
| main | Orchestrator — coordinates, delegates | Sonnet | Active |
| jobsearch | Applications, recruiter tracking, call prep | Sonnet | Active |
| finance | Kugaaruk financial operations, payroll, budget, HR | Sonnet | Active |
| process | BPMN, flowcharts, policy, records management | Sonnet | Active |
| platform | OpenClaw maintenance, version tracking, config | Haiku | Active |
| lite | Lightweight tasks | Haiku | Active |

### Memory infrastructure

Each specialist workspace now has:
- `memory/YYYY-MM-DD.md` — session notes with Retain blocks
- `MEMORY.md` — curated long-term facts
- `memory/TEMPLATE.md` — Retain convention format

The jobsearch agent has a seeded MEMORY.md with current application status, active opportunities, negotiation posture, and reference notes.

The finance, process, and platform agents will build their memory as they get used. Don't architect for memory you don't have yet — the same principle as the agent architecture itself.

### Specialist agents need their own startup ritual

Each specialist AGENTS.md now includes:
```
## Every Session
1. Read MEMORY.md
2. Read today's and yesterday's memory/YYYY-MM-DD.md
3. Read relevant context files (e.g. APPLICATIONS.md for jobsearch)
At end of session: write Retain block to daily notes.
```

Without this, specialist agents start cold every time — no continuity, no accumulated context. The startup ritual is what makes them genuinely useful over time rather than just focused.

### First exercise: Daniela call prep

The jobsearch agent was spawned to prep for a recruiter call (DRFN SFO, 2:30pm today). It timed out at 2 minutes — the task involved reading workspace files, doing web research, and synthesizing a briefing. Lesson: research-heavy sub-agent tasks need 5+ minute timeouts and sequenced instructions (read context first, then research, then synthesize).

The partial result was still useful — it surfaced DRFN's Finance and Audit Committee structure, confirming their FAL has real teeth. The orchestrator completed the briefing from there.

---

## On research vs. synthesis

An ongoing pattern worth documenting: the default is to synthesize from training data rather than verify with sources. This produces fluent answers that may be wrong.

The correct default: treat "I know this" as a trigger to check, not to answer. The yt-dlp discovery is the model — web_fetch failed, so find another path. The MIFN governance structure question is the counter-example — confident extrapolation without checking FAL standards documentation.

SOUL.md now includes: "Research first, synthesize second. A 10-second search beats a confident wrong answer."
