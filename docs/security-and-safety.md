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
- Secrets stay server-side and are never returned by an API response, logged, or rendered.

If an app build were fully exposed tomorrow, it would reveal nothing that could move money, read
another user's data, or call a model on ASCEND's account.

## Scoped data access

- Planner data is guarded by **row-level security** and accessed per-user, on behalf of the signed-in
  person — not with a master key.
- Trader research is **read-only** to the AI platform. There is no path from the AI layer to orders,
  broker credentials, or live account state.

## AI guardrails

APEX is **in development** and pre-launch (see the [roadmap](roadmap.md)) — it does not yet run
against live models. The points below are how it is **designed** to behave: the guardrails ship with
the AI layer and are gated behind kill switches that stay off until it's ready.

By design, the AI layer is trustworthy by construction:

- **Deterministic-first** — where real product logic exists, it runs *before* a model is ever called.
- **No invention** — APEX is built never to fabricate records, balances, trades, or orders. If it
  doesn't know, it says so.
- **Input redaction** — sensitive text is redacted before it's used to build prompts or context.
- **Guardrails** — checks for prompt injection, for financial-advice overreach, for attribution, and
  for evidence backing the AI's outputs.
- **Audit** — every successful AI call is designed to write an audit record, with request IDs and
  cost tracking.
- **Kill switches** — AI features sit behind flags that default to off.

## Trading safety

ASCEND Trader is a **research and risk-analysis** product. Its safety model is deliberate and strict:

- **No live order placement exists.** The order endpoint returns a blocked response.
- Live trading is **disabled** and order placement is **off** by default and by policy.
- Default mode is **paper / simulation**.
- Broker credentials (where present at all) are used only for readiness and safety checks — never to
  trade — and are never logged, displayed, committed, or pasted anywhere.
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
- Payment flows are scaffolded but disabled — no charges, and no fake "payment success" states,
  until billing is genuinely enabled.
- Product status is stated plainly: live, in development, or research preview.

> The guiding idea: **trust is earned by what the system refuses to do.** ASCEND's safety choices
> are structural, so they hold even when no one is watching.
