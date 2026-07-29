# VitaSense

A supplement-stack safety checker. Add what you take to a morning/evening
list and get dose-safety, redundancy, and interaction flags — checked
deterministically against a sourced reference dataset. The matching logic
is plain, explainable functions with no AI/LLM call at request time: every
flag traces back to a specific row in the data files, not a model's guess.

**Live:** https://vita-sense-nu.vercel.app

## What it does

- **Dose safety flags** — sums each supplement's AM+PM dose and flags any
  daily total that exceeds its reference upper limit (e.g. 20mg AM + 30mg
  PM zinc = 50mg/day, over the 40mg limit, even though neither entry looks
  alarming alone).
- **Redundancy flags** — catches ingredients already included in a
  combination product also in your stack (e.g. a standalone B12 supplement
  alongside a multivitamin that already contains it).
- **Interaction & timing flags** — pairwise rules for known interactions
  and absorption/timing notes (e.g. zinc and calcium competing for
  absorption, vitamin D and K2 commonly paired).
- **Reference limits table** — upper limit, RDA, and timing guidance for
  everything currently in your stack.

The app is intentionally stateless: visit, check your stack, close the
tab. No accounts, no database, no saved history — nothing you enter is
collected or stored anywhere.

## Data

Every value in `data/supplements.json` and `data/interactions.json` —
upper limits, RDAs, interaction notes — was manually cross-checked against
NIH ODS fact sheets (and NIH NCCIH for items outside ODS's scope, like
CoQ10 and turmeric) before being trusted here. That verification was
treated as the top priority throughout this project, ahead of the UI or
deploy — see each entry's `sourceHint` field for what was checked.

- `data/supplements.json` — 26 supplements with upper limits, RDAs, and
  timing notes.
- `data/interactions.json` — 15 pairwise interaction/timing rules.
- `lib/flagger.js` — pure, deterministic functions matching a stack
  against both files.

## How it was built

Built with Claude Code, working data-first: the two JSON files and
`lib/flagger.js` were treated as the part worth getting right before
anything else, with the app itself (Next.js scaffold, UI, deploy) built
around them once the data held up. See `CLAUDE.md` for the working
conventions this repo follows — including a hard rule that a numeric
value or health claim can never be silently invented or adjusted without
being flagged for manual verification.

## Stack

Next.js 16 (App Router, Turbopack), TypeScript, Tailwind v4. Deployed on
Vercel via GitHub import.

## Legal / disclaimer

Informational only, not medical advice. No accounts, no tracking, no data
collection. No lawyer consult was pursued — this is a non-commercial,
stateless portfolio project, so a plain on-page disclaimer (shown in the
app's header, Flags panel, and footer) covers it. See `LICENSE` for reuse
terms.
