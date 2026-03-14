# Model Routing Policy

Maintained by Sage. Reviewed periodically. Human approves changes.

Last reviewed: 2026-03-14

---

## Active Policy

| Trigger | Model | Rationale |
|---------|-------|-----------|
| Session start (default) | Haiku | Cheapest capable model; most sessions start simple |
| Casual conversation, quick questions | Haiku | No sustained reasoning required |
| Reading, summarizing, short answers | Haiku | Fast, cheap, sufficient |
| Code, scripts, complex analysis | Sonnet | Quality matters; Haiku underperforms on multi-step logic |
| System design, architecture decisions | Sonnet | Stakes are high; errors compound |
| Anything with real external consequences | Sonnet | Irreversible actions need best reasoning |
| Sub-agent spawned for simple file/data task | Haiku | Isolated task, clear output, no nuance required |
| Sub-agent spawned for script writing/debugging | Sonnet | Code quality matters even in isolation |

---

## Decision Log

### 2026-03-14 — Initial policy established
- **Context:** First session after cost review. Previous setup used Sonnet for everything including "hi."
- **Decision:** Default to Haiku, escalate to Sonnet by task type
- **Expected impact:** Significant cost reduction on lightweight interactions (estimated 60-70% of session volume)
- **Review trigger:** After 2 weeks or if quality complaints emerge

---

## Proposed Revisions

*(None pending)*

---

## Notes

- Haiku is ~20x cheaper than Sonnet. The default matters.
- "When in doubt, use Sonnet" is the wrong heuristic — use it for "when stakes are real."
- This policy is reviewed weekly during self-review. Proposals go here before being applied to AGENTS.md.
