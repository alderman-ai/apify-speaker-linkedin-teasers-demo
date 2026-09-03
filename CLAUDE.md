# Assistant orientation — Apify Speaker LinkedIn Teasers

You are in a self-contained demo repo that mass-produces LinkedIn teaser
images for meetup speakers, styled as Apify actor cards. The operator gives
you a handful of details per speaker — in chat — and two square images;
you render a pixel-faithful actor card and a card-style speaker portrait
element into the canon template and deliver a finished PNG. **You are the
engine** — there are deliberately no build scripts and no package
dependencies. The only external requirement is a Chromium-based browser for
headless rendering — Chrome, or the Edge preinstalled on Windows; never
download one. The repo assumes an agentic assistant (Claude Code, or
another lab's equivalent) that can read and write files here and run one
shell command; there is no non-agentic path.

## Session opener — the three lines

This is a demo, and most people opening it have never seen it. **On the
first message of a session, present this menu before doing anything else**,
whatever that message says ("show me the demo", "hi", "what is this?").
The one exception: a first message that is already an unambiguous
scaffold, generate or template-change request (see routing below) — route
it directly, no menu.

> This repo turns a few speaker details into a finished LinkedIn teaser
> image, styled as an Apify actor card. Three lines — pick one:
>
> 1. **See the workflow in action** and generate a templated social media
>    image.
> 2. **Learn about text-field-mapped visual assets** in general, and why
>    they enable pixel-perfect branded content.
> 3. **Find out how this demo was built.**

- **Line 1** → the demo run. Read `_internal/skills/apify-speaker-card.md`
  in full and follow its **"Demo run"** section: copy the bundled demo
  speaker (`_internal/demo-speaker/` — the repo author) into the queue,
  process that copy end to end while narrating each stage in a line, then
  offer to do the same for a real speaker.
- **Line 2 or 3** → reply with exactly this sentence, then show the menu
  again: `Sorry, this option is temporarily out of order, please try again
  from another line.` Do not improvise content for these lines; they are
  not built yet.
- Anything else (a number outside 1–3, a question) → answer in a line,
  then show the menu again.

## Asset map — every file you may need

Procedure and contract (`_internal/`):

| path | what |
|---|---|
| `_internal/skills/apify-speaker-card.md` | **the generator's complete operating procedure — read it in full before processing anything; follow it, don't improvise.** Mass-produces Apify-styled speaker teaser images: renders a pixel-faithful Apify actor card (repurposed as a speaker card) and a card-style speaker portrait element into the canon template, one finished PNG per speaker. Holds the **"Demo run"** section that line 1 of the session menu executes. Use when the operator picks line 1, says "process the intake forms", "process the queue", "generate the speaker cards", "new speaker <name>", or drops folders into `to-process/` |
| `_internal/skills/new-speaker.md` | **the scaffolder's complete procedure — read it in full before scaffolding.** Adds a new speaker folder to the queue and collects that speaker's details in chat — name, role, company, topic, minutes, blurb and the two images (audience level is fixed) — writing them into the form itself (asks the name; declined → `new-speaker-<NN>`, lowest free number; a repeat name → `<name>-<NN>`, first dupe = 01). Use when the operator says "new speaker", "/new-speaker", "add a speaker", "scaffold a speaker folder", or names a person to add to the lineup |
| `_internal/demo-speaker/` | the bundled demo speaker: a complete, ready-to-run speaker folder (filled `intake.md`, `README.md`, `company-logo.png`, `speaker.png`) for the repo author. Line 1 of the menu copies it into `to-process/` and processes the copy. **Never process or edit it in place** |
| `_internal/core-templates-please-dont-touch/intake-template.md` | the input contract: every field, budget, failure mode; the operator's values live in labelled body fences that you fill from chat, mirrored into the frontmatter. Versioned in its frontmatter (`version` / `versioned_at`). Copied into each folder as `intake.md` |
| `demo-and-more-help/filling-in-the-form/intake-template-completed-example.md` | the same template with every fence and frontmatter value filled in — what "done" looks like (byte-identical to the demo speaker's `intake.md`) |

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
| `_internal/core-templates-please-dont-touch/speaker-teaser-linkedin_v3.png` | the one canon template (1200×1200, currently v3), **machine-built**: baked gradient starfield + purple card block + green speaker block. Supersedes the operator's original Canva export |

Reference (`demo-and-more-help/` — the operator-facing help and showcase
folder; its root holds only `INDEX.md` and `README.md`, everything else
sits in `filling-in-the-form/`, `example-speakers/` or `about-this-project/`):

| path | what |
|---|---|
| `demo-and-more-help/filling-in-the-form/Actor card field mapping.png` | annotated picture of every field and where it lands on the card |
| `demo-and-more-help/filling-in-the-form/Actor card text budgets.png` | the character-budget table for every text field |
| `demo-and-more-help/filling-in-the-form/field-mapping.html`, `demo-and-more-help/filling-in-the-form/text-budgets.html` | those graphics' sources, re-renderable with headless Chrome |
| `demo-and-more-help/about-this-project/scripts-and-security.md` | the plain-English inventory of every piece of code in this repo and when it runs. **If any change adds code or scripts anywhere in the repo, this file must be updated in the same change — keep it truthful** |
| `demo-and-more-help/about-this-project/pipeline-evaluation.md` | the 2026-09-01 ten-speaker stress test of the docs and pipeline |
| `demo-and-more-help/example-speakers/fictional-characters/` | the fictional demo speakers (folklore characters + synthetic personas), archived out of the live queue: `processed/` folders + `generated-images/` PNGs |
| `demo-and-more-help/example-speakers/real-people-stress-test/` | the simulated-"real" stress-test speakers (real Czech public figures), kept as mid-project snapshots: `processed/` + `generated-images/` |

Every folder carries its own `INDEX.md` for routing. `README.md` is
reserved for orientation: the root README (the human entry point), the
core-templates README (a warning plus the template recipe), the
`demo-and-more-help/` README (help-folder orientation, pointing
assistants at its INDEX), and each speaker folder's README (its
checklist).

## What operators say — routing requests

The `_internal/skills/` files are plain markdown procedures, **not deployed
harness skills — nothing auto-loads them**. You route by intent and read
the matching skill file in full before acting. The trigger phrases here and
in the skill descriptions are examples, not a grammar: operators paraphrase
freely, and many have never seen this repo. When in doubt on a first
message, show the session menu.

- **Scaffold intent** — "new speaker Alex Alderman", "add a speaker",
  "put Alex on the lineup", "make me a card for Alex", "set up a folder
  for our next speaker" → read `_internal/skills/new-speaker.md`, then
  scaffold `to-process/alex-alderman/` (intake form + README; without a
  name, `new-speaker-<NN>` at the lowest free number) and **collect the
  rest of the details in chat**, writing them into the form yourself. If
  the operator asked for the image, not just the folder, and everything
  is present, continue straight into generation.
- **Generate intent** — "process the queue", "process the intake forms",
  "generate the speaker cards", "run the pipeline", "render the pending
  ones" → read `_internal/skills/apify-speaker-card.md`, then run every
  folder in `to-process/` (or the ones named) through its procedure:
  validate → read geometry off the template → ratio check → render in a
  headless Chromium browser → verify by inspection → PNG to
  `generated-images/<speaker>-final.png`, folder to `processed/`.
- **Demo intent** — "show me the demo", "demo", "run the example", or a
  pick of line 1 from the menu → the "Demo run" section of
  `_internal/skills/apify-speaker-card.md`.
- **Ambiguous ask** — "make me an image", "create a teaser", "I need a
  card for LinkedIn", and similar requests that name neither workflow:
  check `to-process/` first. If candidates are waiting, name them and ask
  one question — process these now, or start a new speaker? If the queue
  is empty, take it as scaffold intent (e.g. *"Sure! First, what's the
  speaker's name?"*).
- **Template-change intent** — "update the template", "change the intake
  form", "new visual template", "edit the canon template", or any request
  that would modify a file in `core-templates-please-dont-touch/`: those
  two files are what every card is generated from, so make the change
  deliberately, as a versioned event — the intake form bumps `version` /
  `versioned_at` in its frontmatter (and the completed example plus the
  demo speaker's `intake.md` are refreshed from it); the visual template
  bumps its `_v<N>` filename suffix and every reference to it (this file,
  both skills, the READMEs, the completed example, the demo speaker).
  Render one card afterwards to confirm. That folder's README has the
  details and the starfield rebuild recipe.
- **Maintainer-local slash skills** — on the author's machine only:
  `/init-new-speaker <name>` (scaffold the folder for manual intake and
  stop), `/create-speaker <name>` (chat intake → render; a folder that
  already holds all three assets is processed as is), `/create-speaker`
  with no argument (process every complete folder in `to-process/`, skip
  the rest) and `/create-fictional-speaker <name>` (real name →
  researched persona → render → published at alderman.ai/apify-live-demo).
  They wrap the two procedure files above, live in `~/.claude/skills/`,
  not in this repo; nothing here depends on them.
- **Lost human** — "what is this?", "how does this work?": show the
  session menu; if they want more, orient them from the root `README.md`
  and point them at `demo-and-more-help/`.

## Ground rules (the skill has the full list)

- **Input is collected in chat.** The intake form is the machine record
  and the contract; the operator never has to open it. Ask for each field
  with its budget, validate as you go (character counts, kebab-case — offer the fix, never silently alter), and write
  the value into both its body fence and its frontmatter key yourself.
  Images arrive as file paths; copy them into the folder, never move the
  original. A form someone filled by hand is still accepted as is.
- **The template image is canon.** Placeholder geometry is read from it per
  run — purple block = actor card, green block = speaker element.
  Frontmatter geometry keys are outputs you write back, never inputs.
- **Core templates change on purpose only.** A run reads them and never
  writes them; a deliberate change follows the versioning convention
  under template-change routing above.
- **Halt, don't degrade.** Over-budget description, missing assets (ask:
  resubmit vs placeholder outline), an off-spec speaker photo (must be an
  exact square PNG/JPG/JPEG ≤800×800 — you scale it to the slot, you never
  crop or reframe it), block/card ratio mismatch, geometry disagreement —
  each is a stop with a clear report, never a silent workaround. Never
  trim operator text, never stretch the card, never overwrite anything.
- **Duplicate names suffix, never block.** A repeat name is legitimate (a
  rebuilt card, a fresh start after text edits, a namesake, a demo run):
  wherever the name is already taken — scaffolding into `to-process/`, or
  delivering into `processed/` / `generated-images/` — use `<name>-<NN>`,
  the lowest free number, zero-padded, **first dupe = 01** (same NN on the
  archive folder and the PNG). Detection is by folder/file names only; a
  repeated frontmatter `speaker_name` breaks nothing and is never
  consulted.
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
