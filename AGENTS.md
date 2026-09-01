# Assistant orientation — Apify Speaker LinkedIn Teasers

You are in a self-contained demo repo that mass-produces LinkedIn teaser
images for meetup speakers, styled as Apify actor cards. The operator fills
one small markdown form per speaker; you render a pixel-faithful actor card
and a card-style speaker portrait element into the canon template and
deliver a finished PNG. **You are the engine** — there are deliberately no
build scripts and no package dependencies. The only external requirement is
a Chromium-based browser for headless rendering — Chrome, or the Edge
preinstalled on Windows; never download one.

## Asset map — every file you may need

Procedure and contract (`_internal/`):

| path | what |
|---|---|
| `_internal/skills/apify-speaker-card.md` | **the generator's complete operating procedure — read it in full before processing anything; follow it, don't improvise.** Mass-produces Apify-styled speaker teaser images: renders a pixel-faithful Apify actor card (repurposed as a speaker card) and a card-style speaker portrait element into the canon template, one finished PNG per speaker. Use when the operator says "process the intake forms", "process the queue", "generate the speaker cards", "new speaker <name>", or drops folders into `to-process/` |
| `_internal/skills/new-speaker.md` | **the scaffolder's complete procedure — read it in full before scaffolding.** Adds a new speaker folder to the queue — the single place to drop all the visual assets needed to generate that speaker's teaser image (asks the name; declined → `new-speaker-<NN>`, lowest free number; a repeat name → `<name>-<NN>`, first dupe = 01). Use when the operator says "new speaker", "/new-speaker", "add a speaker", "scaffold a speaker folder", or names a person to add to the lineup |
| `_internal/core-templates-please-dont-touch/intake-template.md` | the input contract: every field, budget, failure mode; everything the operator types lives in labelled body fences. Versioned in its frontmatter (`version` / `versioned_at`). Copied into each folder as `intake.md` |
| `demo-and-more-help/intake-template-completed-example.md` | the same template with every fence and frontmatter value filled in — what "done" looks like |

The queue:

| path | what |
|---|---|
| `to-process/<speaker>/` | one folder per pending card: `intake.md`, `README.md`, `company-logo.*`, `speaker.*` |
| `processed/<speaker>/` | the folder after success (archive — form + assets). Never overwrite here — a taken name gets `-<NN>`. Archives are historical: intakes from before the 2026-09-01 tree reorganisation reference old paths; never take current paths from an archive |
| `generated-images/<speaker>-final.png` | the finished render, one PNG per speaker. Never overwrite here — a taken name gets `-<NN>` |

Render machinery (`_internal/` — use, never restructure):

| path | what |
|---|---|
| `_internal/render/shell.html` | the render page: verified card CSS, the `.SpeakerCard` element, all double-brace tokens. Fill a copy saved beside it as `_run-<name>.html` |
| `_internal/render/footer-help-icon.svg` | the static footer circle icon (orange `?`; its 1px orange ring is CSS in the shell) — same on every card, never an input |
| `_internal/render/particles.svg` | apify.com's particle pattern — the starfield's source; rebuild recipe in the core-templates README |
| `_internal/fonts/fonts.css` | the @font-face set the shell links as `../fonts/fonts.css` — renders never touch a network |
| `_internal/fonts/*.woff2` | Inter 400/500/600 + IBM Plex Mono 500, latin + latin-ext |
| `_internal/fonts/licenses/` | the two OFL licence texts |
| `_internal/speaker-folder-README.md` | copied into each new speaker folder as its `README.md` |
| `_internal/sample-assets/` | synthetic sample logo + portrait used by the retired fictional worked examples in `demo-and-more-help/fictional-speakers-as-demo/` |
| `_internal/core-templates-please-dont-touch/speaker-teaser-linkedin_v3.png` | the one canon template (1200×1200, currently v3), **machine-built**: baked gradient starfield + purple card block + green speaker block. Supersedes the operator's original Canva export |

Reference (`demo-and-more-help/` — the operator-facing help and showcase folder):

| path | what |
|---|---|
| `demo-and-more-help/Actor card field mapping.png` | annotated picture of every field and where it lands on the card |
| `demo-and-more-help/Actor card text budgets.png` | the character-budget table for every text field |
| `demo-and-more-help/field-mapping.html`, `demo-and-more-help/text-budgets.html` | those graphics' sources, re-renderable with headless Chrome |
| `demo-and-more-help/scripts-and-security.md` | the plain-English inventory of every piece of code in this repo and when it runs. **If any change adds code or scripts anywhere in the repo, this file must be updated in the same change — keep it truthful** |
| `demo-and-more-help/pipeline-evaluation.md` | the 2026-09-01 ten-speaker stress test of the docs and pipeline |
| `demo-and-more-help/fictional-speakers-as-demo/` | the fictional demo speakers (folklore characters + synthetic personas), archived out of the live queue: `fictional-processed/` folders + `fictional-generated-images/` PNGs |
| `demo-and-more-help/mid-project-snapshots/` | the simulated-"real" stress-test speakers (real Czech public figures), kept as mid-project snapshots: `processed/` + `generated-images/` |

Every folder carries its own `INDEX.md` for routing. `README.md` is
reserved for orientation: the root README (the human entry point), the
core-templates README (a warning plus the template recipe), and each
speaker folder's README (its checklist).

## What operators say — routing requests

The `_internal/skills/` files are plain markdown procedures, **not deployed
harness skills — nothing auto-loads them**. You route by intent and read
the matching skill file in full before acting. The trigger phrases here and
in the skill descriptions are examples, not a grammar: operators paraphrase
freely, and many have never seen this repo.

- **Scaffold intent** — "new speaker Alex Alderman", "add a speaker",
  "put Alex on the lineup", "set up a folder for our next speaker" → read
  `_internal/skills/new-speaker.md`, then scaffold `to-process/alex-alderman/`
  (intake form + README; without a name, `new-speaker-<NN>` at the lowest
  free number).
- **Generate intent** — "process the queue", "process the intake forms",
  "generate the speaker cards", "run the pipeline", "render the pending
  ones" → read `_internal/skills/apify-speaker-card.md`, then run every
  folder in `to-process/` (or the ones named) through its procedure:
  validate → read geometry off the template → ratio check → render in a
  headless Chromium browser → verify by inspection → PNG to
  `generated-images/<speaker>-final.png`, folder to `processed/`.
- **Ambiguous ask** — "make me an image", "create a teaser", "I need a
  card for LinkedIn", and similar requests that name neither workflow:
  check `to-process/` first. If candidates are waiting, name them and ask
  one question — process these now, or start a new speaker? If the queue
  is empty, take it as scaffold intent (e.g. *"Sure! First, what's the
  speaker's name?"*).
- **Template-change intent** — "update the template", "change the intake
  form", "new visual template", "edit the canon template", or any request
  that would modify a file in `core-templates-please-dont-touch/`: the
  core templates are **locked**. If a maintainer-local update procedure is
  present in `_internal/skills/` beyond the two tracked skills (it ships
  only on the maintainer's machine, not in this repo), read it in full and
  follow it. If it is absent, decline the change — offer instead a local
  fork in `_internal/local-forks/` (gitignored, never committed): copy the
  template there with a `.fork.` name marker, apply the changes to the
  copy, and point that speaker's `base_image` at it. Canon stays locked
  either way.
- **Lost human** — "what is this?", "how does this work?": orient them
  from the root `README.md` and point them at `demo-and-more-help/`.

## Ground rules (the skill has the full list)

- **The template image is canon.** Placeholder geometry is read from it per
  run — purple block = actor card, green block = speaker element.
  Frontmatter geometry keys are outputs you write back, never inputs.
- **Core templates are locked; git is the reference.** Both skills diff
  `core-templates-please-dont-touch/` against git HEAD before using it
  and **auto-restore any drift** (`git restore`), telling the operator
  what was reverted and tagging that session's commits `MISMATCH` in the
  subject. Never edit those files, never commit a change to them, and
  never bypass or weaken the auto-restore. **At the start of work in this
  repo, run `git log --oneline -20 --grep=MISMATCH`; on any hit, alert
  the operator** that a drift event occurred. Template changes are made
  only by the maintainer, from their machine, through the local procedure
  described under routing — its commits move the baseline, which is why a
  sanctioned change is never reverted.
- **Halt, don't degrade.** Over-budget description, missing assets (ask:
  resubmit vs placeholder outline), an off-spec speaker photo (must be an
  exact square PNG/JPG/JPEG ≤800×800 — you scale it to the slot, you never
  crop or reframe it), block/card ratio mismatch, geometry disagreement —
  each is a stop with a clear report, never a silent workaround. Never
  trim operator text, never stretch the card, never overwrite anything.
- **Duplicate names suffix, never block.** A repeat name is legitimate (a
  rebuilt card, a fresh start after text edits, a namesake): wherever the
  name is already taken — scaffolding into `to-process/`, or delivering
  into `processed/` / `generated-images/` — use `<name>-<NN>`, the lowest
  free number, zero-padded, **first dupe = 01** (same NN on the archive
  folder and the PNG). Detection is by folder/file names only; a repeated
  frontmatter `speaker_name` breaks nothing and is never consulted.
- **The card CSS is a verified reproduction** of apify.com's ActorStoreItem
  (400 × 153.667 at width 400; heights quantised 113.667 / 121.667 /
  137.667 / 153.667 / 169.667 by description lines). Its oddities are
  load-bearing — do not tidy them. The speaker element (`.SpeakerCard`)
  renders 1:1 with fixed chrome and the fixed `Join me in PRAGUE` copy;
  only its photo changes per speaker.
- **Fonts are local** (Inter + IBM Plex Mono, OFL, in `_internal/fonts/`).
  Renders never fetch from a network. **GT Walsheim must never be added to
  this repo** — it is a commercial Grilli Type font; apify.com's headings
  use it, this pipeline does not need it.
- This is an unofficial, non-commercial demo. Apify's name, logo and visual
  design belong to Apify.

`CLAUDE.md` and `AGENTS.md` are identical by design — same orientation
regardless of which assistant harness reads it.
