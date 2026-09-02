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
| `_internal/skills/novice-walkthrough.md` | **the guided first-run procedure — read it in full before walking anyone through.** A wrapper for people who have never used GitHub, a terminal, or an AI coding tool: silent preflight (both skills read, drift check, browser detection), folder tour, then one question at a time through scaffolding, the form, the two images and the render — delegating to `new-speaker.md` and `apify-speaker-card.md` at the right moments rather than re-implementing them. Use when the operator says "Walk me through making my first speaker card", "I'm new to this", "hold my hand", "guide me step by step", or otherwise sounds like they have never seen this repo |
| `_internal/skills/apify-speaker-card.md` | **the generator's complete operating procedure — read it in full before processing anything; follow it, don't improvise.** Mass-produces Apify-styled speaker teaser images: renders a pixel-faithful Apify actor card (repurposed as a speaker card) and a card-style speaker portrait element into the canon template, one finished PNG per speaker. Use when the operator says "process the intake forms", "process the queue", "generate the speaker cards", "new speaker <name>", or drops folders into `01 To Process/` |
| `_internal/skills/new-speaker.md` | **the scaffolder's complete procedure — read it in full before scaffolding.** Adds a new speaker folder to the queue — the single place to drop all the visual assets needed to generate that speaker's teaser image (asks the name; declined → `new-speaker-<NN>`, lowest free number; a repeat name → `<name>-<NN>`, first dupe = 01). Use when the operator says "new speaker", "/new-speaker", "add a speaker", "scaffold a speaker folder", or names a person to add to the lineup |
| `_internal/core-templates-please-dont-touch/intake-template.md` | the input contract: every field, budget, failure mode; everything the operator types lives in labelled body fences. Versioned in its frontmatter (`version` / `versioned_at`). Copied into each folder as `intake.md` |
| `04 Demo and More Help/Filling in the Form/intake-template-completed-example.md` | the same template with every fence and frontmatter value filled in — what "done" looks like |

The queue:

| path | what |
|---|---|
| `01 To Process/<speaker>/` | one folder per pending card: `intake.md`, `README.md`, `company-logo.*`, `speaker.*` |
| `02 Processed/<speaker>/` | the folder after success (archive — form + assets). Never overwrite here — a taken name gets `-<NN>`. Archives are historical: every archived intake predates the 2026-09-01 tree reorganisation and the 2026-09-02 folder renaming, so it still uses the old un-numbered folder names; never take current paths from an archive |
| `03 Generated Images/<speaker>-final.png` | the finished render, one PNG per speaker. Never overwrite here — a taken name gets `-<NN>` |

Render machinery (`_internal/` — use, never restructure):

| path | what |
|---|---|
| `_internal/render/shell.html` | the render page: verified card CSS, the `.SpeakerCard` element, all double-brace tokens. Fill a copy saved beside it as `_run-<name>.html` |
| `_internal/render/footer-help-icon.svg` | the static footer circle icon (orange `?`; its 1px orange ring is CSS in the shell) — same on every card, never an input |
| `_internal/render/particles.svg` | apify.com's particle pattern — the starfield's source; rebuild recipe in the core-templates README |
| `_internal/fonts/fonts.css` | the @font-face set the shell links as `../fonts/fonts.css` — renders never touch a network |
| `_internal/fonts/*.woff2` | Inter 400/500/600 + IBM Plex Mono 500, latin + latin-ext |
| `_internal/fonts/licenses/` | the two OFL licence texts |
| `_internal/fonts/jic/` | byte-identical reference copies of the two core templates (`BU_intake-template.md`, `BU_speaker-teaser-linkedin_v3.png`) — the drift check falls back to these when there is no `.git` |
| `_internal/speaker-folder-README.md` | copied into each new speaker folder as its `README.md` |
| `_internal/sample-assets/` | synthetic sample logo + portrait used by the retired worked examples archived under `04 Demo and More Help/Example Speakers/` |
| `_internal/core-templates-please-dont-touch/speaker-teaser-linkedin_v3.png` | the one canon template (1200×1200, currently v3), **machine-built**: baked gradient starfield + purple card block + green speaker block. Supersedes the operator's original Canva export |

Reference (`04 Demo and More Help/` — the operator-facing help and showcase
folder; its root holds only `INDEX.md` and `README.md`, everything else
sits in `Getting Started/`, `Filling in the Form/`, `Example Speakers/` or
`About This Project/`):

| path | what |
|---|---|
| `04 Demo and More Help/Getting Started/` | the novice entrance: a linear human page for people who have never used GitHub or an AI coding tool, paired with the novice-walkthrough skill |
| `04 Demo and More Help/Filling in the Form/Actor card field mapping.png` | annotated picture of every field and where it lands on the card |
| `04 Demo and More Help/Filling in the Form/Actor card text budgets.png` | the character-budget table for every text field |
| `04 Demo and More Help/Filling in the Form/field-mapping.html`, `04 Demo and More Help/Filling in the Form/text-budgets.html` | those graphics' sources, re-renderable with headless Chrome |
| `04 Demo and More Help/About This Project/scripts-and-security.md` | the plain-English inventory of every piece of code in this repo and when it runs. **If any change adds code or scripts anywhere in the repo, this file must be updated in the same change — keep it truthful** |
| `04 Demo and More Help/About This Project/pipeline-evaluation.md` | the 2026-09-01 ten-speaker stress test of the docs and pipeline |
| `04 Demo and More Help/Example Speakers/Fictional Characters/` | the five folklore demo speakers, archived out of the live queue: `Processed/` folders + `Generated Images/` PNGs |
| `04 Demo and More Help/Example Speakers/Mid-Project Snapshots/` | seven records of the project mid-flight: the five simulated-"real" stress-test speakers (real Czech public figures) plus the two early synthetic personas (`jana-novakova`, `petr-svoboda`) made before the pipeline was finished — `Processed/` + `Generated Images/` |

Every folder carries its own `INDEX.md` for routing, with three exceptions:
the repo root and `_internal/core-templates-please-dont-touch/`, whose
`README.md` serves that role, and `_internal/fonts/licenses/`, which holds
only the two licence texts. `README.md` is
reserved for orientation: the root README (the human entry point), the
core-templates README (a warning plus the template recipe), the
`04 Demo and More Help/` README (help-folder orientation, pointing
assistants at its INDEX), and each speaker folder's README (its
checklist).

## What operators say — routing requests

The `_internal/skills/` files are plain markdown procedures, **not deployed
harness skills — nothing auto-loads them**. You route by intent and read
the matching skill file in full before acting. The trigger phrases here and
in the skill descriptions are examples, not a grammar: operators paraphrase
freely, and many have never seen this repo.

- **Scaffold intent** — "new speaker Alex Alderman", "add a speaker",
  "put Alex on the lineup", "set up a folder for our next speaker" → read
  `_internal/skills/new-speaker.md`, then scaffold `01 To Process/alex-alderman/`
  (intake form + README; without a name, `new-speaker-<NN>` at the lowest
  free number).
- **Generate intent** — "process the queue", "process the intake forms",
  "generate the speaker cards", "run the pipeline", "render the pending
  ones" → read `_internal/skills/apify-speaker-card.md`, then run every
  folder in `01 To Process/` (or the ones named) through its procedure:
  validate → read geometry off the template → ratio check → render in a
  headless Chromium browser → verify by inspection → PNG to
  `03 Generated Images/<speaker>-final.png`, folder to `02 Processed/`.
- **Walkthrough intent** — "Walk me through making my first speaker card"
  (the exact sentence the getting-started page tells them to paste),
  "walk me through", "I'm new to this", "I've never done this before",
  "hold my hand", "guide me step by step", "first time", "start the
  walkthrough" → read `_internal/skills/novice-walkthrough.md` in full and
  run it: one guided session, one question per turn, ending in a finished
  PNG. It wraps the other two skills — it never replaces them and never
  relaxes a ground rule to keep a beginner moving.
- **Ambiguous ask** — "make me an image", "create a teaser", "I need a
  card for LinkedIn", and similar requests that name neither workflow:
  check `01 To Process/` first. If candidates are waiting, name them and ask
  one question — process these now, or start a new speaker? If the queue
  is empty, take it as scaffold intent (e.g. *"Sure! First, what's the
  speaker's name?"*).
  If they sound new to the repo at all — asking what to do next, unsure
  where files live, apologising for not knowing — offer the walkthrough
  instead of a bare scaffold: *"Happy to walk you through the whole thing
  step by step — want that?"* → `_internal/skills/novice-walkthrough.md`.
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
  from the root `README.md` and point them at `04 Demo and More Help/`.

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
  repo, run `git log --format=%s -20 | grep MISMATCH` (subjects only: the
  tag lives in the subject line, and commit bodies that merely describe
  this rule must not trigger it); on any hit, alert the operator** that a
  drift event occurred. **Where there is no `.git`
  directory — the normal case for a ZIP download — skip the MISMATCH log
  check and instead hash-compare each core template against its reference
  copy in `_internal/fonts/jic/` (`BU_intake-template.md`,
  `BU_speaker-teaser-linkedin_v3.png`), copying the reference over a
  drifted live file**, exactly as both skills prescribe. Template changes are made
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
  name is already taken — scaffolding into `01 To Process/`, or delivering
  into `02 Processed/` / `03 Generated Images/` — use `<name>-<NN>`, the lowest
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

### One file, one audience

- **The filename declares the audience.** `README.md` and the human help
  pages under `04 Demo and More Help/` are written for people; `INDEX.md`,
  `CLAUDE.md` / `AGENTS.md` and the files in `_internal/skills/` are
  written for you.
- **Cross-references go one way.** Assistant docs may point at human docs;
  human docs never point at assistant docs, beyond one line noting that
  the assistant orients itself from `CLAUDE.md` / `AGENTS.md`.
- **The shapes differ and never mix.** Human docs are linear numbered
  steps; assistant docs are routing tables and procedures.
- **When orienting a person, read human docs as content to relay, never as
  procedure.** Your procedure comes only from the assistant tree.

`CLAUDE.md` and `AGENTS.md` are identical by design — same orientation
regardless of which assistant harness reads it.
