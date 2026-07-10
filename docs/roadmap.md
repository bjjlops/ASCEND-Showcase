# Roadmap & status

A high-level, honest view of where **ASCEND Solutions** is and where it's heading. Dates are
intentionally omitted — this is a solo-led project, and the order matters more than the calendar.
For the log of what *recently* changed, see **[changelog.md](changelog.md)**.

## Status at a glance

| Surface / capability | Status |
| --- | --- |
| Web Planner | 🟢 Live — free early access |
| Hub / marketing site | 🟢 Live |
| APEX (AI layer) | 🟢 Live — deterministic-first & guardrailed; **AI now gated behind the relevant solution's subscription** (no longer free to every signed-in user) |
| Trader | 🔵 Live — research only (no execution) |
| Mobile Planner | 🟡 In development |
| Billing | 🟢 Live — **per-solution**, **$5/mo** each (additive — one subscription, one invoice) + **$199 lifetime** each |
| Connected trade execution | ⛔ Gated behind a written safety checklist |

## ✅ Done

- **Platform foundations** — one pnpm + Turborepo monorepo, a shared package layer
  (contracts, auth, tokens, UI, typed AI client, entitlements, billing), and enforced security
  boundaries.
- **Hub-and-spoke identity + Profile Hub** — sign in once at the hub; a shared parent-domain session
  routes you across the ecosystem. The post-login **/app** home is now the **Profile Hub** — a real
  account page and the single self-serve place to manage everything (subscribe, unsubscribe, fix a
  failed card, edit your profile), not a bare launcher. It still launches the **Planner** and the
  **Trader**, and every "upgrade" prompt across the platform points here.
- **Hybrid deployment + CI** — application surfaces on one cloud, the private AI platform as a Worker
  on another; CI runs the whole graph plus an AI-boundary guard and a trader safety check.
- **APEX live — now gated per solution** — planner day analysis, weekly summaries, and chat; trader
  research summaries, risk review, strategy extraction, and a streaming coach — all behind the private
  boundary, guardrailed, and kill-switchable. These AI benefits are now **paid**: a solution's AI
  requires that solution's subscription, enforced at **three layers** — the app's pages (the prompt),
  the app's server routes (the real block), and the central AI backend every request passes through
  (the backstop, so even a bug in an app can't leak free AI). The core stays **free** — Planner tasks,
  habits, planner, reviews, and stats; Trader manual research, journaling, and the rule-based daily
  review. Free users see clean **"part of Pro — $5/month"** prompts linking to the hub, never broken
  screens or raw errors.

  > This supersedes the earlier "**APEX live — early access**" note — the *what* changed (AI benefits
  > now require the relevant solution's subscription), the *how* didn't (still deterministic-first,
  > still guardrailed, still behind the private boundary; the boundary is now *also* the entitlement
  > backstop).
- **Trader workbench** — research with scoring, risk validation, paper/simulation with a kill switch,
  journaling, daily review, watchlist, news + economic calendar, deterministic coach, and read-only
  broker connect.
- **Billing live — per-solution** — each solution (the **Planner**, the **Trader**) is **$5/month**
  or **$199 lifetime**, and they're **additive on one subscription and one invoice**: Planner alone is
  **$5/mo**, add the Trader and you're at **$10/mo**. Adding your *first* solution goes through
  **Stripe Checkout**; *additional* solutions are charged **instantly to the card on file, prorated,
  with no checkout**. Removing a solution gives **prorated credit for unused time**; removing your
  *last* one cancels the subscription. Buy **lifetime** while on monthly and the system **automatically
  removes the now-pointless $5 monthly item** so nobody pays twice. Entitlements are still
  **webhook-truth only** — that server-side webhook is the **only** thing that can grant access (never
  a client claim, never a return URL), now hardened against real edge cases: retry / duplicate delivery
  (idempotent), duplicate subscriptions from two browser tabs, a later monthly event overwriting a
  lifetime purchase, and price-ID changes accidentally revoking paying customers. Admin revenue math
  (in **ASCEND Control**) now counts per-solution correctly and **excludes free comps**. Shipped with a
  production-hardening pass (fail-closed access walls, per-user rate limiting, automatic migrations,
  /privacy + /terms, correct per-user time zones), the live **Stripe** setup (2 products, 4 prices, a
  webhook, and the customer portal locked to app-only plan changes), the AI-backend and Trader deploy
  fixes (**trader.ascenddaily.app** is live), and **~40 new automated tests** for billing + gating,
  all green in CI.

  > This supersedes the earlier "**Billing live — Planner Pro ($5/mo) & Lifetime ($199)**" note — the
  > *what* changed (billing is per-solution and additive; there's no single "Planner Pro" product —
  > "Pro" just means a solution's paid tier is active), the *how* didn't (Stripe Checkout for the first
  > solution, entitlements still granted only by the verified server-side webhook).

- **Profile Hub live** — the post-login **/app** home is a real account page and the single self-serve
  place customers manage everything, with four sections: **Your account** (edit name — syncs with
  planner settings — email, sign out), **Your solutions** (one card per product showing a status badge —
  **Free / Pro / Lifetime / Payment issue** — the AI benefits it unlocks, an **Open** button, and
  **buy/manage** buttons), **Billing summary** (total per month, next renewal date, and a **Manage
  payment & invoices** button opening the **Stripe** secure customer portal), and **Account** (change
  password without leaving the page). It still launches the solutions, and every upgrade prompt across
  the platform routes here.
- **Login & signup facelift** — from boxes-inside-boxes to **one clean card**: on desktop, a single
  card next to a **gold-wash brand panel**; on mobile, the form sits **directly on the page with no box
  at all**. Same login, same accounts, same security — a cleaner first impression, applied identically
  to both the main site and the planner login.

  <!-- SCREENSHOTS TO ADD: Profile Hub (Your solutions cards + Billing summary); per-solution billing
       flow; the login/signup facelift (desktop one-card + gold-wash panel, mobile boxless). No image
       files exist yet — do not embed until captured. -->

  > **Cancel-grace:** cancel a sub and its AI keeps working until the end of the period you already paid
  > for, then shuts off within about a minute.
- **Safety posture** — order placement blocked server-side; honest, status-accurate marketing.

## 🔄 In progress

- Deepening **APEX** planning and review quality (still deterministic-first, still no invention).
- Hardening **audit-trail persistence** and telemetry behind the AI boundary.
- Pushing the **mobile** planner toward a publishable beta.
- Expanding **Trader** research and risk tooling within the no-execution boundary.

## ⏭️ Next

- Mobile planner beta (no store links until it's genuinely published).
- Extracting remaining shared UI/tokens to fully remove cross-app drift.

## ⛔ Considered, deliberately gated

- **Any connected trading execution** — gated behind the written safety checklist in
  **[security-and-safety.md](security-and-safety.md)**. Until every item is met, execution stays
  manual and in the user's own broker.

## Principles that won't change

Whatever ships next, these hold:

1. Apps never hold privileged keys.
2. AI never invents your data.
3. Trader never trades for you without explicit, gated, reviewed safety.
4. Marketing only advertises what's real.

> The roadmap is a direction, not a promise. ASCEND would rather ship one honest thing than announce
> ten.
