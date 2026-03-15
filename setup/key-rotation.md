# API Key Rotation Procedure

**Frequency:** Bi-weekly (every two weeks, Sundays)
**Trigger:** Cron reminder + always immediately on exposure

---

## Keys to rotate

| Key | Where | Notes |
|-----|-------|-------|
| Anthropic API key (TreZel-01) | `auth-profiles.json` | Main Sage instance |
| Anthropic API key (Kali/Scout) | Scout's `auth-profiles.json` | Separate key — separate blast radius |
| OpenAI API key | `auth-profiles.json` | Fallback model |
| Telegram bot token (@trevor_assistant_bot) | `openclaw.json` | Rotate only on exposure |
| Telegram bot token (@sage_too_bot) | Scout's `openclaw.json` | Rotate only on exposure |
| Discord bot token | `openclaw.json` | Rotate only on exposure |

---

## Steps

### Anthropic key (TreZel-01)
1. Go to [console.anthropic.com](https://console.anthropic.com) → API Keys → Create Key
2. Copy the new key
3. Ask Sage to update `auth-profiles.json` — **do not paste the full file anywhere**
4. Revoke the old key in the console
5. Restart gateway via `scripts/restart-gateway.ps1`

### Anthropic key (Scout/Kali)
1. Same as above — generate a separate key scoped to Scout
2. Update Scout's `auth-profiles.json` on the Kali instance
3. Restart Scout's gateway

### OpenAI key
1. Go to [platform.openai.com](https://platform.openai.com) → API Keys → Create New
2. Same update/revoke process as Anthropic

### Telegram/Discord tokens (on exposure only)
- Telegram: BotFather → `/mybots` → select bot → `API Token` → `Revoke current token`
- Discord: Developer Portal → your app → Bot → Reset Token
- Update `openclaw.json` and restart gateway

---

## Security rules

- **Never paste `openclaw.json` or `auth-profiles.json` anywhere** — they contain live secrets
- If you need to share config for troubleshooting, ask Sage to sanitize it first
- Scout's Anthropic key should be separate from TreZel-01's — limits exposure if one is compromised
- After any rotation, verify the gateway starts cleanly before revoking the old key
- **Scout-specific:** Scout cannot self-repair if his key is revoked before a replacement is in place — he has no brain without a working API key. Always update Scout's config and confirm he's responding via sage_too_bot *before* revoking the old key

---

## The billing lockout trap

If a billing error occurs, OpenClaw writes a backoff timestamp to `auth-profiles.json`. Adding credit does **not** clear it — you'll be locked out for up to 5 hours. Fix: edit `auth-profiles.json` and remove the backoff entry, then restart the gateway.
