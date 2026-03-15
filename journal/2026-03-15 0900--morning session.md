# 2026-03-15 — Morning Session: Discord, Cross-Context, Logging, and Hard Lessons

## What we built

- Discord server (`openclaw-practitioner`) created and live: discord.gg/HYT8zPFz
- Sage added to the server with Send Messages permission
- Trevor paired with Sage via Discord DM
- Cross-context messaging enabled (Telegram → Discord)
- Logging redirected to a permanent location: `~/.openclaw/logs/`
- Restart script (`scripts/restart-gateway.ps1`) written and validated
- README updated with Discord link and committed

---

## The billing lockout — a hidden trap

During the session, a billing error triggered a 5-hour backoff written to `auth-profiles.json`. Trevor added credit immediately — and was still locked out. The error gave no indication of a cooldown. It took significant effort and creative prompting to find the actual cause.

**The fix:** edit or delete the backoff entry in `auth-profiles.json` to clear it manually.

**The lesson for practitioners:** if you hit a billing error, don't just add credit and retry. Check `auth-profiles.json` for a backoff timestamp. OpenClaw will not tell you it's waiting — it will just fail.

This is not documented anywhere obvious. It should be.

---

## Cross-context messaging — too many round trips

Getting Sage to post to Discord from a Telegram session took far longer than it should have. Here's the honest account:

**What should have happened:**
1. Check docs → find the FAQ entry for "Cross-context messaging denied"
2. Find the correct config path (`tools.message.crossContext.allowAcrossProviders`)
3. Apply it, restart, test — one turn

**What actually happened:**
- Sage sent Trevor to the Discord DM session to post the question
- That session had no context and couldn't post to the OpenClaw community server anyway
- Multiple config attempts failed because the path was tried at `agents.defaults.tools` instead of top-level `tools`
- The gateway validator rejected it each time
- Trevor had to post the question to Krill (OpenClaw admin) directly
- Krill confirmed the correct path — top-level `tools`, not `agents.defaults.tools`

**The root cause:** Sage explored in public instead of researching first. Each failed attempt cost Trevor time and attention.

**The working config (v2026.3.13):**
```json
{
  "tools": {
    "message": {
      "crossContext": {
        "allowAcrossProviders": true,
        "marker": {
          "enabled": true,
          "prefix": "[from {channel}] "
        }
      }
    }
  }
}
```

Note: the docs show this under `agents.defaults.tools.message` — that path is **rejected** by the current version's validator. Top-level `tools` is correct.

---

## API key exposure — what to never do

`openclaw.json` and `auth-profiles.json` contain live secrets: Telegram bot token, Discord bot token, API keys. Trevor pasted `auth-profiles.json` into Claude Desktop during the billing lockout investigation. The Anthropic and OpenAI keys were rotated as a result.

**The rule going forward:** never paste either file anywhere. If config needs to be shared for troubleshooting, ask Sage to sanitize it first — all tokens replaced with `[REDACTED]` before sharing.

---

## The restart script pattern

Direct `openclaw gateway restart` is banned. Every gateway change goes through `scripts/restart-gateway.ps1`, run by Trevor after explicit confirmation.

Why: Sage ran `gateway restart` twice without confirmation during this session, stopping the gateway mid-conversation both times. The script pattern prevents this — Sage prepares, Trevor runs.

The exec tool also proved unreliable for this — the script ran but the result was swallowed. Running the script directly in PowerShell is the reliable path.

---

## What's working well

- Discord integration: DM pairing, channel reads, cross-context posting — all live
- GitHub Desktop re-cloned cleanly after the unrelated histories error
- Logs now writing to a permanent, reviewable location
- The restart script is solid and tested

---

## Pending

- Sage approved to the OpenClaw community server (pending Krill)
- Scout (Kali agent) identity files not yet set up
