# Apify Speaker LinkedIn Teasers — demo

Mass-produce LinkedIn teaser images for meetup speakers, styled as
[Apify](https://apify.com) actor cards. You design a template once in Canva
with two coloured placeholder blocks; after that, each teaser is one small
markdown form. A Claude Code skill renders a pixel-faithful actor card and a
speaker photo into the template and writes a finished PNG. **No build scripts,
no dependencies to audit — just Chrome and the skill.**

## Start here (you, the human)

This repo turns a one-page form into a finished LinkedIn teaser image, one
per speaker, in your event's Apify-card style. You never touch design tools
after the template exists — the whole loop is:

1. **Open this folder in [Claude Code](https://claude.com/claude-code)**
   (Chrome installed is the only requirement).
2. Say **"new speaker Jana Novakova"** — you get a folder in `to-process/`
   with a form and a note listing exactly what to drop in.
3. **Fill the form** (name, role, talk blurb, level, minutes) and **drop in
   two images** — company logo and speaker photo. Either exactly 400×400, or
   any size with a visible "x" marked at the point you want centred.
4. Say **"process the queue"**. Finished PNGs land in `processed/`, one
   folder per speaker, ready to post.

If something is off — blurb too long, an image missing, the template box the
wrong shape — the run stops and tells you exactly what to fix. It never
silently crops your text or stretches the card.

This README is written for you; your assistant reads `CLAUDE.md` /
`AGENTS.md` (identical files) and the skills in `.claude/skills/` for its
operating procedure.

---

**Unofficial demo.** Not affiliated with or endorsed by Apify. The card is a
reproduction of apify.com's public actor-card design, rebuilt from scratch as
an homage for a non-commercial community-event teaser. Apify's name, logo and
visual design belong to Apify.

## How it works

```
Canva template (exported PNG)          intake form (markdown)
  ├─ purple block  = card goes here      ├─ labelled fences: all typed inputs
  └─ green block   = photo goes here     └─ (frontmatter is machine-written)
                    │                        │
                    └────────┬───────────────┘
                             ▼
              the apify-speaker-card skill
        read geometry off the template → validate →
        render the card page in headless Chrome →
        composite → verify by inspection
                             ▼
        processed/<speaker>/  (finished PNG + archived form)
```

The card is not a mock-up: it is Apify's `ActorStoreItem` rebuilt in plain
HTML/CSS and verified box-for-box against the live site to 0.001px
(400 × 153.667 at its render width). It renders at native size in headless
Chrome — real Inter and IBM Plex Mono, real ellipsis behaviour, the real
2px-shell "border" — then scales into the template's block.

## The queue

```
to-process/jana-novakova/     one folder per speaker: intake.md, README.md,
                              company-logo.png, speaker.png
processed/jana-novakova/      the whole folder moves here on success,
                              finished PNG inside
```

Scaffold a folder by saying **"new speaker Jana Novakova"** (no name →
`new-speaker-<N>`, renamed from the form at processing time).
A folder lives in exactly one queue. Successes move whole; failures stay put
with a printed reason and never write partial output. Nothing is ever
overwritten.

**Images** can be exactly **400×400** (PNG/JPG), or **any size with a visible
mark** — an "x" or scribble at the intended centre — which the skill reads and
turns into a proportion-correct crop (`speaker_focus`), leaving the original
untouched. **Missing images** stop that folder with a choice: resubmit, or
render with a dashed in-block placeholder outline that a correctly sized image
covers completely post-hoc.

## Quick start

Two skills ship in `.claude/skills/` and load automatically:

- **`new-speaker`** — asks for the speaker's name and scaffolds the folder
  (declining the name gives `new-speaker-<NN>`, lowest free number).
- **`apify-speaker-card`** — "process the queue": validates, renders,
  verifies, delivers. It reads the template's printed dimension panels (paste Canva's
Width/Height/X/Y readout into the placeholders for sub-pixel accuracy) and
sanity-checks them against the visible blocks.

## The intake form

`intake-template.md` is the whole contract — field-by-field schema, measured
character budgets, failure modes. Everything you type lives in its labelled
body fences; the assistant copies fence values into the frontmatter at
processing time. `intake-template (completed example).md` beside it shows a
finished one. The short version:

| field | card position | budget |
|---|---|---|
| `company_logo` | 40×40 top-left | square image, ≥80×80 |
| `speaker_name` | title | 30 chars |
| `speaker_position` / `speaker_company` | monospace line, joined with `/` | 39 chars |
| description *(fenced block, not frontmatter)* | body, clamps at 2 lines | 82 chars |
| `topic_category` | footer left | 33 chars |
| `level` | footer, after 👥 | `all` `easy` `int.` `adv.` |
| `duration_minutes` | footer, after ★ | number only — card shows `10 (mins)` |
| `speaker_image` | fills the green block | any portrait, centre-cropped |

Budgets are measured on the rendered card (binary-searched to the truncation
point, averaged over samples) minus an 8% buffer. Over-budget descriptions are
rejected, not silently cut. The footer's small circle icon is static on every
card and ships in `render/`.

See `docs/Actor card field mapping.png` for the annotated picture and
`docs/Actor card text budgets.png` for the budgets table.

## The template is canon

Whatever the template PNG says, goes — geometry is measured from the coloured
blocks per run (Canva's printed readout panels win when present, to sub-pixel
precision). Move or resize blocks in Canva, re-export, done. One rule is
enforced: **the purple block's proportions must match the card as it actually
renders** (±2px at render scale). The card's height is quantised by its
description line count, so it cannot be stretched to fit a wrong box — the
run halts and tells you the height the block should be.

## Repo layout

```
intake-template.md            the input contract (copied into each folder)
to-process/  processed/       the queue — this is all you touch day to day
visual-templates/             the one fixed base PNG (blocks + readouts)
docs/                         the annotated field-mapping graphic
internal/                     machinery you never edit: the render page
                              (verified card CSS), static footer icon,
                              self-hosted OFL fonts, folder README template
.claude/skills/new-speaker/         scaffolds a speaker folder
.claude/skills/apify-speaker-card/  the generator — the entire engine
```

## Fonts and licences

Bundled: **Inter** and **IBM Plex Mono** — exactly what apify.com serves for
the card — under the SIL Open Font License 1.1 (`internal/fonts/licenses/`), freely
redistributable, self-hosted so renders never depend on a CDN.

**Not bundled: GT Walsheim**, apify.com's heading face, used only on the
Canva side of the template. It is a commercial Grilli Type font and cannot
be redistributed here. For mockups, Grilli Type offers free trial fonts at
[grillitype.com](https://www.grillitype.com/free-trial-fonts) (their trial
EULA: mockups/testing only); for anything shipped, license it there. The
render pipeline itself never needs it.
