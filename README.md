# Apify Speaker LinkedIn Teasers — demo

One small form in → one finished LinkedIn teaser image out, styled as an
[Apify](https://apify.com) actor card.

Built by [Alex Alderman](https://alderman.agency) as the live demo for the
AI marketing meetup — and left fully working on purpose. If you ever want
real speaker teasers for LinkedIn, this will happily generate them for you.
No pressure to use it either way — I obviously don't know your brand rules
as well as you do. :)

**Unofficial demo.** Not affiliated with or endorsed by Apify. The card is a
reproduction of apify.com's public actor-card design, rebuilt from scratch as
an homage for a non-commercial community-event teaser. Apify's name, logo and
visual design belong to Apify.

## What you need

Nothing to install. The repo is 100% self-contained — fonts, card code,
template and instructions all ship inside this folder; nothing is fetched
from the internet at any point, and there are no scripts or packages.

You only need two things that are almost certainly already on your machine:

- **An AI coding assistant** — any of them. Claude Code, Cursor, Copilot,
  Codex… they all read the same instruction files here (`CLAUDE.md` and
  `AGENTS.md` are identical copies, one name per convention).
- **A Chromium browser** — Google Chrome, or the Microsoft Edge that comes
  preinstalled with Windows. The assistant drives it invisibly ("headless")
  to draw the card; you never open it yourself.

## Make a teaser in four steps

1. **Open this folder in your AI assistant** and just talk to it.
2. Say **"new speaker Jana Novakova"**. A folder appears in `to-process/`
   with the form and a note listing exactly what goes in it.
3. **Fill in the form's boxes** — name, role, company, talk blurb, topic,
   level, minutes; each box says what it wants and how long it can be —
   and **drop in two images**: the company logo and the speaker photo.
   Any size works — just draw a visible "x" where you want the crop
   centred (or supply the exact size named in the folder's README).
4. Say **"process the queue"**. The finished 1200×1200 PNG lands in
   `processed/jana-novakova/`, ready to post.

If anything is off — blurb too long, an image missing, a template box the
wrong shape — the run stops and says exactly what to fix. It never silently
crops your text or stretches the card.

Want to see what "done" looks like first? Open
`intake-template (completed example).md`, or the two finished examples in
`processed/`.

## Where the skills are

The "skills" — the assistant's step-by-step instructions — are two plain
markdown files in the `skills/` folder. Any assistant can run one just by
being pointed at it, and `CLAUDE.md` / `AGENTS.md` route the two operator
phrases to them automatically. Each file opens with a short description of
when it applies:

| skill | file | what it does |
|---|---|---|
| `new-speaker` | `skills/new-speaker.md` | scaffolds one speaker folder in `to-process/` — asks the name, or numbers the folder if you skip it |
| `apify-speaker-card` | `skills/apify-speaker-card.md` | the whole generator: validates the form, measures the template, renders the card, composites and verifies the PNG |

This README is written for you, the human; your assistant orients itself
from `CLAUDE.md` / `AGENTS.md`.

## How it works

```
Canva template (exported PNG)          intake form (markdown)
  ├─ purple block  = card goes here      ├─ labelled boxes: all typed inputs
  └─ green block   = photo goes here     └─ (frontmatter is machine-written)
                    │                        │
                    └────────┬───────────────┘
                             ▼
              the apify-speaker-card skill
        read geometry off the template → validate →
        render the card in a headless Chromium browser →
        composite → verify by inspection
                             ▼
        processed/<speaker>/  (finished PNG + archived form)
```

The card is not a mock-up: it is Apify's `ActorStoreItem` rebuilt in plain
HTML/CSS and verified box-for-box against the live site to 0.001px
(400 × 153.667 at its render width). It renders at native size — real Inter
and IBM Plex Mono, real ellipsis behaviour, the real 2px-shell "border" —
then scales into the template's block.

## The queue

```
to-process/jana-novakova/     one folder per speaker: intake.md, README.md,
                              company-logo.png, speaker.png
processed/jana-novakova/      the whole folder moves here on success,
                              finished PNG inside
```

A folder lives in exactly one queue. Successes move whole; failures stay put
with a printed reason and never write partial output. Nothing is ever
overwritten.

**Images** can be exactly the template's photo-area size (PNG/JPG), or **any
size with a visible mark** — an "x" or scribble at the intended centre —
which the skill reads and
turns into a proportion-correct crop, leaving your original file untouched.
**Missing images** stop that folder with a choice: resubmit, or render with a
dashed in-block placeholder outline that a correctly sized image covers
completely afterwards.

## The intake form

`intake-template.md` is the whole contract — field-by-field schema, measured
character budgets, failure modes. Everything you type lives in its labelled
boxes; the assistant copies your entries into the machine-read frontmatter at
processing time. The short version:

| field | card position | budget |
|---|---|---|
| `company_logo` | 40×40 top-left | square image, ≥80×80 |
| `speaker_name` | title | 30 chars |
| `speaker_position` / `speaker_company` | monospace line, joined with `/` | 39 chars |
| description *(its own labelled box)* | body, clamps at 2 lines | 82 chars |
| `topic_category` | footer left | 33 chars |
| `level` | footer, after 👥 | `all` `easy` `int.` `adv.` |
| `duration_minutes` | footer, after ★ | number only — card shows `10 (mins)` |
| `speaker_image` | fills the green block | any portrait, centre-cropped |

Budgets are measured on the rendered card (binary-searched to the truncation
point, averaged over samples) minus an 8% buffer. Over-budget descriptions are
rejected, not silently cut. The footer's small circle icon is static on every
card and ships with the repo.

See `docs/Actor card field mapping.png` for the annotated picture and
`docs/Actor card text budgets.png` for the budgets table.

## The template is canon

This demo ships exactly one visual template,
`visual-templates/speaker-teaser-linkedin.png` (1200×1200). Whatever that PNG
says, goes — geometry is measured from the coloured blocks per run (Canva's
printed readout panels win when present, to sub-pixel precision), so tweaks
re-exported over the same file are picked up automatically. One rule is
enforced: **the purple block's proportions must match the card as it actually
renders** (±2px at render scale). The card's height is quantised by its
description line count, so it cannot be stretched to fit a wrong box — the
run halts and tells you the height the block should be.

## Repo layout

```
intake-template.md            the input contract (copied into each folder)
intake-template (completed example).md   the same form, filled in
to-process/  processed/       the queue — this is all you touch day to day
visual-templates/             the one fixed base PNG (blocks + readouts)
docs/                         annotated field-mapping + text-budget graphics
internal/                     machinery you never edit: the render page
                              (verified card CSS), static footer icon,
                              self-hosted OFL fonts, folder README template
skills/new-speaker.md         scaffolds a speaker folder
skills/apify-speaker-card.md  the generator — the entire engine
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
