# Ship log

> A running, dated record of what actually shipped — the honest version, including the bugs that
> had to be beaten to call something "done." Newest first. For the thematic summary, see
> **[changelog.md](changelog.md)**; for direction, see **[roadmap.md](roadmap.md)**.

---

## 2026-07-10 — Per-solution billing, three-layer AI gating, and the Profile Hub

The day the business model became **per-solution and additive**, AI benefits started **requiring a
subscription**, and every account got a real self-serve home. Billing is now **webhook-truth only and
fail-closed** end to end — verified with real money, not a mock. Nothing loosened on the safety side:
the Trader still **researches, never trades** (order placement stays blocked server-side), and AI
**never invents your data**. Gating is fail-closed discipline, not a paywall grab.

### 💳 Per-solution billing (2 products, 4 prices)

- Billing is now **per-solution and additive** on **one subscription and one invoice**. Each solution
  — the **Planner**, the **Trader** — is **$5/month**, or **$199 lifetime** (pay once for that
  solution, never again). Planner alone = **$5/mo**; add the Trader and you're at **$10/mo** on the
  same invoice. There is no single "Planner Pro" product anymore — **"Pro" now just means "this
  solution's paid tier is active."**
- **Add a solution:** the *first* solution goes through **Stripe Checkout**; **additional** solutions
  are charged **instantly to the card on file, prorated, with no checkout**.
- **Remove a solution:** **immediate removal with prorated credit** for unused time. Removing your
  **last** solution **cancels the subscription**.
- **Buy lifetime while on monthly:** the system **automatically removes the now-pointless $5 monthly
  item** so nobody pays twice for the same solution.

> This supersedes the earlier "**Planner Pro** ($5/mo) and **Planner Lifetime** ($199)" note below —
> the *what* changed (billing is per-solution and additive across the Planner and the Trader, not one
> flat plan), the *how* didn't (still **Stripe Checkout** for the first buy, still upgrade on cleared
> payment, still no fake success states).

### 🔐 Webhook hardening (the only pathway that grants access)

Stripe reports every payment event to the server, and **that webhook pathway is the ONLY thing that
can grant access** — never a client claim, never a return URL. No fake "payment success" states.
Hardened against the real edge cases:

- **Retries / duplicate delivery** — processing is **idempotent**, so a redelivered event can't
  double-grant or corrupt state.
- **Duplicate subscriptions** created from **two browser tabs** racing checkout.
- **A later monthly event overwriting a lifetime purchase** — lifetime is protected from being
  clobbered by a stray subscription event.
- **Price-ID changes accidentally revoking paying customers** — a swapped price can't silently strip
  access from someone who paid.

### 🧪 Verified with real money — the canary

- Live **Stripe fully provisioned:** **2 products, 4 prices, a webhook, and the customer portal** —
  the portal configured so **plan changes can only happen through the app** (app-only plan changes).
- **A real $5 Planner subscription ran the entire chain** — charge → webhook → access granted, paid
  with real money, not a mock — and it's **left running as a live canary** on the production path, so
  a break anywhere in payment→access surfaces there first.

### 🤖 AI gating, enforced at three layers

Previously almost all AI was free to any signed-in user. Now a solution's **AI benefits require that
solution's subscription** — the core stays free:

- **Planner AI — paid** (requires the Planner sub): **APEX** chat, dashboard AI insights, day
  analysis, weekly summary. **Still free:** tasks, habits, planner, reviews, stats.
- **Trader AI — paid** (requires the Trader sub): AI coach chat, research summaries, "paste &
  extract", AI risk review. **Still free:** manual research, journaling, the rule-based daily review.

Enforced at **three layers** so it cannot be bypassed:

1. inside each app's **pages** — the nice UI prompt,
2. inside each app's **server routes** — the real block,
3. at the **central AI backend** every AI request must pass through — the backstop, so even a bug in
   an app cannot leak free AI.

This maps onto the existing **private AI boundary / Nyquest gateway**: the boundary is now **also the
entitlement backstop**. Free users see clean **"this is part of Pro — $5/month"** prompts linking to
the hub — **never broken screens or raw errors**. **Cancel-grace:** cancel your sub and AI keeps
working until the end of the period you already paid for, then shuts off **within about a minute**.

On the admin side, **ASCEND Control**'s revenue math now **counts per-solution correctly** and
**does not count free comps** (free access handed out) as revenue.

### 🏠 The Profile Hub (post-login home)

The old bare "pick an app" launcher became a real account page at **ascenddaily.app/app** — the
single self-serve place customers manage everything, and where **every** "upgrade" prompt across the
platform now points. It still launches the solutions (routing preserved). Four sections:

- **Your account** — name (editable right there, syncs with planner settings), email, sign out.
- **Your solutions** — one card per product showing a **status badge (Free / Pro / Lifetime / Payment
  issue)**, the **AI benefits that solution unlocks**, an **Open** button, and **buy/manage** buttons.
- **Billing summary** — **total per month**, **next renewal date**, and a **"Manage payment &
  invoices"** button that opens the **Stripe secure customer portal** (update card, view invoices,
  cancel).
- **Account** — **change password** without leaving the page.

### 🎨 Login & signup facelift

Retired the boxes-inside-boxes login (bordered panel + bordered card + bordered messages + bordered
inputs). New: on **desktop**, exactly **one card** next to a clean **gold-wash brand panel**; on
**mobile**, the form sits **directly on the page with no box at all**. Same login, same accounts,
same security — purely a cleaner first impression, applied identically to both the main site and the
planner login.

### 🐛 Deploy-pipeline fixes

- **AI backend deploy pipeline** had been broken since setup — **missing Cloudflare credentials** — so
  the Worker never auto-deployed. Fixed; it now **auto-deploys on push**.
- **Trader deploys** were failing on an **asleep database** plus a **connection string Vercel could
  not reach**. Fixed; the Trader is **live at trader.ascenddaily.app**.

### ✅ Tests

- **~40 new automated tests** covering the per-solution billing logic and the gating, **all green in
  CI**.

### 📓 Runbook

The runbook documenting every key, switch, and rollback lever lives at **`docs/provisioning-checklist.md`
in the product monorepo** — **not** in this showcase repo, so there's nothing to link here.

### 📸 Screenshots to add

Pending capture into `../assets/screenshots/` (then embed here + in recent-updates + README): the
**Profile Hub** (the account section, the per-solution cards with **Free / Pro / Lifetime / Payment
issue** status badges, and the billing summary), the **per-solution buy & $199-lifetime** buttons,
the **Stripe secure customer portal**, the **change-password** panel, and the **login/signup
facelift** (one card + gold-wash panel on desktop, no-box form on mobile).

---

## 2026-07-02 — Billing live end to end + a production-hardening pass

The day billing stopped being "integrated but in test mode" and became **real, verified, and on** —
on top of a platform-wide hardening pass so a paid launch is trustworthy from a clean environment.

### 💳 Real billing, verified end to end

> Superseded by the **2026-07-10** per-solution model above — the flat "**Planner Pro** / **Planner
> Lifetime**" framing below is kept as record. Pricing is now **$5/mo** or **$199 lifetime** *per
> solution* (additive on one subscription), not a single product.

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
**Stripe Checkout** page, the "Payment received" confirmation banner, and the **/privacy** and
**/terms** pages. *(The retired single-product targets — pricing page, "Manage billing" card, "APEX
is part of Planner Pro" prompt — are replaced by the per-solution surfaces listed under the
**2026-07-10** milestone above: the Profile Hub, per-solution buy/lifetime buttons, and the Stripe
secure portal.)*

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
