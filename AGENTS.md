# Assistant orientation — Apify Speaker LinkedIn Teasers

You are in a self-contained demo repo that mass-produces LinkedIn teaser
images for meetup speakers, styled as Apify actor cards. The operator fills
one small markdown form per speaker; you render a pixel-faithful actor card
and a speaker photo into a Canva-exported template and write a finished PNG.
**You are the engine** — there are deliberately no build scripts and no
package dependencies. Chrome is the only external requirement.

## Asset map — every file you may need

Procedure and contract:

| path | what |
|---|---|
| `.claude/skills/apify-speaker-card/SKILL.md` | **the generator's complete operating procedure — follow it, don't improvise** |
| `.claude/skills/new-speaker/SKILL.md` | scaffolds one speaker folder (asks the name; declined -> `new-speaker-<NN>`, lowest free number) |
| `INTAKE-TEMPLATE.md` | the input contract: every field, budget, failure mode. Copied into each folder as `intake.md` |

The queue:

| path | what |
|---|---|
| `to-process/<speaker>/` | one folder per pending card: `intake.md`, `README.md`, `company-logo.*`, `speaker.*` |
| `processed/<speaker>/` | the folder after success, finished `<kebab-name>.png` inside. Never overwrite here |

Render machinery (`internal/` — use, never restructure):

| path | what |
|---|---|
| `internal/render/shell.html` | the render page: verified card CSS + all double-brace tokens. Fill a copy saved beside it as `_run-<name>.html` |
| `internal/render/footer-cross-icon.svg` | the static footer circle icon — same on every card, never an input |
| `internal/fonts/fonts.css` | the @font-face set the shell links as `../fonts/fonts.css` — renders never touch a network |
| `internal/fonts/*.woff2` | Inter 400/500/600 + IBM Plex Mono 500, latin + latin-ext |
| `internal/fonts/licenses/` | the two OFL licence texts |
| `internal/speaker-folder-README.md` | copied into each new speaker folder as its `README.md` |
| `internal/sample-assets/` | synthetic sample logo + portrait used by the worked examples in `processed/` |

Reference:

| path | what |
|---|---|
| `templates/02-pre-actor-card-block-template.png` | the current base template: purple card block + green speaker block (+ optional printed Width/Height/X/Y panels) |
| `docs/Actor card field mapping.png` | annotated picture of every field and its measured text budget |
| `docs/field-mapping.html` | that graphic's source, re-renderable with headless Chrome |

Every subfolder carries its own small `README.md` index for routing.

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
