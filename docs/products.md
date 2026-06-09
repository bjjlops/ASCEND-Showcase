# Products

A tour of the **ASCEND Solutions** ecosystem. Status labels here match what the public site
advertises — ASCEND markets only what's real today.

| Product | Status |
| --- | --- |
| ASCEND Planner — Web | 🟢 **Live** — free in early access |
| ASCEND Planner — Mobile | 🟡 **In development** — no store links until published |
| ASCEND Trader | 🔵 **Live — research only** (no order execution) |
| APEX (AI layer) | 🟢 **Live — early access**, deterministic-first & guardrailed |
| Hub / marketing site | 🟢 **Live** |
| Billing | ⚪ **Test mode** — no live charges |

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
- **APEX planning support** — ask for a plan, a review of your habits, a day analysis, or a weekly
  summary.
- **Settings & profile** — display name, username, avatar, and a privacy flag.

**Privacy by default** — private profiles stay out of public rankings and feeds.

| Surface | Stack | Status |
| --- | --- | --- |
| **Web** | Next.js (App Router) · React · Tailwind · Supabase | 🟢 **Live** — free in early access |
| **Mobile** | Expo · React Native · Expo Router | 🟡 **In development** — no app-store links until published |

---

## ASCEND Trader

A safety-first workbench for trading **research and risk analysis** — built for people who want to
think before they act. It's now surfaced as **live** in the app launcher, as a research tool — and
it still **never places trades.**

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

**Status:** 🔵 **Live — research only**

---

## APEX — the intelligence layer

APEX is the shared AI layer across ASCEND. It's designed to feel *disciplined, precise, calm, and
useful* — and it's deliberately constrained. **APEX is now live in early access** across the Planner
and Trader, running behind the private AI boundary.

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

- **Planner APEX** — day analysis, weekly summaries, and a chat assistant that schedules tasks,
  reviews habits, explains streaks, and suggests next actions, using your planner data only.
- **Trader APEX** — research summaries, risk review, strategy extraction, and a streaming coach. It
  never claims to give financial advice and never submits orders; it reads research **read-only**.

**Status:** 🟢 **Live — early access.** A feature *inside* the products, not a standalone chatbot.

---

## The hub / marketing site

The public site at [ascenddaily.app](https://ascenddaily.app) is the ecosystem's front door **and**
its identity hub: mission, products, packages, sign-in, and an **/app launcher** that routes you into
the Planner or Trader.

Beyond identity, it's intentionally minimal in what it touches: **no model access, no privileged
database keys, no AI calls.** A checkout path exists but runs in **test mode** — there are no live
charges and no fake "payment success" states until billing is genuinely enabled.

**Status:** 🟢 **Live**

---

## What is *not* a product

Some parts of ASCEND are infrastructure, not products, and are never marketed as such: the private
AI platform, the **internal control plane** (an admin/observability console — accounts, subscriptions,
the AI cost ledger, and an audit log), the shared packages (contracts, auth, tokens, UI, the typed AI
client), and this documentation. They exist so the products can be safe and coherent — but they're
plumbing, and ASCEND is honest about that. See the workspace map in **[tech-stack.md](tech-stack.md)**
and the control-plane write-up in **[architecture.md](architecture.md#the-internal-control-plane)**.
