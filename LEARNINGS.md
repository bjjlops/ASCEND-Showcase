# What I've been learning

> A running engineering journal from building **ASCEND Solutions**. This is the honest part — the
> decisions, the dead ends, and the lessons that actually changed how I work. No code, just what I
> took away.

Building a multi-product platform mostly solo has taught me more than any single feature ever
could. These are the lessons I keep coming back to.

---

## 1. Architecture is mostly about what you *forbid*

The best decision I made was deciding what each part of ASCEND is **not allowed to do.**

Early on it's tempting to let any app reach for whatever it needs — call the model here, hit the
database there. It works, briefly. Then every surface knows a little about everything, and changing
one thing breaks three others.

The fix was to draw hard boundaries and write them down: the planner knows nothing about brokers,
the trader knows nothing about planner logic, the AI layer owns no UI, the marketing site owns no
data. Cross-product coupling became a *defect*, not a convenience.

**Lesson:** good architecture reads like a list of refusals. The boundaries are the design.

---

## 2. The apps you ship should hold the least power possible

The most important security decision was making the client apps boring. They hold public
configuration and nothing else — no model keys, no privileged database credentials.

All the powerful work happens behind a **private boundary** the apps talk to. The apps ask for
intelligence; they never wield it.

The mental test I started applying: *if this build leaked tomorrow, what could an attacker do with
it?* When the answer is "nothing that moves money or reads someone else's data," you've drawn the
boundary in the right place.

**Lesson:** push privilege to the smallest, most-audited place it can live. Containment beats
cleverness.

---

## 3. AI is most trustworthy when it refuses to guess

The hardest and most rewarding part was making the AI layer (APEX) feel *trustworthy* rather than
*impressive.*

What actually built trust:

- **Run deterministic logic first.** Where real product logic exists, use it before ever calling a
  model. The model is a last resort, not a first reflex.
- **Never invent state.** No fabricated records, balances, or trades. "I don't have that" is a
  feature.
- **State assumptions out loud** when the data is incomplete.
- **Redact before prompting**, guard against injection and advice-overreach, and **audit** every
  successful call.

*(APEX is still pre-launch — these are the rules it ships with, not a claim that it already runs
against live models.)*

The counterintuitive part: the constraints made the AI *more* useful, not less. People trust a tool
that knows its limits.

**Lesson:** for AI features, design the refusals as carefully as the capabilities.

---

## 4. Safety is a structure, not a promise

ASCEND Trader does trading *research* — and it does not place trades. The temptation is to treat
that as a setting. I learned to treat it as **structure**: order placement is blocked at the server,
live trading is off by policy, and any future execution path has to clear a written checklist
before it can exist at all.

The difference matters. A promise ("we won't trade for you") depends on no one flipping a flag. A
structure ("the order path returns blocked, and there's a checklist gating any change") holds even
when no one's watching.

**Lesson:** if a safety property matters, make it something the code *can't* casually undo.

---

## 5. The monorepo migration was worth the pain

ASCEND started as separate repositories — one per app and library. It taught me exactly why people
move to monorepos.

The breaking point was sharing a private library across repos. Keeping versions in sync, wiring up
private installs in CI with access tokens, rebuilding artifacts before dependents could use them —
it was constant friction and a recurring source of broken builds.

Consolidating into a **pnpm workspaces + Turborepo** monorepo removed an entire category of
problems: shared code became a workspace package consumed directly, the token gymnastics in CI
disappeared, and Turborepo gave me dependency-ordered builds and caching for free.

**Lesson:** if you're hand-rolling cross-repo linking and token-gated installs, the tooling is
telling you to merge. Listen earlier than I did.

---

## 6. Deployment is where assumptions go to die

I assumed deployment would be the easy part. It was the most educational.

- A hosting approach I expected to "just work" **failed on a newer framework's routing/proxy model.**
  Rather than fight it, I moved that surface to a platform that supported it natively. Knowing when
  to stop fighting a tool is a skill.
- I ended up with a **hybrid**: application surfaces on one cloud, edge/DNS and the private AI
  boundary on another — each used for what it's genuinely best at.
- Making it *feel* like one product across many surfaces came down to a **shared session across
  subdomains** (a parent-domain cookie), so signing in once carries through the ecosystem.

**Lesson:** pick infrastructure for how it behaves under your real constraints, not its marketing.
And budget real time for deploy — it's part of the build, not an afterthought.

---

## 7. The small tooling cuts add up

A scattered but real set of lessons from the day-to-day:

- **Framework conventions change underneath you.** A newer major version moved a routing/proxy
  primitive to a new file convention; renaming it back to the old name silently broke things.
  Read the migration notes, then read them again.
- **Bundlers in a monorepo need to be told where "root" is.** Resolving a sibling workspace package
  meant explicitly pointing the bundler at the workspace root and marking the shared package for
  transpilation. Hours lost before I understood *why*, minutes to fix once I did.
- **Environment loading is a real dependency.** A backend that doesn't auto-load env files will look
  "broken" on startup when it's really just unconfigured. Most of those startup errors were an
  unloaded env file, not a bug.
- **Worktrees and generated folders must be gitignored everywhere.** An un-ignored tooling folder
  committed itself as a stray reference and broke clean clones in CI. One `.gitignore` line per repo
  would have saved a confusing afternoon.

**Lesson:** keep a log of these. Each one feels trivial; together they're most of what "experience"
actually is.

---

## 8. AI agents are a force multiplier — if you give them context

A lot of ASCEND was built working alongside AI coding agents. The thing that made them genuinely
useful wasn't prompting tricks — it was **writing down the project's rules** so an agent could act
safely without me re-explaining everything each time.

Persistent context files — the architecture, the security rules, the engineering standards, the
"read me first" for any agent touching the code — turned the agents from clever autocomplete into
collaborators that respected the boundaries I'd set.

**Lesson:** treat your AI tools like a new teammate. Onboard them. Write the rules down once;
benefit every session after.

---

## 9. Honesty is a product decision

It would be easy to make the marketing site look further along than the product is — store badges
for an unreleased app, a "subscribe now" button that doesn't charge, a confident claim or two.

I decided the opposite: advertise only what's real. Live is "live," in-development is
"in-development," research preview is exactly that. No store links until an app is published. No
checkout charges until billing actually works.

It's slower and less flashy. It's also the thing I'm most sure was right.

**Lesson:** the credibility you build by *not* overselling compounds. Shipping one honest thing
beats announcing ten.

---

## 10. Going live is a test of your constraints, not your features

For a long time APEX was "designed to be safe but switched off." Turning it **on** in production was
the real exam — because the temptation, the moment something works, is to relax the rules that made
it slow.

I didn't. APEX went live still **deterministic-first**, still **guarded** (injection, advice limits,
no-invention, evidence floors), still **kill-switchable**, and still reaching models only through a
single private, server-side gateway. The flip from off to on changed exactly one thing — whether a
model is allowed to run — and nothing about what it's allowed to *do*.

The same held for surfacing Trader as "live": it became genuinely usable (research, paper/sim,
journaling, a coach) without the order endpoint ever becoming un-blocked. Live didn't mean *more
power*; it meant *the same constraints, now in front of real people.*

**Lesson:** the discipline isn't in building the guardrails — it's in keeping them when the feature
finally works. Ship the constraints, not just the capability.

---

## The throughline

Almost every lesson here is the same lesson wearing different clothes: **restraint scales.**
Forbidding the right things, holding the least power, refusing to guess, advertising only what's
real — discipline isn't the constraint on the work. It *is* the work.

— Diego
