# Roadmap & status

A high-level, honest view of where **ASCEND Solutions** is and where it's heading. Dates are
intentionally omitted — this is a solo-led project, and the order matters more than the calendar.
For the log of what *recently* changed, see **[changelog.md](changelog.md)**.

## Status at a glance

| Surface / capability | Status |
| --- | --- |
| Web Planner | 🟢 Live — free early access |
| Hub / marketing site | 🟢 Live |
| APEX (AI layer) | 🟢 Live — early access, deterministic-first & guardrailed |
| Trader | 🔵 Live — research only (no execution) |
| Mobile Planner | 🟡 In development |
| Billing | ⚪ Test mode — no live charges |
| Connected trade execution | ⛔ Gated behind a written safety checklist |

## ✅ Done

- **Platform foundations** — one pnpm + Turborepo monorepo, a shared package layer
  (contracts, auth, tokens, UI, typed AI client, entitlements, billing), and enforced security
  boundaries.
- **Hub-and-spoke identity** — sign in once at the hub; an /app launcher and a shared parent-domain
  session route you across the ecosystem.
- **Hybrid deployment + CI** — application surfaces on one cloud, the private AI platform as a Worker
  on another; CI runs the whole graph plus an AI-boundary guard and a trader safety check.
- **APEX live** — planner day analysis, weekly summaries, and chat; trader research summaries, risk
  review, strategy extraction, and a streaming coach — all behind the private boundary, guardrailed,
  and kill-switchable.
- **Trader workbench** — research with scoring, risk validation, paper/simulation with a kill switch,
  journaling, daily review, watchlist, news + economic calendar, deterministic coach, and read-only
  broker connect.
- **Safety posture** — order placement blocked server-side; honest, status-accurate marketing.

## 🔄 In progress

- Deepening **APEX** planning and review quality (still deterministic-first, still no invention).
- Hardening **audit-trail persistence** and telemetry behind the AI boundary.
- Pushing the **mobile** planner toward a publishable beta.
- Expanding **Trader** research and risk tooling within the no-execution boundary.

## ⏭️ Next

- Mobile planner beta (no store links until it's genuinely published).
- Extracting remaining shared UI/tokens to fully remove cross-app drift.
- Turning **billing** from test mode to live — only when there's something worth charging for and the
  flow is genuinely complete.

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
