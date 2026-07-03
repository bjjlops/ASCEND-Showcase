# Tech stack & workspace map

A reference for *how* ASCEND Solutions is built. This describes structure and choices, not internals —
there's no source here.

## The stack

| Layer | Choice |
| --- | --- |
| **Language** | TypeScript (strict) end to end |
| **Web apps** | Next.js (App Router) · React · Tailwind CSS |
| **Mobile** | Expo · React Native · Expo Router |
| **AI platform** | A lightweight server that runs as a **Cloudflare Worker** in production |
| **Auth & data** | Supabase — Auth, Postgres, and **row-level security** |
| **Trader data** | Prisma · Postgres |
| **Contracts** | Zod schemas shared across every surface |
| **AI access** | **Nyquest** — a single private, server-side multi-model gateway |
| **Payments** | Stripe Checkout — live (Planner Pro subscription + Lifetime one-time), webhook-driven entitlements |
| **Monorepo** | pnpm workspaces + Turborepo |
| **Deployment** | Hybrid — application surfaces on one cloud, edge/DNS + the AI Worker on another |
| **CI** | Whole-graph typecheck/build/test + AI-boundary guard + trader safety check |

## Apps

| App | Role | Status |
| --- | --- | --- |
| **services** | The hub: marketing site **and** identity (sign-in + /app launcher) | 🟢 Live |
| **web-planner** | The web planner — tasks, habits, streaks, focus, stats, APEX | 🟢 Live |
| **trader** | Research & risk workbench — research, paper/sim, journal, coach (no execution) | 🔵 Live, research only |
| **ai-platform** | The private AI boundary — guardrails, audit, the only thing with model access | 🟢 Live |
| **control** | Internal admin command center — accounts, subscriptions, AI cost ledger, audit (not customer-facing) | 🟢 Internal |
| **mobile** | Native planner (Expo) | 🟡 In development |

## Shared packages

These are consumed directly as `workspace:*` packages — no cross-repo linking, no token-gated installs.

| Package | What it owns |
| --- | --- |
| **`@ascend/shared-contracts`** | Zod schemas, stable error codes, and route definitions. **Definitions only** — no business logic, model clients, or DB clients. |
| **`@ascend/ai-client`** | The one typed client the apps use to call the AI platform. Carries the user's session token; holds **no keys**. |
| **`@ascend/auth`** | Supabase SSR helpers — server/browser clients, session refresh, and the parent-domain SSO session. Never holds a service-role key. |
| **`@ascend/ui-web`** | Shared React UI primitives (Badge, Button, Card, SkipLink) driven by design tokens. |
| **`@ascend/tokens`** | Design tokens — CSS custom properties (source of truth) + a synced JS object for native. |
| **`@ascend/config`** | Shared build presets — Next.js wrapper, TypeScript base, PostCSS/Tailwind. |
| **`@ascend/entitlements`** | Pure, zero-dependency feature-gating model (products, tiers, status). |
| **`@ascend/billing`** | Stripe plan catalog + browser checkout/portal helpers. Server validates every entitlement. |

## Why these choices

- **One language, strict.** TypeScript everywhere means a shared shape is a compile-time contract,
  not a hope.
- **A monorepo.** Shared code is consumed directly; Turborepo gives dependency-ordered builds and
  caching, and an entire class of cross-repo linking problems disappears.
- **A Worker for the AI boundary.** The one component that holds power runs at the edge, isolated
  from the apps, behind its own domain and CORS allow-list.
- **Supabase + RLS.** Per-user data scoping is enforced at the database, not just in app code.
- **A single AI gateway (Nyquest).** One server-side path to models keeps keys, guardrails, and cost
  tracking in exactly one auditable place.

See the diagrams in **[architecture.md](architecture.md)** for how these fit together.
