# Products

A tour of the ASCEND ecosystem. Status labels here match what the public site advertises — ASCEND
markets only what's real today.

---

## ASCEND Planner

The discipline and planning product, shared across Web and Mobile. It's the heart of the ecosystem.

**What it does**

- **Tasks & schedule** — plan a day, organize by category, and complete with a clean flow.
- **Habits & streaks** — build routines and keep streaks honest (a streak day is *earned*, not
  granted — it requires real logged focus time).
- **Focus sessions** — log focused time; minutes become points, with sane daily caps.
- **Stats & review** — see progress by day, week, and month, with leaderboards for those who opt in.
- **APEX planning support** — ask for a plan, a review of your habits, or a next action.

**Privacy by default** — private profiles stay out of public rankings and feeds.

| Surface | Stack | Status |
| --- | --- | --- |
| **Web** | Next.js App Router · React · Tailwind | 🟢 **Live** — free in early access |
| **Mobile** | Expo · React Native · Expo Router | 🟡 **In development** — no app-store links until published |

---

## ASCEND Trader

A safety-first workbench for trading **research and risk analysis** — built for people who want to
think before they act.

**What it does**

- Research setups and score their quality and completeness.
- Plan long-only stock/ETF setups and validate them against your own risk rules.
- Run local paper/simulation tooling.
- Calculate setup risk and keep a trade journal.
- Review readiness with masked, never-exposed credential status.

> [!IMPORTANT]
> **ASCEND Trader does not place trades.** Order placement is blocked at the server. There is no
> live, paper-order, margin, crypto, options, or short execution. Any execution you choose to make
> happens manually, in your own broker. **ASCEND Trader is not financial advice.**

Any future move toward a connected execution path is gated behind an explicit, written safety
checklist — see **[security-and-safety.md](security-and-safety.md)**.

**Status:** 🔵 **Research preview**

---

## APEX — the intelligence layer

APEX is the shared AI layer across ASCEND. It's designed to feel *disciplined, precise, calm, and
useful* — and it's deliberately constrained.

**How APEX behaves**

- Prefers structured plans over vague encouragement.
- Uses deterministic product logic *before* it reaches for a model.
- States its assumptions when data is incomplete.
- Shows its limits clearly to keep your trust.
- **Never invents** database state, records, balances, trades, or orders.

**Per-product behavior**

- **Planner APEX** helps schedule tasks, review habits, explain streaks, and suggest next actions —
  using your planner data only.
- **Trader APEX** analyzes journal entries, risk, setups, and simulations. It never claims to give
  financial advice and never submits orders.

**Status:** 🟡 **In development** — a feature inside the products, not a standalone product.

---

## The marketing site

The public site at [ascenddaily.app](https://ascenddaily.app) is the ecosystem's front door:
mission, products, packages, and CTAs that link out to the apps.

It is intentionally minimal in what it touches: **no user data, no authentication, no database
access, no AI calls, and no secrets.** Payment flows are scaffolded but disabled — there are no
checkout charges and no fake "success" states until billing is genuinely enabled.

**Status:** 🟢 **Live**

---

## What is *not* a product

Some parts of ASCEND are infrastructure, not products, and are never marketed as such: the private
AI platform, the shared contracts library, and this documentation. They exist so the products can
be safe and coherent — but they're plumbing, and ASCEND is honest about that.
