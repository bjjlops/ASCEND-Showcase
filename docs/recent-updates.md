# Recent updates

Product-facing release notes for **ASCEND Solutions** — what shipped to production and what you
actually get, newest first, with screenshots. For the engineering ship log (including the bugs that
had to be beaten), see **[showcase-log.md](showcase-log.md)**; for the thematic milestone summary,
**[changelog.md](changelog.md)**.

<!--
  HOW TO ADD AN UPDATE (do this on each production push that's worth showing):
  1. Add a new "## YYYY-MM-DD — <headline>" block at the TOP (newest first).
  2. Write 2–4 plain, product-facing sentences: what shipped and what the user gets.
  3. Drop screenshots into ../assets/screenshots/ and embed: ![alt](../assets/screenshots/<file>.png)
  4. Mirror the newest block's summary + hero images into the README "Recent updates" section
     so the repo landing page shows the latest change.
-->

---

## 2026-07-10 — Per-solution billing, the Profile Hub, and AI that unlocks with your plan

**In production now:** billing is **per-solution** and **additive**, there's a real account home
called the **Profile Hub** where you manage everything yourself, and each solution's **AI benefits**
now unlock with that solution's plan. A visitor can sign up free, use the core planner and journaling
at no cost, hit an AI feature, see a clean **"part of Pro — $5/month"** prompt, subscribe in one
checkout, and have the AI unlock within seconds — all self-serve.

**Per-solution pricing — additive, one subscription, one invoice**

- **Each solution is its own paid tier.** The **Planner** and the **Trader** are **$5/month** each,
  or **$199 lifetime** each (pay once for that solution, never again). They add up on **one
  subscription and one invoice** — Planner alone is **$5/mo**; add the Trader and you're at
  **$10/mo**, still one subscription, still one invoice.
- **Adding a solution is instant after the first.** Your **first** solution goes through **Stripe
  Checkout**. Any **additional** solution is charged **instantly to the card on file, prorated, with
  no second checkout**.
- **Removing is fair.** Drop a solution and you get **immediate removal with prorated credit** for
  the unused time. Remove your **last** solution and the subscription **cancels** cleanly.
- **No double-paying.** Buy **lifetime** while you're on monthly and the system **automatically
  removes the now-pointless $5 monthly item** for that solution — you never pay twice for the same
  thing.
- **"Pro" just means paid now.** There's no single "Planner Pro" product anymore; **Pro** simply
  means "this solution's paid tier is active."

**The Profile Hub — your account home at [ascenddaily.app/app](https://ascenddaily.app/app)**

The bare "pick an app" launcher is gone. The Profile Hub is now a real account page — the **single
self-serve place** to subscribe, unsubscribe, fix a failed card, and edit your profile — and it still
launches the solutions (routing preserved). **Every "upgrade" prompt across the platform points
here.** Four sections:

- **Your account.** Name (**editable right there**, and it syncs with your planner settings), email,
  and sign out.
- **Your solutions.** One card per product with a **status badge (Free / Pro / Lifetime / Payment
  issue)**, the **AI benefits that solution unlocks**, an **"Open"** button, and **buy / manage**
  buttons.
- **Billing summary.** Your **total per month**, your **next renewal date**, and a **"Manage payment
  & invoices"** button that opens the **Stripe secure customer portal** — update your card, view
  invoices, or cancel.
- **Account.** **Change your password** without leaving the page.

**AI gating — no subscription, no AI (but never a broken screen)**

Each solution's AI benefits now **require that solution's plan**. The free core stays free; the AI
is the paid part.

- **Planner AI — paid** (needs the Planner plan): **APEX chat**, dashboard AI insights, day analysis,
  weekly summary.
- **Planner — still free:** tasks, habits, planner, reviews, stats.
- **Trader AI — paid** (needs the Trader plan): **AI coach chat**, research summaries, "paste &
  extract", AI risk review.
- **Trader — still free:** manual research, journaling, and the rule-based daily review.
- **Free users see a clean prompt, never a wall.** When you reach a paid AI feature without the plan,
  you get a tidy **"this is part of Pro — $5/month"** card that links to your hub — **never a broken
  screen or a raw error**.
- **Enforced in three layers so it can't be bypassed** — the nice UI prompt inside each app's
  **pages**, the real block inside each app's **server routes**, and a backstop at the **central AI
  backend** every AI request must pass through, so even a bug in an app can't leak free AI. This is
  the existing private AI boundary (the **Nyquest** gateway) now doing double duty as the entitlement
  backstop.
- **Cancel-grace.** Cancel and your AI **keeps working until the end of the period you already paid
  for**, then shuts off within about a minute.

**Login & signup facelift**

The old auth pages were boxes-inside-boxes. Now, on **desktop**, it's exactly **one card** next to a
clean **gold-wash brand panel**; on **mobile**, the form sits **directly on the page with no box at
all**. Same login, same accounts, same security — just a cleaner first impression. Applied identically
to both the **main site** and the **planner** login.

**Earned, never claimed.** Access is still granted **only** by the verified server-side webhook —
never by a client claim or a return URL, no fake "payment success" states. The change adds billing
and gating; it loosens **no** safety constraint. The Trader still never places trades (order placement
stays blocked server-side), and AI never invents your data.

> This supersedes the earlier **"Billing is live: Planner Pro & Lifetime"** note below — the *what*
> changed (pricing is now **per-solution and additive**: **$5/mo** or **$199 lifetime** *per
> solution*, not a single flat "Planner Pro"), the *how* didn't (entitlements are still granted
> **only** by the verified webhook).

<!--
  SCREENSHOTS TO ADD (drop into ../assets/screenshots/ and embed here + in the README):
    • Profile Hub — the four sections in one view                (hub-overview.png)
    • Profile Hub — a solution card with status badge + AI list  (hub-solution-card.png)
    • Billing summary with "Manage payment & invoices"           (hub-billing-summary.png)
    • the "part of Pro — $5/month" AI upgrade prompt             (gating-pro-prompt.png)
    • login facelift — desktop (one card + gold-wash panel)      (login-facelift-desktop.png)
    • login facelift — mobile (boxless form)                     (login-facelift-mobile.png)
-->

---

## 2026-07-02 — Billing is live: Planner Pro & Lifetime

**In production now:** you can upgrade to a paid plan and your premium features unlock the instant
you do. The full path — **checkout → payment → account upgrade** — runs on real infrastructure and
is verified end to end.

- **Two ways to upgrade.** **Planner Pro** at **$5/mo** (subscription) or **Planner Lifetime** at
  **$199 once** (one-time). Stripe Checkout handles payment; your account upgrades **automatically**
  the moment it clears.
- **Self-serve billing.** A **"Manage billing"** card lets you review or change your plan yourself —
  no emailing support.
- **Earned, never claimed.** A paid entitlement is granted **only** by the verified server-side
  webhook — never by a client claim or a return URL. No fake "payment success" states.
- **APEX is part of Planner Pro.** The upgrade prompt makes clear what a plan unlocks before you pay.

Alongside billing, the whole platform got a **production-hardening pass** — the groundwork that makes
a paid launch trustworthy:

- **Secure by default.** Every access wall now **fails closed**, with **per-user rate limiting** and
  no data left exposed.
- **Reproducible & deployable.** Every app is deployable from a clean environment with **automatic
  migrations**.
- **Launch essentials.** **/privacy** and **/terms** pages are live, **per-user time zones** compute
  the correct "today" everywhere, and the waitlist is protected against abuse.

<!--
  SCREENSHOTS TO ADD (drop into ../assets/screenshots/ and embed here + in the README):
    • pricing page with live Subscribe / Get Lifetime buttons  (billing-pricing.png)
    • Stripe Checkout page for Planner Pro                      (billing-checkout.png)
    • "Payment received" confirmation banner                   (billing-confirmation.png)
    • account billing card ("Manage billing")                  (billing-manage.png)
    • the "APEX is part of Planner Pro" upgrade prompt          (billing-apex-upsell.png)
    • /privacy and /terms pages                                 (legal-privacy-terms.png)
-->

---

## 2026-06-08 — APEX is live across ASCEND, and one login

**In production now:** AI is switched **on** across both products, and sign-in is unified — one
account works everywhere.

- **Planner — APEX assistant.** APEX now answers **grounded in your real schedule**: day analysis,
  weekly summaries, and a chat that suggests your next action in plain language (not raw data).
- **Trader — AI coach + analysis.** A reflective **trade coach** (no buy/sell signals, no price
  predictions), plus a research summarizer, risk review, and strategy extraction.
- **One login.** The Trader now shares the Planner's account — **sign in once**, use both.
- **Honestly run.** Everything routes behind the private AI boundary (guardrailed, audited,
  kill-switchable) through a Cloudflare Worker to the **Nyquest** multi-model gateway, with **real
  per-call cost capture.**
- **Still off by design.** Real-money trading stays disabled; order placement is blocked server-side.

<p align="center">
  <img src="../assets/screenshots/planner-apex.png" width="49%" alt="APEX workspace — grounded in your real plan">
  <img src="../assets/screenshots/trader-coach.png" width="49%" alt="Trader trade coach">
</p>
<p align="center"><sub><b>Left:</b> APEX, grounded in your real plan (context chips: Today's plan · This week · Unscheduled). <b>Right:</b> the Trader's reflective coach.</sub></p>

<p align="center">
  <img src="../assets/screenshots/platform-usage.png" width="62%" alt="Nyquest usage — real per-call cost capture">
</p>
<p align="center"><sub>Honestly metered — real per-call cost capture, auto-routing across models (<code>deepseek</code> · <code>claude-haiku</code> · <code>qwen</code>).</sub></p>

*Engineering detail — the unified-identity config, the production bugs found and fixed, and the
full rollout — is in the **[ship log](showcase-log.md)**.*
