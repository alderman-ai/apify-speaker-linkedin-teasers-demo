# AI Marketing Meetup – hosted by [Apify](https://apify.com)
## (demo by [alex alderman](https://alderman.ai))

>[!Brand disclaimer]+
> Apify's name, logo and visual design belong to Apify. No social or custom-coded web assets were **==` seriously  harmed`== ** in the process of making this demo, **==` just a minor scrape `==** 🥰
# Start here

Want to just dive in?
```path-1-create-new-image

```


## Demo Summary

- `_IN_` A few speaker details + IMG files into a fully agentic pipeline 
- `_OUT_` one pixel-perfect, Apify branded LinkedIn image
- `_OR_` 30 images? 50? Just like at the arcade, you can play until you're out of tokens.
- `_WITH_` no brand kit, no Apify design assets 😇 ... asked for.

Built by [Alex Alderman](https://alderman.agency) for a live demo at the AI marketing meetup 17.09.

**Unofficial demo.** Not affiliated with or endorsed by Apify. The card is a
reproduction of apify.com's public actor-card design, rebuilt from scratch as
an homage for a non-commercial community-event teaser. Apify's name, logo and
visual design belong to Apify.

## What you need

- **An agentic AI coding assistant** — Claude Code, or another lab's
  equivalent: something that can open this folder, read and write files in
  it, and run a shell command. It orients itself from `CLAUDE.md` /
  `AGENTS.md` (identical copies, one name per convention). 
- **A Chromium browser** — Google Chrome, or the Microsoft Edge that comes
  preinstalled with Windows. The assistant drives it invisibly ("headless")
  to draw the image; you never open it yourself.

Nothing to install beyond that. The repo is 100% self-contained — fonts,
card code, template and instructions all ship inside this folder; nothing
is fetched from the internet at any point, and there are no scripts or
packages. Wary of cloned repos? Good instinct —
`demo-and-more-help/about-this-project/scripts-and-security.md` is a
plain-English inventory of every piece of code in this project and when it
runs (short version: there are **zero executable scripts** here).



Open this folder in your assistant and say:

> show me the demo

It answers with a short menu. Pick the first line and it runs the whole
pipeline in front of you on the bundled demo speaker — form checked,
template measured, card rendered in a headless browser, result inspected,
finished PNG delivered to `generated-images/`. Then it offers to do the same
for a real speaker: you give it the name, role, company, talk blurb, topic,
minutes and two square images right there in chat, and it
takes it from there. (The other two menu lines aren't built yet.)

If anything is off — blurb too long, an image missing or not square, a
template box the wrong shape — the run stops and says exactly what to fix.
It never silently crops your text, reframes your photo, or stretches the
card. And it works on a branch of its own: unless you are the author, the
assistant puts your session on `session/<date>-<NN>` before it touches
anything, so `main` — and the core template every card is built from —
stays exactly as you cloned it. `git switch main` gets you back to a clean
slate at any time.

Want to see what "done" looks like first? The finished PNG in
`generated-images/` is the author's own card, and
`demo-and-more-help/example-speakers/` holds twelve more.

## What the image looks like

Two elements sit on a fixed 1200×1200 backdrop (dark, orange-framed, with
a starfield that brightens toward the bottom — all baked into the
template):

- **The actor card** — Apify's `ActorStoreItem` rebuilt in plain HTML/CSS
  and verified box-for-box against the live site to 0.001px. Your name,
  role / company, blurb, topic and talk length fill its slots (the
  audience level is fixed at "For All Levels"); real Inter and IBM Plex Mono, real ellipsis behaviour, the real
  hover ring.
- **The speaker element** — a matching grey-framed card holding your
  square photo with **Join me in PRAGUE** underneath. Its frame, corners
  and copy are fixed; only the photo is yours.

## The three queue stages

```
to-process/<speaker>/                 the assistant builds this from what you
                                      tell it: intake.md, README.md,
                                      company-logo.png, speaker.png
processed/<speaker>/                  the folder moves here on success (archive)
generated-images/<speaker>-final.png  the finished render — publish from here
```

A folder lives in exactly one stage. Successes move whole; failures stay
put with a printed reason and never write partial output. Nothing is ever
overwritten — a name that's already taken gets a `-01`, `-02`… suffix,
which is also why the demo run lands as `alex-alderman-01`: the author's
original card already holds the bare name.

**Missing images** stop the run with a choice: resubmit, or render now with
a dashed placeholder outline that a correctly sized image covers completely
afterwards.

## The details it asks for

The `intake.md` in each speaker folder is the machine record and the whole
contract — field-by-field schema, character budgets, failure modes. You
don't have to open it: the assistant writes in what you tell it and checks
the budgets as it goes. (Filling it by hand still works if you prefer; its
labelled boxes say what goes where.) The short version:

| field | where it lands | budget |
|---|---|---|
| company logo | 40×40 top-left of the card | square image, ≥80×80 |
| speaker name | card title | 30 chars |
| position / company | monospace line, joined as `position / company` | 39 chars incl. the ` / ` |
| description | card body, clamps at 2 lines | 100 chars |
| topic category | card footer left | 26 chars |
| level | card footer, after 👥 | fixed: `For All Levels` — not an input |
| duration | card footer, after ★ | number only — card shows `10 (mins)` |
| speaker photo | square photo slot of the speaker element | square PNG/JPG/JPEG ≤800×800 (262×262 ideal) |

The speaker photo must be **square** (same width as height) and no bigger
than 800×800. The pipeline scales it into place without cropping a single
pixel; what it will never do is crop or reframe for you — square it
yourself, framed the way you want to be seen. Over-budget descriptions are
rejected, not silently cut. The footer's small orange `?` circle is static
on every card and ships with the repo.

See `demo-and-more-help/filling-in-the-form/Actor card field mapping.png`
for the annotated picture and
`demo-and-more-help/filling-in-the-form/Actor card text budgets.png` for
the budgets table.

## The template is canon

This demo ships exactly one visual template,
`_internal/core-templates-please-dont-touch/speaker-teaser-linkedin_v3.png`
(1200×1200). Whatever that PNG says, goes — the coloured placeholder blocks
on it are measured per run and decide exactly where the two elements land.
As the folder name suggests: don't touch it in passing. A deliberate
template change is a versioned event — that folder's README says how.

The current template is **machine-built**: the original Canva export (the
one with the green and purple squares) has been superseded by a version
with the starfield baked in and the blocks redrawn at the confirmed layout.
The rebuild recipe lives in that folder's README, so a future design
change is a re-run of that recipe, not a hunt.

One rule is enforced every run: **the purple block's proportions must match
the card as it actually renders** (±2px at render scale). The card's height
is quantised by its description line count, so it cannot be stretched to
fit a wrong box — the run halts and tells you the height the block should
be.

## Where the skills are

The "skills" — the assistant's step-by-step instructions — are two plain
markdown files in `_internal/skills/`. Any agentic assistant runs one just
by being pointed at it, and `CLAUDE.md` / `AGENTS.md` route your requests
to them:

| skill | file | what it does |
|---|---|---|
| `new-speaker` | `_internal/skills/new-speaker.md` | scaffolds one speaker folder in `to-process/` and collects the speaker's details from you in chat, writing them into the form |
| `apify-speaker-card` | `_internal/skills/apify-speaker-card.md` | the whole generator: validates the form, measures the template, renders, verifies, delivers — and holds the demo run the menu's first line executes |

This README is written for you, the human; your assistant orients itself
from `CLAUDE.md` / `AGENTS.md`.

## Repo layout

```
README.md                     you are here — the only doc you need to start
to-process/  processed/       the queue — built and moved by the assistant
generated-images/             the finished PNGs (<speaker>-final.png)
demo-and-more-help/           lost, curious, or cautious? three subfolders:
                              filling-in-the-form/, example-speakers/ and
                              about-this-project/ — start at its README.md
_internal/                    machinery you never edit: the skills, the
                              bundled demo speaker, the render page,
                              self-hosted fonts, and the two core templates
                              everything is generated from
CLAUDE.md / AGENTS.md         how your assistant orients itself
```

Every folder carries its own `INDEX.md` describing what's inside.

## Fonts and licences

Bundled: **Inter** and **IBM Plex Mono** — exactly what apify.com serves for
the card — under the SIL Open Font License 1.1
(`_internal/fonts/licenses/`), freely redistributable, self-hosted so
renders never depend on a CDN. GT Walsheim (apify.com's heading face) is a
commercial font and is deliberately not part of this repo.
