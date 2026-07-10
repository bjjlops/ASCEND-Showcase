# Design system

**ASCEND Solutions** should feel **direct, premium, and disciplined** — a command center, not a toy.
The design language is restrained on purpose: it gets out of the way so the work can take focus.

The shipped values below come from the single source of truth, the `@ascend/tokens` package — CSS
custom properties for the web apps, with a hand-synced JS object for native/programmatic use.

## The mark

<img src="../assets/ascend-mark.png" alt="ASCEND Solutions mark" width="92" align="left">

The mark is an upward **"A" peak** — ascent — in a dark-to-gold gradient: a dark base lifting into
gold, a summit reached. Give it room to breathe, keep it on a calm background, and never recolor or
stretch it.

<br clear="left">

## Color

A near-black, green-tinted foundation carries **two accents: gold and teal.** Gold anchors the
**brand and the Planner**; teal anchors the **Trader**. On the same dark ground they make the
ecosystem feel like one system with two moods — disciplined gold, calm teal. Risk and status always
get their own semantic colors — never an accent alone.

### Gold scale

Gold runs as a small scale — brightest in the brand, softer on product surfaces. Same hue family,
different jobs, so the mark and the app never look like two different golds by accident:

| Role | Token | Use |
| --- | --- | --- |
| **Brand gold** | `#E3B341` | The mark, the banner, and marketing highlights — the brightest gold |
| **Gold (bright)** | `#D19D34` | Hover / emphasis on product surfaces |
| **Gold** | `#B9841F` | Primary product accent — progress, focus, achievement (e.g. the Planner) |

### Accents & semantics

| Role | Token | Use |
| --- | --- | --- |
| **Teal** | `#236B5A` | Co-accent — **leads the Trader**; calm, supportive highlights |
| **Success** | `#167455` | Positive / on-track states |
| **Warning** | `#9B650B` | Caution states |
| **Danger** | `#B43F32` | Risk / destructive states |
| **Info blue** | `#275F9F` | Informational states |

### Foundation

| Role | Token | Use |
| --- | --- | --- |
| **Dark bg** | `#0D100E` | Primary app background — near-black, green-tinted |
| **Dark surface** | `#151A15` | Panels, navigation, raised surfaces |
| **Dark text** | `#F4F6EF` | Primary text on dark surfaces |
| **Light bg** | `#F7F9F4` | Marketing / light theme background |
| **Light text** | `#111711` | Primary text on light surfaces |

The system ships **dark** (the command-center theme) and **light** (the marketing theme) from one
token set in `@ascend/tokens`, so every surface stays in agreement.

**Rules**

- Accents are for **precision and focus**, not a full-page wash. Used sparingly, they mean something.
- **Gold leads the Planner and the brand; teal leads the Trader.** The per-surface lead is
  intentional — the pairing is what makes the ecosystem read as one.
- **Risk states get clear, semantic colors and labels** — especially in Trader. Risk is never
  communicated by an accent alone.
- Contrast stays high enough for mobile, desktop, and dense command-center views alike.

## Typography

- **Inter Tight** for the interface — strong, compact headings for command and orientation.
- **JetBrains Mono** for numbers, code, and precise/operational readouts.
- **Compact body copy** in dashboards and operational views, where density helps.
- **Generous line height** for reflective moments — planning, journaling, and guidance.
- No gimmicky display type inside product workflows.

## Motion & shape

- **Calm, quick motion** — short, eased transitions (`cubic-bezier(0.2, 0.8, 0.2, 1)`), nothing
  bouncy or attention-seeking.
- **Soft, modest radii** — rounded enough to feel considered, never playful.

### Auth surfaces

The login and signup pages are where restraint shows first. They used to be boxes-inside-boxes — a
bordered panel wrapping a bordered card wrapping bordered messages and bordered inputs. The facelift
strips that to a single, uncluttered layout: **get out of the way, premium without noise.**

- **Desktop** — exactly **one card** beside a clean **gold-wash** brand panel. The wash draws on
  **Brand gold** `#E3B341`, and stays a panel — not a full-page bath — honoring *accents are for
  precision, not a full-page wash.*
- **Mobile** — the form sits **directly on the page**, no box or card at all.
- Applied **identically** to the main-site login and the planner login, so the first impression is
  one system, not two.

The post-login home, the **Profile Hub**, is the account surface these pages open onto — same
restraint carried past the door.

## Voice

ASCEND sounds **calm, precise, and high-agency.** Copy is concise, specific, and steady.

**Prefer**

- "Plan today" over vague motivation.
- "Risk limit reached" over a dramatic warning.
- The product verbs: **Plan · Focus · Review · Execute · Risk · Limit · Journal · Simulate.**

**Avoid**

- Hype without instruction.
- Any claim of financial certainty.
- Shaming language.
- Any suggestion that AI can guarantee outcomes.

## Naming

- **ASCEND Solutions** — the umbrella brand and ecosystem. Tagline: *Where solutions to people's struggles come to life.*
- **ASCEND Planner** — planning and discipline.
- **ASCEND Web / ASCEND Mobile** — the desktop/tablet and native planner clients.
- **ASCEND Trader** — trading research and risk analysis.
- **APEX** — the shared AI intelligence layer.

## The feeling

Premium without noise. Disciplined without coldness. Every screen should make the next right action
obvious — and never oversell it.
