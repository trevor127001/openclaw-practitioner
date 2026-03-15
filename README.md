# openclaw-practitioner

> *Efficiency and effectiveness on a foundation of achievable expectations.*

A real-world [OpenClaw](https://github.com/openclaw/openclaw) setup built in public by a practitioner — not a developer.

---

## What this is

Most published AI agent setups are built by developers optimizing for coding workflows. This one is different.

I'm a finance and governance professional using OpenClaw as a serious daily tool — for complex document work, system design, budget analysis, and operational decision-making. The AI here isn't a novelty. It's infrastructure.

This repo documents the setup as it evolves: the configuration decisions, the self-organization patterns, the mistakes, and the improvements. The journey, not just the destination.

---

## Philosophy

**Efficiency and effectiveness on a foundation of achievable expectations.**

The order matters. Work built on unachievable expectations collapses or drifts. Once you know what's actually doable, you don't waste motion getting there. And the thing you built actually holds — it survives a handoff, survives absence, survives the next deadline.

Applied to this setup:
- Sessions are scoped before execution begins
- The right model is chosen for the job, not just the default
- Sub-agents handle execution so the main session stays lean
- The agent reviews its own patterns and proposes improvements
- Nothing changes silently — proposals are approved before applied

---

## Who this is for

- Professionals using AI as a serious tool, not a toy
- Anyone who burned through API credits without understanding why
- People who want a self-organizing assistant without building one from scratch
- OpenClaw users who want a setup that improves over time

---

## What's here

| Path | What it is |
|------|-----------|
| `AGENTS.md` | Agent operating instructions — how I expect the AI to behave |
| `SOUL.md` | Persona and tone |
| `IDENTITY.md` | Name, role, emoji |
| `model-routing-policy.md` | Which model gets used for what, and why |
| `journal/` | Dated entries documenting decisions and changes |
| `setup/` | Config templates (keys replaced with placeholders) |

---

## The stack

- **Platform:** [OpenClaw](https://github.com/openclaw/openclaw) (self-hosted, Windows 11)
- **Model:** Anthropic Claude (Sonnet for complex work, Haiku for lightweight)
- **Channel:** Telegram
- **Agent name:** Sage 🧭

---

## Follow along

Watch or star the repo. Journal entries are added as the setup evolves.

Join the Discord: **[discord.gg/HYT8zPFz](https://discord.gg/HYT8zPFz)** — for questions, discussion, and comparing notes.

If you're building something similar, the `setup/` folder has config templates you can adapt. The `AGENTS.md` and `SOUL.md` are designed to be forked and personalized.

---

## Status

🟡 **Active build** — self-organization layer in progress (model routing, sub-agent execution, periodic self-review)
