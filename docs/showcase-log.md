# Ship log

> A running, dated record of what actually shipped — the honest version, including the bugs that
> had to be beaten to call something "done." Newest first. For the thematic summary, see
> **[changelog.md](changelog.md)**; for direction, see **[roadmap.md](roadmap.md)**.

---

## 2026-06-08 — AI live across ASCEND, and one login

The day APEX stopped being "designed to be safe but switched off" and became **on, in production**,
across both products — plus a unified sign-in so the ecosystem is genuinely one account.

### 🔐 One shared login (SSO)

- Repointed the **Trader's** sign-in at the **Planner's** Supabase account (env config + a shared
  `.ascenddaily.app` cookie).
- The Trader's own user pool turned out to be empty (0 users, 0 data), so this was a clean **config**
  change — no risky data migration.
- **Result:** one account now works across the Planner and the Trader. Sign in once.

### 🤖 AI turned on everywhere (via Nyquest)

Shipped the full AI integration to production (73 files) and switched it **on** in the live backend.

- **Planner:** APEX conversational chat grounded in your real schedule, AI day-analysis, and weekly
  summaries.
- **Trader:** AI trade coach, research summarizer, risk review, and strategy extraction.
- Everything routes through **one shared AI client → a Cloudflare Worker → Nyquest**
  (`openrouter/auto`), behind safety guardrails (crisis gate, prompt-injection screening,
  attribution) with **audit hooks and real per-call cost capture**.
- Models actually being routed to in early use: `deepseek-chat-v3.1`, `claude-haiku-4.5`,
  `qwen3-235b-a22b-thinking-2507` — auto-selected per request.
- **Real-money trading stays OFF by design.** Going live meant turning on *intelligence*, not
  *execution*.

### 🐛 Production bugs found & fixed (the hard part)

Going live is a test of your constraints, not your features — and of your error logs. What broke,
and what fixed it:

- **"Illegal invocation" `this`-binding bug** in three network clients. Node tolerated it;
  Cloudflare Workers didn't — and it was silently breaking *every* AI call. Fixed all three.
- **Coach rendered blank** — for auto-routed models the gateway returns one JSON blob instead of a
  live token stream; taught the reader to handle both.
- **APEX returned raw JSON** — tightened the prompt so it replies in plain language.
- **Apps couldn't find the backend** — set `AI_PLATFORM_URL` on both apps.
- **Upgraded error logging** so failures are actually diagnosable. (This is what cracked the bugs —
  the logging was the fix that found the fixes.)

### 🧱 Pipeline & docs

- Fixed a CI check that was failing on its own documentation.
- Started this ship log.

---

*Earlier milestones — the monorepo migration, the shared platform layer, the hybrid deploy, the
Trader workbench reset — are summarized in **[changelog.md](changelog.md)**.*
