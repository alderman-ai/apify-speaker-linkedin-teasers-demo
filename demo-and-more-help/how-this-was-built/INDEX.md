# how-this-was-built/

**How this demo was built, for a technical reader.** This is where line 3
of the assistant's menu lands. Jargon is allowed here; each page defines
its terms where they first appear.

## The build story in ten sentences

The whole pipeline is one markdown form, two images, one static HTML page
and one headless-browser screenshot; nothing in between is a program. The
assistant is the engine: the "skills" are plain markdown procedures in
`_internal/skills/` that any agentic harness can follow by reading them,
routed by `CLAUDE.md` / `AGENTS.md`. The actor card is apify.com's
`ActorStoreItem` rebuilt in plain HTML and CSS and verified box-for-box
against the live site to 0.001px, oddities included, because the oddities
are what make the heights come out right. The backdrop is one machine-built
1200×1200 template whose two placeholder blocks were measured once, as
exact-colour footprints in whole pixels, and frozen as eight constants in
section 6 of the intake form. The intake form is simultaneously the form,
the schema, the budget table and the failure-mode spec; operator values
live in labelled fences, the fences win over the frontmatter, and the
checks are executable by an assistant reading them. Every check halts
rather than degrades: over-budget text, a non-square photo, a block that
cannot hold the card's quantised height. The render is a Chromium already
on the machine, launched headless with fixed flags, drawing local Inter
and IBM Plex Mono with no network and no JavaScript; the screenshot is the
deliverable. A ten-speaker stress test with two cold-reading executors
produced four findings, and each one tightened the contract — the blurb
budget became 115, and the colour-mask geometry rules were deleted in
favour of constants. The git log is the design history: seventeen days
from first cut to tonight. The on-stage version added a page of
pre-built checkpoints promoted on a tick and a skill that fans one dictated
name out to four parallel card builds, with a fallback card rendered in
advance.

## The pages

1. [Architecture](architecture.md) — inputs, stages and outputs; the
   no-scripts decision; the queue's three stages and the `-NN` rule.
2. [The card: a verified CSS reproduction](the-card-css-reproduction.md) —
   the 400 × 153.667 card, the five quantised heights, why the oddities are
   load-bearing, the speaker element and every template token.
3. [The template: one canon image, eight fixed constants](template-geometry-and-constants.md)
   — how the blocks were measured, the per-run ratio check, the versioning
   convention, the starfield recipe, and how the template evolved (with
   the author's working images).
4. [The intake contract and gates](the-intake-contract-and-gates.md) —
   fences, frontmatter, the gate table with its numbers, validation order,
   and each halt's report.
5. [The render step](the-render-step.md) — the exact Chromium command and
   what each flag does, why there is no compositing step, local fonts, zero
   JavaScript, and verify-by-inspection.
6. [Timeline](timeline.md) — the git history in seven phases, the stress
   test's four findings and which one is kept as evidence.
7. [What the on-stage live demo added](the-live-demo.md) — pre-built page
   checkpoints on a tick, a polled feed, and a four-way parallel build with a
   fallback.
8. [Forking it for your own brand](forking-for-your-own-brand.md) — what a
   developer replaces to rebrand, what stays, and the licensing state.

`images/` holds copies of the author's working files used on the template
and timeline pages (template drafts, an early render, a screenshot of an
earlier write-up). Every other fact traces to a file in this repo: the
skills, the render page, the intake template, the core-templates README,
the stress-test report, the code inventory, and `git log`. Nothing here is
read by the pipeline at run time.
