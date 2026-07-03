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
