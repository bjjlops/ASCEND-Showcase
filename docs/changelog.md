# What's been updated

> A plain-language log of the meaningful changes to **ASCEND Solutions** — what shipped, what
> moved, and what's now switched on. It's grouped by theme rather than by day, because for a
> solo-led project the *order* of decisions matters more than the calendar. Newest themes first.

For where things are *headed*, see **[roadmap.md](roadmap.md)**. For the honest status of each
product, see **[products.md](products.md)**.

---

## 🏠 The Profile Hub — one place to manage everything

The old post-login screen was a bare "pick an app" launcher. It's now the **Profile Hub** at
**ascenddaily.app/app** — a real account home and the **single self-serve place** to manage everything:
subscribe, unsubscribe, fix a failed card, edit your profile. Every "upgrade" prompt across the platform
now points here. It still launches the solutions (the routing is preserved) — it just does far more than
route. Four sections:

- **Your account** — name (editable right there, syncs with planner settings), email, sign out.
- **Your solutions** — one card per product (Planner, Trader) with a **status badge** (Free / Pro /
  Lifetime / Payment issue), the **AI benefits that solution unlocks**, an **"Open"** button, and
  **buy/manage** buttons.
- **Billing summary** — **total per month**, **next renewal date**, and a **"Manage payment & invoices"**
  button that opens the **Stripe** secure customer portal (update card, view invoices, cancel).
- **Account** — **change password** without leaving the page.

> This reframes the earlier "**/app launcher**" note under *Hub-and-spoke identity* — the *what* changed
> (a bare pick-an-app screen became a full account home), the *how* didn't (one sign-in still routes you
> into either solution).

---

## 🔒 AI gating — no subscription, no AI

AI used to be free to any signed-in user. Now a solution's **AI benefits require that solution's
subscription** — a fail-closed line drawn on purpose, not a paywall bolted on. The **free core stays
free**; only the AI on top is gated.

- **Planner — paid AI** (needs the Planner sub): **APEX chat**, dashboard **AI insights**, **day
  analysis**, **weekly summary**.
- **Planner — still free**: tasks, habits, planner, reviews, stats.
- **Trader — paid AI** (needs the Trader sub): **AI coach chat**, **research summaries**, **"paste &
  extract"**, **AI risk review**.
- **Trader — still free**: manual research, journaling, the rule-based daily review.
- **No broken screens.** Free users see a clean **"this is part of Pro — $5/month"** prompt that links
  to the Profile Hub — never a raw error or a dead page.

**Enforced at three layers**, so it can't be bypassed: (1) in each app's **pages** (the friendly UI
prompt), (2) in each app's **server routes** (the real block), and (3) at the **central AI backend**
every AI request must pass through (the backstop — even a bug in an app can't leak free AI). That third
layer maps onto the existing **private AI boundary / Nyquest gateway**: the boundary is now **also** the
entitlement backstop.

**Cancel-grace:** cancel a sub and AI keeps working through the end of the period you already paid for,
then shuts off **within about a minute**.

---

## 💳 Billing is live — per-solution, additive, verified end to end

Billing left test mode, and the model matured with it. There's now something worth charging for, the
flow is genuinely complete — so it's **on**. The shape changed too: billing is **per-solution and
additive**, not one flat product.

- **Priced per solution.** Each solution — the **Planner**, the **Trader** — is **$5/month**, and they
  stack on **one subscription and one invoice**: the Planner alone is **$5/mo**; add the Trader and
  you're at **$10/mo** — still one subscription, one invoice. Each solution also has a **$199 lifetime**
  option (pay once for that solution, never again). "Pro" no longer names a single product — it just
  means *this solution's paid tier is active.*
- **Adding is smooth.** Your **first** solution goes through **Stripe Checkout**; **additional**
  solutions are charged **instantly to the card on file, prorated, with no second checkout.**
- **Removing is fair.** Remove a solution and you get **immediate removal with prorated credit** for the
  unused time; removing your **last** solution **cancels the subscription**. Buy a **lifetime** while on
  monthly and the system **automatically removes the now-pointless $5 monthly item** — nobody pays twice
  for the same solution.
- **The webhook is still the only door.** A paid entitlement is granted **only** by the verified
  server-side **Stripe** webhook — never by a client claim or a return URL, and never a fake "payment
  success" state. That discipline didn't loosen; it got **hardened** against real edge cases: **retries
  and duplicate delivery (idempotent)**, **duplicate subscriptions started from two browser tabs**, **a
  later monthly event overwriting a lifetime purchase**, and **price-ID changes accidentally revoking
  paying customers.**
- **The operator's numbers stay honest.** In **ASCEND Control**, revenue math now **counts per-solution
  correctly** and **does not count free comps** (access handed out for free) as revenue.

> This supersedes the earlier "Billing is live — **Planner Pro** ($5/mo) & **Planner Lifetime** ($199)"
> framing — the *what* changed (a single **Planner Pro** product became **per-solution, additive**
> pricing, **$5/mo** or **$199 lifetime** *each*), the *how* didn't (entitlements are still earned by a
> verified **Stripe** webhook, never a client claim).

---

## 🚀 A production-hardening pass for a trustworthy launch

A paid launch has to be trustworthy end to end, so the whole platform got hardened alongside billing.

- **Secure by default** — every access wall **fails closed**, **per-user rate limiting** is in place,
  and no data is left exposed.
- **Reproducible & deployable** — every app deploys from a clean environment with **automatic
  migrations**.
- **Launch essentials** — **/privacy** and **/terms** pages are live, **per-user time zones** compute
  the correct "today" everywhere, safer AI-chat safety UX, and the waitlist is protected against abuse.

---

## 🛰️ An internal control plane + a real cost ledger

With AI live in production, ASCEND needed an operator's view — and a way to know exactly what each
model call costs. Both shipped together.

- **ASCEND Control** — a new internal admin command center (not customer-facing): a 360° account
  view, access approvals/gating, subscriptions and revenue, AI usage, Stripe webhook deliveries,
  live system health, and an **immutable audit log** of every privileged action.
- **A per-call AI cost ledger** — the AI platform now persists a usage event per request (tokens,
  model, status, latency, and **real cost**) into a durable, idempotent ledger, fire-and-forget so it
  never slows a response. Control reads it to show spend by model, product, and account — so AI cost
  is **measured, not estimated.**
- **Guarded by three fail-closed walls** — Cloudflare Access at the edge, an origin identity
  re-verify, and a database-level admin allowlist; privileged reads are narrow and account changes
  are audited.

Architecture detail: **[architecture.md → the internal control plane](architecture.md#the-internal-control-plane)**.

---

## 🧠 APEX is now live (early access)

The biggest recent change: **APEX — the shared AI intelligence layer — is now switched on in
production**, across both the Planner and Trader surfaces.

- Planner APEX runs **day analysis**, **weekly summaries**, and the **APEX chat** assistant.
- Trader APEX runs **research summaries**, **risk review**, **strategy extraction**, and a
  **streaming coach** — all research/analysis only.
- It runs behind the **private AI platform boundary** (the apps never call a model directly), and
  reaches models through **Nyquest**, a single private multi-model gateway.
- The discipline didn't change when it went live: **deterministic product logic still runs first**,
  the guardrails (prompt-injection checks, no-invention, financial-advice limits, evidence floors)
  are **active**, and every AI feature still sits behind a **kill-switch flag** that can turn it off
  instantly.
- **APEX is now gated behind a subscription.** APEX chat and the other Planner/Trader AI features are
  part of each solution's paid tier — see **[AI gating](#-ai-gating--no-subscription-no-ai)** above for
  the exact free-vs-paid split and the three-layer enforcement.

> Until now the showcase described APEX as "in development / not yet running against live models."
> That's no longer accurate — it's live. The *constraints* are what stayed the same.

> This also supersedes any earlier reading of APEX as **free for every signed-in user** — the *what*
> changed (APEX AI now requires the relevant solution's subscription), the *how* didn't (it still runs
> behind the private AI boundary, deterministic-first, with the guardrails and kill-switch intact).

---

## 📈 Trader: a full research-and-risk workbench

ASCEND Trader went through a multi-phase "reset" and is now a genuinely usable, customer-facing
research workbench — and it's surfaced as **live** in the app launcher (research only; it still
**never places orders**).

What landed:

- **Information architecture + shell reset** — cleaner navigation, consolidated Config/Guide,
  at-a-glance dashboard, real loading and empty states.
- **Deterministic trading coach** — rule- and behavior-based coaching with a streaming chat surface.
- **News + economic calendar** — a news tab and a "red-folder" high-impact economic calendar.
- **Connect your own broker (read-only)** — OAuth pop-up login and API-key connect for **read-only**
  account inspection (Alpaca), used purely for readiness/safety checks. Webull was removed.
- **Paper / simulation tooling** — local backtests across deterministic strategies, a paper-trading
  session runner with a **kill switch**, and parameter sweeps.

The safety floor is unchanged and structural: the order-placement endpoint returns a **blocked**
response server-side, and live trading stays **off by policy**.

---

## 🏗️ The monorepo migration is complete

ASCEND used to be a set of separate repositories — one per app and library — with all the cross-repo
linking pain that implies. It's now **one pnpm + Turborepo monorepo**, and the migration is done.

- Every surface — **hub/marketing site, web planner, trader, AI platform** — now lives in one repo.
- A real **shared platform layer** was extracted into workspace packages: `@ascend/config`
  (build presets), `@ascend/tokens` (design tokens), `@ascend/auth` (Supabase SSR), `@ascend/ui-web`
  (UI primitives), `@ascend/shared-contracts` (API schemas), `@ascend/ai-client` (typed AI client),
  `@ascend/entitlements`, and `@ascend/billing`.
- Cross-repo token-gated installs are **gone** — shared code is consumed directly as `workspace:*`.

More on *why* this was worth the pain in **[../LEARNINGS.md](../LEARNINGS.md)**.

---

## 🔑 Hub-and-spoke identity

The marketing site became the **identity hub**: you sign in once, and **/app** — now the
**[Profile Hub](#-the-profile-hub--one-place-to-manage-everything)**, an account home that also launches
the solutions rather than a bare pick-an-app screen — sends you into the Planner or Trader. A shared
**parent-domain session** makes the whole ecosystem feel like one product across subdomains, even though
each surface is its own app. The Trader now shares the **same account pool** as the Planner — **one
login** works across both products.

---

## ☁️ Hybrid deployment + CI guardrails

ASCEND now runs as a **hybrid deployment** and ships through a real pipeline:

- Application surfaces deploy to one cloud; **DNS/edge and the private AI platform** run on another
  (the AI platform is a **Cloudflare Worker** at `api.ascenddaily.app`).
- **CI runs the whole graph** — typecheck, build, and tests — plus an **AI-boundary guard** that
  fails the build if a model-provider key or privileged database key ever leaks toward a client app,
  and a **trader safety check** that fails if the unsafe defaults (live trading / order placement)
  are ever flipped on.

---

## 🎨 A refined design language

The shipped design tokens were consolidated into one source of truth (`@ascend/tokens`) and the
palette matured: a deeper, **muted gold** as the primary accent, a **calm teal** as the secondary,
and a near-black green-tinted background. Details in **[design-system.md](design-system.md)**.

---

## What hasn't changed

The principles held through all of it:

1. Apps never hold privileged keys.
2. AI never invents your data.
3. Trader never trades for you — order placement is blocked server-side.
4. Marketing only advertises what's real.
