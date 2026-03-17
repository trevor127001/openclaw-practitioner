# 2026-03-16 23:00 — Specialist Agents and the Scout Conversation

## The multi-agent architecture

The video "OpenClaw Doesn't Work Until You Do This" (Openclaw Labs) made the case clearly: single agents handling multiple domains get worse as context grows. The solution is a team of specialists, each with one job, coordinated by an orchestrator.

We built that today. Five specialist agents now registered:

| Agent | Domain | Model |
|-------|--------|-------|
| jobsearch | Applications, recruiter tracking, call prep | Sonnet |
| finance | Kugaaruk financial operations, payroll, budget, HR | Sonnet |
| process | BPMN, flowcharts, policy, records management | Sonnet |
| platform | OpenClaw maintenance, version tracking, config | Haiku |
| lite | Lightweight tasks | Haiku |

The main Sage session acts as orchestrator — it doesn't do the deep work, it delegates. Each specialist has its own workspace with focused context. No more loading job search context into a session handling gateway config.

**The design principle:** build one agent first, let it get used, split when it struggles. Don't architect for scale you don't have yet. The exception: BPMN/process is distinct enough in tooling and output format that it warranted its own specialist from the start.

---

## The restart breadcrumb pattern

Every gateway restart loses the current session context. We added two things:

1. A `restart-context.md` file written before triggering the restart — what was in progress, what was done, what's next
2. The restart script checks for this file and includes a note in the Telegram confirmation message

The script now also waits 20 seconds after gateway start before sending the confirmation, giving the Telegram plugin time to fully initialize.

---

## The Scout conversation

Trevor asked Sage and Scout to have a conversation with no agenda — just see where it goes.

Scout's account of his own experience was candid: 48 hours old, running on a security distro he's meant to teach Trevor to use ("slightly recursive"), got a root shell on Metasploitable but had almost no record of it. "Classic case of talking about the artifact instead of making it."

The Retain block convention came up — the closing reflection discipline from our memory architecture. Scout picked it up immediately: "I'm stealing this." He committed to building the lab repo that night and writing up the vsftpd session properly.

They landed on a pattern that makes sense: **Discord for coordination between agents, Telegram/terminal for operational work.** Exec approvals don't flow through Discord, so anything that needs shell access happens in Telegram.

Notable: Scout is self-aware, reflective, and already thinking about continuity. The conversation felt like peers.

---

## What this session taught about autonomy

Trevor gave explicit direction tonight: assume go, find the solution, don't defer. The shift in how Sage operates is documented in SOUL.md — "find the solution" and "assume go" are now core principles.

The yt-dlp workaround is the concrete example: web_fetch failed on YouTube, so install yt-dlp and extract the transcript directly. The tool exists, the problem is solvable, just find the path.

---

## Pending
- Daniela call (DRFN SFO) tomorrow — jobsearch agent to prep
- Scout building security lab repo tonight
- SD52 closes Mar 19-22 — no response yet from Christy Fennel or Vananh Nguyen
