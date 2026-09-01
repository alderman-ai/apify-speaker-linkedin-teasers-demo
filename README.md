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
  to draw the image; you never open it yourself.

## Make a teaser in four steps

1. **Open this folder in your AI assistant** and just talk to it.
2. Say **"new speaker Jana Novakova"**. A folder appears in `to-process/`
   with the form and a note listing exactly what goes in it.
3. **Fill in the form's boxes** — name, role, company, talk blurb, topic,
   level, minutes; each box says what it wants and how long it can be —
   and **drop in two images**:
   - the company logo (square, 80×80 or larger), and
   - the speaker photo — **a square image** (same width as height),
     PNG/JPG/JPEG, **no bigger than 800×800**. If you can supply exactly
     262×262 that's the perfect fit, but any square within the cap works:
     the pipeline scales it into place without cropping a single pixel.
     What it will never do is crop or reframe for you — square it
     yourself, framed the way you want to be seen.
4. Say **"process the queue"**. The finished 1200×1200 PNG lands in
   `final-output/jana-novakova-final.png`, ready to post; your folder is
   archived to `processed/`.

If anything is off — blurb too long, an image missing or not square, a
template box the wrong shape — the run stops and says exactly what to fix.
It never silently crops your text, reframes your photo, or stretches the
card.

Want to see what "done" looks like first? Open
`intake-template (completed example).md`, or the finished PNGs in
`final-output/`.

## What the image looks like

Two elements sit on a fixed 1200×1200 backdrop (dark, orange-framed, with
a starfield that brightens toward the bottom — all baked into the
template):

- **The actor card** — Apify's `ActorStoreItem` rebuilt in plain HTML/CSS
  and verified box-for-box against the live site to 0.001px. Your name,
  role / company, blurb, topic, talk length and audience level fill its
  slots; real Inter and IBM Plex Mono, real ellipsis behaviour, the real
  hover ring.
- **The speaker element** — a matching grey-framed card holding your
  square photo with **Join me in PRAGUE** underneath. Its frame, corners
  and copy are fixed; only the photo is yours.

## The three queue stages

```
to-process/<speaker>/            you fill this: intake.md, README.md,
                                 company-logo.png, speaker.png
processed/<speaker>/             the folder moves here on success (archive)
final-output/<speaker>-final.png the finished render — publish from here
```

A folder lives in exactly one stage. Successes move whole; failures stay
put with a printed reason and never write partial output. Nothing is ever
overwritten — re-running a speaker means clearing their old results first.

**Missing images** stop the run with a choice: resubmit, or render now with
a dashed placeholder outline that a correctly sized image covers completely
afterwards.

## The intake form

`intake-template.md` is the whole contract — field-by-field schema,
character budgets, failure modes. Everything you type lives in its labelled
boxes; the assistant copies your entries into the machine-read frontmatter
at processing time. The short version:

| field | where it lands | budget |
|---|---|---|
| `company_logo` | 40×40 top-left of the card | square image, ≥80×80 |
| `speaker_name` | card title | 30 chars |
| `speaker_position` / `speaker_company` | monospace line, joined as `position / company` | 39 chars incl. the ` / ` |
| description *(its own labelled box)* | card body, clamps at 2 lines | 140 chars |
| `topic_category` | card footer left | 33 chars |
| `level` | card footer, after 👥 | `all` `easy` `int.` `adv.` |
| `duration_minutes` | card footer, after ★ | number only — card shows `10 (mins)` |
| `speaker_image` | square photo slot of the speaker element | square PNG/JPG/JPEG ≤800×800 (262×262 ideal) |

Over-budget descriptions are rejected, not silently cut. The footer's small
orange `?` circle is static on every card and ships with the repo.

See `docs/Actor card field mapping.png` for the annotated picture and
`docs/Actor card text budgets.png` for the budgets table.

## The template is canon

This demo ships exactly one visual template,
`visual-templates/speaker-teaser-linkedin.png` (1200×1200). Whatever that
PNG says, goes — the coloured placeholder blocks on it are measured per run
and decide exactly where the two elements land.

The current template is **machine-built**: the original Canva export (the
one with the green and purple squares) has been superseded by a version
with the starfield baked in and the blocks redrawn at the confirmed layout.
The rebuild recipe lives in `visual-templates/README.md`, so a future
design change is a re-run of that recipe, not a hunt.

One rule is enforced every run: **the purple block's proportions must match
the card as it actually renders** (±2px at render scale). The card's height
is quantised by its description line count, so it cannot be stretched to
fit a wrong box — the run halts and tells you the height the block should
be.

## Where the skills are

The "skills" — the assistant's step-by-step instructions — are two plain
markdown files in the `skills/` folder. Any assistant can run one just by
being pointed at it, and `CLAUDE.md` / `AGENTS.md` route the two operator
phrases to them automatically:

| skill | file | what it does |
|---|---|---|
| `new-speaker` | `skills/new-speaker.md` | scaffolds one speaker folder in `to-process/` — asks the name, or numbers the folder if you skip it |
| `apify-speaker-card` | `skills/apify-speaker-card.md` | the whole generator: validates the form, measures the template, renders, verifies, delivers |

This README is written for you, the human; your assistant orients itself
from `CLAUDE.md` / `AGENTS.md`.

## Repo layout

```
intake-template.md            the input contract (copied into each folder)
intake-template (completed example).md   the same form, filled in
to-process/  processed/       the queue — this is all you touch day to day
final-output/                 the finished PNGs (<speaker>-final.png)
visual-templates/             the canon template + the starfield pattern
docs/                         annotated field-mapping + text-budget graphics
internal/                     machinery you never edit: the render page
                              (verified card CSS + speaker element), static
                              footer icon, self-hosted OFL fonts
skills/new-speaker.md         scaffolds a speaker folder
skills/apify-speaker-card.md  the generator — the entire engine
```

## Fonts and licences

Bundled: **Inter** and **IBM Plex Mono** — exactly what apify.com serves for
the card — under the SIL Open Font License 1.1
(`internal/fonts/licenses/`), freely redistributable, self-hosted so renders
never depend on a CDN. GT Walsheim (apify.com's heading face) is a
commercial font and is deliberately not part of this repo.
