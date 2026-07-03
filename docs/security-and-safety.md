# Security & safety

Security and safety aren't a feature of ASCEND Solutions; they're constraints the whole system is
built around. This page describes the posture at a high level.

## Credential discipline

The rule is simple and absolute: **the things you run hold the least power possible.**

- Apps hold only public, browser-safe configuration (public keys and the AI platform URL).
- **Model-provider keys** live only in the private platform's server environment — never in an app,
  never in browser-visible variables, never prefixed as public.
- **Privileged database keys** are never placed in app repositories. Privileged access is reserved
  for a narrow, audited slice of platform-only code.
- **Broker credentials**, where present, are server-only and used solely for read-only readiness
  checks. The platform's environment validation actively **refuses to start** if a forbidden broker
  write-credential is present.
- Secrets stay server-side and are never returned by an API response, logged, or rendered.

This is also enforced in **CI**: an **AI-boundary guard** fails the build if a model-provider key,
a privileged database key, or a dangerous client import ever drifts toward an app. If an app build
were fully exposed tomorrow, it would reveal nothing that could move money, read another user's data,
or call a model on ASCEND's account.

## Scoped data access

- Planner data is guarded by **row-level security** and accessed per-user, on behalf of the signed-in
  person — not with a master key.
- Trader research is **read-only** to the AI platform. There is no path from the AI layer to orders,
  broker credentials, or live account state.

## AI guardrails

APEX is **now live in early access** (see the [roadmap](roadmap.md)) — it runs against real models in
production, reached only through **Nyquest**, one private, server-side gateway. Going live didn't loosen the
guardrails; they ship **active**, and every AI feature sits behind a **kill-switch flag** that can
turn it off instantly.

By design, the AI layer is trustworthy by construction:

- **Deterministic-first** — where real product logic exists, it runs *before* a model is ever called.
- **No invention** — APEX is built never to fabricate records, balances, trades, or orders. If it
  doesn't know, it says so. Output validators block fabricated trades and invented numbers.
- **Input redaction** — sensitive text is redacted before it's used to build prompts or context.
- **Guardrails** — checks for prompt injection, for financial-advice overreach, for attribution, for
  an evidence floor (a model can downgrade its confidence, never inflate it), and for no
  profit-guarantee language.
- **Audit + cost** — every AI call now writes a **per-request usage event** (tokens, model, status,
  latency, and **real cost**, keyed by request ID) into a durable ledger — idempotently and
  fire-and-forget, so it never slows or breaks a user response. That ledger feeds an internal cost
  console, so AI spend is **measured, not estimated.**
- **Kill switches + rate limits** — AI features sit behind flags, and the platform rate-limits requests.

## Admin access (the internal control plane)

The internal admin console (**ASCEND Control**) is the most-guarded surface in the system, because
it's the one place with a privileged, wide-angle read. It's gated by **three fail-closed walls**:

1. **Network** — it lives behind **Cloudflare Access**; unauthenticated traffic never reaches it.
2. **Origin re-verify** — the app re-verifies that identity at its own origin and requires a valid
   session. It doesn't trust the edge alone.
3. **Admin allowlist** — the user must also be an admin, enforced by a database-level security-definer
   check over an admin-roles table. Every page and every mutating action re-checks it.

The RLS-bypassing service role is reserved for **aggregate reads** (usage/cost, entitlements,
webhooks, health). Account changes run through audited, self-gating database functions, and **every
privileged action is recorded in an append-only audit log.** The console's assistant is deterministic
and calls no model. The console is internal-only and excluded from search indexing.

## Trading safety

ASCEND Trader is a **research and risk-analysis** product. Its safety model is deliberate, strict,
and structural — not a setting:

- **No live order placement exists.** The order endpoint returns a **blocked** response server-side,
  by policy.
- Live trading is **disabled** and order placement is **off** by default — and the unsafe defaults
  are guarded: a **CI safety check** fails the build if the live-trading or order-placement defaults
  are ever flipped on.
- Default mode is **paper / simulation**, including a paper-runner **kill switch**.
- Broker credentials (where present at all) are used only for **read-only** readiness and safety
  checks — never to trade — and are never logged, displayed, committed, or pasted anywhere.
- **ASCEND Trader is not financial advice.**

### The future-live checklist

Any future move toward connected execution is gated. Before *any* live or broker-connected order
path could exist, it must clear an explicit, written checklist, including:

- An official, reviewed broker SDK/client (no guessed or scraped endpoints).
- Server-only secrets and account-ID masking / token redaction.
- Expanded order-placement tests written *before* implementation.
- A manual approval UX, position and loss limits, full logging, and a kill switch.
- UAT/paper-only proof first, and a separate risk review for anything exotic (options, crypto,
  futures, shorting, margin, event contracts).

Until every item is met, execution stays manual and in the user's own broker.

## Honest-by-default product posture

Safety includes not lying to users:

- The marketing site advertises only what's real. No store links for unpublished apps.
- Billing is **live** (Planner Pro and Lifetime) — and the discipline is structural: a paid
  entitlement is granted **only** by a verified server-side webhook, never by a client claim or a
  return URL, so there are **no fake "payment success" states.** The checkout path stayed in test
  mode until it genuinely worked end to end.
- Product status is stated plainly: live, in development, or research only.

> The guiding idea: **trust is earned by what the system refuses to do.** ASCEND's safety choices are
> structural, so they hold even when no one is watching.
