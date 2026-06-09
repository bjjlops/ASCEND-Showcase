# What's been updated

> A plain-language log of the meaningful changes to **ASCEND Solutions** — what shipped, what
> moved, and what's now switched on. It's grouped by theme rather than by day, because for a
> solo-led project the *order* of decisions matters more than the calendar. Newest themes first.

For where things are *headed*, see **[roadmap.md](roadmap.md)**. For the honest status of each
product, see **[products.md](products.md)**.

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

> Until now the showcase described APEX as "in development / not yet running against live models."
> That's no longer accurate — it's live. The *constraints* are what stayed the same.

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

The marketing site became the **identity hub**: you sign in once, and an **/app launcher** sends you
into the Planner or Trader. A shared **parent-domain session** makes the whole ecosystem feel like
one product across subdomains, even though each surface is its own app. The Trader now shares the
**same account pool** as the Planner — **one login** works across both products.

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

## 💳 Billing: integrated, in test mode

A checkout path now exists — **Stripe checkout, an idempotent webhook, and an entitlements model** —
but it's deliberately in **test mode**. There are **no live charges**, and a paid entitlement is only
ever granted by the verified server-side webhook, never by a client claim or a return URL. It turns
on for real only when there's something worth charging for.

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
