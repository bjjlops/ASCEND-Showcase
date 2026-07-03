# Ship log

> A running, dated record of what actually shipped — the honest version, including the bugs that
> had to be beaten to call something "done." Newest first. For the thematic summary, see
> **[changelog.md](changelog.md)**; for direction, see **[roadmap.md](roadmap.md)**.

---

## 2026-07-02 — Billing live end to end + a production-hardening pass

The day billing stopped being "integrated but in test mode" and became **real, verified, and on** —
on top of a platform-wide hardening pass so a paid launch is trustworthy from a clean environment.

### 💳 Real billing, verified end to end

- Shipped both plans live: **Planner Pro** ($5/mo subscription) and **Planner Lifetime** ($199
  one-time), through **Stripe Checkout**.
- Walked the **full path on real infrastructure** — checkout → payment → automatic account upgrade —
  and confirmed premium features unlock the instant payment clears.
- **Entitlement is server-truth only:** a paid plan is granted solely by the verified server-side
  webhook — never by a client claim or a return URL. No fake "payment success" states.
- Added self-serve **"Manage billing"** so users manage their own plan.
- Wired the **"APEX is part of Planner Pro"** upgrade prompt so the value is clear before payment.

### 🔒 Production-hardening pass

- **Fail-closed access walls** — every gate defaults to *deny*; a missing/invalid check blocks access
  rather than leaking it.
- **Per-user rate limiting** across sensitive surfaces; no data left exposed.
- **Reproducible from clean** — every app is deployable with **automatic migrations**, so the system
  stands up from a fresh environment.
- **Launch essentials** — **/privacy** and **/terms** pages, **per-user time zones** so "today" is
  correct for every user, safer AI-chat safety UX, and waitlist abuse protection.

### 📸 Screenshots to add

Pending capture into `../assets/screenshots/` (then embed here + in recent-updates + README): the
pricing page with live Subscribe / Get Lifetime buttons, the Stripe Checkout page, the "Payment
received" confirmation banner, the "Manage billing" account card, the "APEX is part of Planner Pro"
upgrade prompt, and the /privacy and /terms pages.

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

![Nyquest usage — per-call cost capture](../assets/screenshots/platform-usage.png)

*Real metering, not a mock: per-request counts, tokens, and **per-call cost**, with the auto-router
fanning out across models (`deepseek-chat-v3.1`, `claude-haiku-4.5`, `qwen3-235b-a22b-thinking-2507`).*

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
