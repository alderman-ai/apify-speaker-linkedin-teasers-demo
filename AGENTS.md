# Assistant orientation — Apify Speaker LinkedIn Teasers

You are in a self-contained demo repo that mass-produces LinkedIn teaser
images for meetup speakers, styled as Apify actor cards. The operator fills
one small markdown form per speaker; you render a pixel-faithful actor card
and a speaker photo into a Canva-exported template and write a finished PNG.
**You are the engine** — there are deliberately no build scripts and no
package dependencies. Chrome is the only external requirement.

## Where everything is

| path | what |
|---|---|
| `.claude/skills/apify-speaker-card/SKILL.md` | **the generator's complete operating procedure — follow it, don't improvise** |
| `.claude/skills/new-speaker/SKILL.md` | scaffolds one speaker folder (asks the name; declined → `new-speaker-<NN>`, lowest free number) |
| `INTAKE-TEMPLATE.md` | the input contract: every field, budget, and failure mode |
| `to-process/` → `processed/` | the queue: one folder per speaker, moves whole on success |
| `templates/` | Canva-exported base PNGs with two coloured placeholder blocks |
| `internal/` | machinery — render shell (verified card CSS), static footer icon, self-hosted fonts, folder-README template. Not operator-facing; don't restructure it |
| `docs/Actor card field mapping.png` | annotated picture of every field on the card |

## The two things operators say

- **"new speaker Jana Novakova"** → the `new-speaker` skill scaffolds
  `to-process/jana-novakova/` (intake form + README; without a name,
  `new-speaker-<NN>` at the lowest free number).
- **"process the queue"** → run every folder in `to-process/` through the
  skill's procedure: validate → read geometry off the template → ratio
  check → render in headless Chrome → verify by inspection → move to
  `processed/`.

## Ground rules (the skill has the full list)

- **The template image is canon.** Placeholder geometry is read from it per
  run — purple block = card, green = speaker photo. Frontmatter geometry
  keys are outputs you write back, never inputs.
- **Halt, don't degrade.** Over-budget description, missing assets (ask:
  resubmit vs placeholder outline), block/card ratio mismatch, geometry
  disagreement, any name collision in `processed/` — each is a stop with a
  clear report, never a silent workaround. Never trim operator text, never
  stretch the card, never overwrite anything.
- **The card CSS is a verified reproduction** of apify.com's ActorStoreItem
  (400 × 153.667 at width 400; heights quantised 113.667 / 121.667 /
  137.667 / 153.667 / 169.667 by description lines). Its oddities are
  load-bearing — do not tidy them.
- **Fonts are local** (Inter + IBM Plex Mono, OFL, in `internal/fonts/`).
  Renders never fetch from a network. **GT Walsheim must never be added to
  this repo** — it is a commercial Grilli Type font; apify.com's headings
  use it, this pipeline does not need it.
- This is an unofficial, non-commercial demo. Apify's name, logo and visual
  design belong to Apify.

`CLAUDE.md` and `AGENTS.md` are identical by design — same orientation
regardless of which assistant harness reads it.
