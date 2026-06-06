# Architecture (conceptual)

> This is a high-level, conceptual description of how ASCEND is put together. It is intentionally
> light on internals — there is no source code, no API contract, and no infrastructure detail here.
> The point is to show *how the system thinks*, not how to rebuild it.

## The core principle: a private AI boundary

The single most important architectural decision in ASCEND is that **the apps you run are not
trusted with privileged power.**

- The apps (Web Planner, Mobile Planner, Trader) hold only public, browser-safe configuration.
- They **never** hold model-provider keys, and they **never** hold privileged database credentials.
- All AI work happens behind a **private platform boundary**. The apps ask the platform for
  intelligence; they don't reach for models themselves.

This keeps the blast radius of any one app small. A leaked client build leaks nothing that matters.

```mermaid
flowchart TB
  subgraph Apps["Apps (public config only)"]
    Web["Planner — Web"]
    Mobile["Planner — Mobile"]
    Trader["Trader"]
  end

  subgraph Boundary["Private AI platform"]
    Routes["AI routes (validated)"]
    Guards["Guardrails + redaction"]
    Audit["Audit log"]
  end

  subgraph Data["Scoped data"]
    Planner[("Planner DB — RLS + scoped access")]
    Research[("Trader research — read-only")]
  end

  Web -- "user session token" --> Routes
  Mobile -- "user session token" --> Routes
  Trader -. "future bridge" .-> Routes
  Routes --> Guards --> Audit
  Routes -- "scoped, per-user" --> Planner
  Routes -- "read-only" --> Research
```

## Contract-driven surfaces

Every surface agrees on the same shapes through a shared **contracts** library — schemas, types,
and stable error codes. The contracts library is the dictionary the whole system speaks; it holds
*definitions*, never business logic, model clients, or database clients.

The payoff: a change to a shared shape shows up as a type error everywhere it matters, before it
ships. The apps and the platform can't quietly drift out of agreement.

## Clear ownership boundaries

ASCEND is organized so each part owns one thing well, and is explicitly forbidden from owning
others:

- **Planner** owns planning logic and planner data. It knows nothing about brokers or trading.
- **Trader** owns research, risk, and trading-safety tooling. It knows nothing about planner logic.
- **The AI platform** owns intelligence, guardrails, and audit. It owns no UI.
- **The marketing site** owns presentation. It owns no data, no auth, and no secrets.

These boundaries are written down and enforced — cross-product coupling is treated as a defect,
not a shortcut.

## Data access is scoped, not broad

- Planner data is protected with **row-level security** and per-user, scoped access — the platform
  acts on behalf of a specific signed-in user, not as a superuser.
- Trader research is exposed to the platform as **read-only**. The platform can read research; it
  cannot touch orders, broker credentials, or live account state.
- Privileged database access is reserved for a narrow, audited slice of platform-only code.

## Built as a monorepo

The apps and shared packages live together in a single **pnpm workspaces + Turborepo** monorepo:

- Shared packages (`contracts`, UI, auth, config, …) are consumed as workspace packages — no
  fragile cross-repo linking, no token-gated private installs in CI.
- Turborepo runs builds in dependency order and caches aggressively.
- Shared config presets (TypeScript, lint, Tailwind) keep every surface consistent.

## Deployed as a hybrid

ASCEND runs across two clouds, each used for what it's best at — application surfaces on one
platform, edge/DNS and the AI boundary on another. A shared session works across subdomains so the
ecosystem feels like one product even though it's many surfaces.

The details of *why* it's split this way — including a deployment approach that didn't work and
what replaced it — are in **[../LEARNINGS.md](../LEARNINGS.md)**.

## What this architecture buys

- **Containment** — apps can't leak what they never held.
- **Trust** — AI behavior is validated, guarded, redacted, and audited at one boundary.
- **Coherence** — shared contracts and enforced ownership stop drift.
- **Honesty** — safety constraints (no live trading, no privileged keys in apps) are structural,
  not promises.
