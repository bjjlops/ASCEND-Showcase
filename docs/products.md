# Products

A tour of the **ASCEND Solutions** ecosystem. Status labels here match what the public site
advertises — ASCEND markets only what's real today.

| Product | Status |
| --- | --- |
| ASCEND Planner — Web | 🟢 **Live** — free core; **Planner** AI is paid |
| ASCEND Planner — Mobile | 🟡 **In development** — no store links until published |
| ASCEND Trader | 🔵 **Live — research only** (no order execution); **Trader** AI is paid |
| APEX (AI layer) | 🟢 **Live — early access**, deterministic-first & guardrailed; each solution's AI requires that solution's subscription |
| Hub / marketing site | 🟢 **Live** |
| Billing | 🟢 **Live** — per-solution: each solution **$5/mo** (additive on one subscription & one invoice) or **$199 lifetime** each |

---

## ASCEND Planner

The discipline and planning product, shared across Web and Mobile. It's the heart of the ecosystem.

**What it does**

- **Tasks & schedule** — plan a day, organize by category, and complete with a clean flow; a
  day/weekly planner with calendar view.
- **Habits & streaks** — build routines and keep streaks honest (a streak day is *earned*, not
  granted — it requires real logged activity).
- **Focus sessions** — log focused time; minutes become points, with sane daily caps.
- **Stats & review** — follow-through %, task completion, and habit metrics across 7-, 14-, and
  30-day ranges, with week-over-week trends and category breakdowns. Leaderboards for those who opt in.
- **Dashboard** — a greeting, your follow-through summary, and the next right action.
- **APEX planning support** *(Planner AI — paid)* — ask for a plan, a review of your habits, a day
  analysis, or a weekly summary. The **Planner** core — tasks, habits, planner, reviews, stats —
  stays **free**; the **Planner** AI (APEX chat, dashboard AI insights, day analysis, weekly
  summary) requires an active **Planner** subscription (**$5/mo** or **$199 lifetime**).
- **Settings & profile** — display name, username, avatar, and a privacy flag.

**Privacy by default** — private profiles stay out of public rankings and feeds.

> This supersedes the earlier "APEX planning support is free to any signed-in user" framing — the
> *what* changed (a **Planner** subscription now unlocks the **Planner** AI), the *how* didn't (the
> planner core is still free, and AI still never invents your data).

| Surface | Stack | Status |
| --- | --- | --- |
| **Web** | Next.js (App Router) · React · Tailwind · Supabase | 🟢 **Live** — free core; **Planner** AI requires a **Planner** subscription (**$5/mo** / **$199 lifetime**) |
| **Mobile** | Expo · React Native · Expo Router | 🟡 **In development** — no app-store links until published |

---

## ASCEND Trader

A safety-first workbench for trading **research and risk analysis** — built for people who want to
think before they act. It's **live** at [trader.ascenddaily.app](https://trader.ascenddaily.app) and
launches from your **Profile Hub**, as a research tool — and it still **never places trades.**

**What it does**

- **Research** — capture and tag research entries, with quality/completeness scoring and AI summaries.
- **Risk review & setup planning** — plan long-only stock/ETF setups and validate them against your
  own risk rules; score setup risk.
- **Paper / simulation** — local backtests across deterministic strategies, a paper-trading session
  runner with a **kill switch**, and parameter sweeps. No real money, no broker orders.
- **Journal & daily review** — keep a trade journal and a daily review workflow.
- **Watchlist, news & economic calendar** — a local watchlist, a news tab, and a "red-folder"
  high-impact economic calendar.
- **Deterministic trading coach** — rule- and behavior-based coaching, with a streaming chat surface.
- **Connect your own broker (read-only)** — optional OAuth/API-key connect for **read-only** account
  readiness checks (Alpaca). Credentials stay server-side and are used only for safety checks —
  never to trade.

> [!IMPORTANT]
> **ASCEND Trader does not place trades.** Order placement is **blocked at the server** — the order
> endpoint returns a blocked response by policy. There is no live, paper-order, margin, crypto,
> options, or short execution. Any execution you choose to make happens manually, in your own broker.
> **ASCEND Trader is not financial advice.**

Any future move toward a connected execution path is gated behind an explicit, written safety
checklist — see **[security-and-safety.md](security-and-safety.md)**.

**Free vs. paid** — the **Trader** core stays **free**: manual research, journaling, and the
rule-based daily review. The **Trader** AI benefits — AI coach chat, research summaries, "paste &
extract", and AI risk review — now require an active **Trader** subscription (**$5/mo** or **$199
lifetime**). Gating an AI feature never loosens a safety constraint: order placement is still blocked
server-side, and AI never invents your data.

**Status:** 🔵 **Live — research only**

---

## APEX — the intelligence layer

APEX is the shared AI layer across ASCEND. It's designed to feel *disciplined, precise, calm, and
useful* — and it's deliberately constrained. **APEX is now live in early access** across the Planner
and Trader, running behind the private AI boundary.

**AI is gated by subscription** — a solution's AI benefits require that solution's paid tier; only a
subset of each app's non-AI features stays free. This is fail-closed discipline, not a paywall grab:
free users never hit broken screens or raw errors — they see a clean **"this is part of Pro —
$5/month"** prompt that links to their **Profile Hub**. Gating is enforced at **three layers** so it
can't be bypassed: the app's **pages** (the UI prompt), the app's **server routes** (the real block),
and the **central AI backend** every request must pass through (the backstop — even a bug in an app
can't leak free AI). This maps onto the existing private AI boundary: the boundary is now **also** the
entitlement backstop. **Cancel-grace:** cancel a subscription and the AI keeps working until the end
of the period you already paid for, then shuts off within about a minute.

> This supersedes the earlier "almost all AI is free to any signed-in user" note — the *what* changed
> (each solution's AI now needs that solution's subscription), the *how* didn't (the boundary,
> deterministic-first logic, guardrails, and kill-switches all still stand).

**How APEX behaves**

- Prefers structured plans over vague encouragement.
- Uses **deterministic product logic *before*** it reaches for a model — the model is a last resort,
  not a first reflex.
- States its assumptions when data is incomplete.
- Shows its limits clearly to keep your trust.
- **Never invents** database state, records, balances, trades, or orders.
- Runs behind **active guardrails** (prompt-injection checks, financial-advice limits, evidence
  floors, no-profit-guarantee and no-fabrication checks) and **kill-switch flags** that can disable
  any AI feature instantly.

**Per-product behavior**

- **Planner APEX** *(requires a Planner subscription)* — day analysis, weekly summaries, dashboard
  AI insights, and a chat assistant that schedules tasks, reviews habits, explains streaks, and
  suggests next actions, using your planner data only.
- **Trader APEX** *(requires a Trader subscription)* — research summaries, risk review, "paste &
  extract", and a streaming coach. It never claims to give financial advice and never submits orders;
  it reads research **read-only**.

**Status:** 🟢 **Live — early access**, gated per-solution. A feature *inside* the products, not a
standalone chatbot.

---

## The hub / marketing site

The public site at [ascenddaily.app](https://ascenddaily.app) is the ecosystem's front door **and**
its identity hub: mission, products, packages, and sign-in. After login, the **Profile Hub** at
**/app** replaces the old bare "pick an app" launcher — it's now a real account page and the **single
self-serve place** to manage everything, with four sections: **Your account** (name, email, sign
out), **Your solutions** (one card per product with a status badge, the AI benefits it unlocks, an
**Open** button, and buy/manage buttons), a **Billing summary** (total per month, next renewal, and a
**"Manage payment & invoices"** button), and **Account** (change password in place). It still launches
the solutions — routing is preserved — and **all "upgrade" prompts across the platform point here.**

> This supersedes the earlier "**/app launcher** that routes you into the Planner or Trader" note —
> the *what* changed (the launcher is now the full **Profile Hub**), the *how* didn't (it still routes
> you into the solutions).

Beyond identity, it's intentionally minimal in what it touches: **no model access, no privileged
database keys, no AI calls.** Checkout is now **live** and billing is **per-solution**: each solution
(the **Planner**, the **Trader**) is **$5/mo** — additive on **one subscription and one invoice**
(Planner alone = **$5/mo**; add the Trader and you're at **$10/mo**) — or **$199 lifetime** each.
Adding your **first** solution runs through **Stripe Checkout**; **additional** solutions are charged
**instantly to the card on file, prorated, with no checkout.** Removing a solution gives **prorated
credit** for unused time. The Hub's **"Manage payment & invoices"** button opens the **Stripe secure
customer portal** — update card, view invoices, cancel — configured so **plan changes happen only
through the app**. A paid entitlement is granted **only** by the verified server-side webhook — never
by a client claim, and never a fake "payment success" state. **/privacy** and **/terms** pages are
live.

**Status:** 🟢 **Live**

---

## What is *not* a product

Some parts of ASCEND are infrastructure, not products, and are never marketed as such: the private
AI platform, the **internal control plane** (an admin/observability console — accounts, subscriptions,
the AI cost ledger, and an audit log), the shared packages (contracts, auth, tokens, UI, the typed AI
client), and this documentation. They exist so the products can be safe and coherent — but they're
plumbing, and ASCEND is honest about that. See the workspace map in **[tech-stack.md](tech-stack.md)**
and the control-plane write-up in **[architecture.md](architecture.md#the-internal-control-plane)**.
