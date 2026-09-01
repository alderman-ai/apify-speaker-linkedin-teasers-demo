---
# =============================================================================
# ACTOR CARD INTAKE  —  one file = one rendered card
# Lives as intake.md inside one speaker folder under to-process/.
# Scaffold a folder by telling the assistant: new speaker "Name Surname"
# Relative paths resolve from this file, i.e. from inside that folder.
# Every field is documented in §2 below. Operators type ONLY in the fenced
# input sections in the body ("Input presentation details here"); as step 1
# of processing the assistant transfers each fence's contents into its
# frontmatter variable here. This frontmatter is the machine record, not
# the authoring surface. Everything else is documentation for humans.
# =============================================================================

# --- 1. base template --------------------------------------------------------
# Fixed for this demo: ONE visual template with fixed dimensions (1200x1200).
base_image:         "../../visual-templates/speaker-teaser-linkedin.png"

# --- 2. images ---------------------------------------------------------------
# NOTE: the small circle icon in the card footer is STATIC across every
# generated card and is bundled with the skill. It is not an input.
assets_root:        ""              # images live beside this form, in the folder
company_logo:       "company-logo.png"
speaker_image:      "speaker.png"   # square 1:1, PNG/JPG/JPEG, at most 800x800.
                                     # Ideal: 262x262 (the slot's exact size).
                                     # The skill scales an accepted square to
                                     # fit; anything off-spec halts the run.

# --- 3. speaker -- WRITTEN BY ASSISTANT from the body fences -----------------
speaker_name:       "Jana Novakova"
speaker_position:   "head-of-growth"
speaker_company:    "acme-analytics"

# --- 4. the talk -- WRITTEN BY ASSISTANT from the body fences ----------------
# NOTE: the presentation description is NOT a frontmatter field. It lives in
# the body below, in the presentation-description fence. See just under the
# frontmatter, and §2.
topic_category:     "Content Ops"

# --- 5. talk metadata (card footer, right) -- WRITTEN BY ASSISTANT -----------
level:              "int."              # all | easy | int. | adv.
duration_minutes:   "10"              # number only — the card renders 10 (mins)

# --- 6. placement -- DERIVED, DO NOT AUTHOR ----------------------------------
# The template image is canon. The skill measures both placeholders from
# base_image at generation time and writes the eight values back here as a
# record of what it used. Anything you type is overwritten. See §1b.
# Values below are from the last run, not a specification.

# actor card block — purple placeholder
card_x:             200.5
card_y:             748.028
card_w:             799
card_h:             306.949

# speaker card block — green placeholder (the full footprint of the
# card-style speaker component: square photo + "Join me in PRAGUE" bar)
speaker_x:          705.5
speaker_y:          345
speaker_w:          294
speaker_h:          336

# --- 7. render options -------------------------------------------------------
# output is optional. Default: final-output/<speaker-name>-final.png
output:             ""
card_width:         400
desc_lines:         2
href:               ""
---

# Input presentation details here

These fields will be filled in by operator -- or by the assistant if the info was give during the new-speaker skill -- and as the first step of converting this intake form into a completed 1200x1200 LinkedIn teaser image, the assistant will transfer their contents to the appropriate frontheader variable state.

>[! Instructions for the HUMAN]+
>`<from_alderman_ai>`
>
>Hi! All you need to do is fill in these fields below, Ill let you know when the input sections are over. The rest of the document is for the agents.  And, um, delete the `[type here]` 🙂 Depending on how you used the skills for this repo, some may be filled in for you already.
>
>`</from_alderman_ai>`

## Speaker name

Title Case, the form the speaker uses publicly. Fits whole up to **30
characters**; longer is cut off with `…` on the card.

```speaker-name
Jana Novakova
```

## Position

Lowercase kebab-case (words joined by `-`), mirroring Apify's slug style —
that's the pastiche, keep it.

```speaker-position
head-of-growth
```

## Company

Lowercase kebab-case again. Position and company render joined with a
spaced slash as `position / company`; together they fit up to **39
characters** counting the 3-character ` / ` joiner.

```speaker-company
acme-analytics
```

## Topic category

The track or theme, Title Case. Fits up to **33 characters**.

```topic-category
Content Ops
```

## Audience level

Choose the level of complexity of the talk. See **audience level notes**
below. Choose one of these **exactly** (keep the trailing dot on the two
abbreviations):

- `all`
- `easy`
- `int.`
- `adv.`

```level
int.
```

### Audience level notes

- All --  mostly conceptual, all levels can benefit
- Easy -- more basic, and specific enough that advanced might be bored
- Int. -- Intermediate, for those who know what an md file and a repo are
- Adv. -- More technical topics that might be not very fun for a beginner.

## Duration

Talk length in minutes — the number only, no unit; the card renders it as
`10 (mins)` automatically.

```duration-minutes
10
```

## Presentation description

The short blurb under the speaker's name on the card. The number in the
fence label is the enforced character budget: the two-line capacity of the
**actor card's description text slot** with the card at its native CSS
render width (`card_width: 400`), operator-calibrated against real renders.
Exceeding the budget rejects the form; the card is never silently truncated.

```presentation-description-140-char-max

How we cut lead research from six hours a week to twenty minutes.
```

## End of inputs

>[! Instructions for the HUMAN]+
>`<from_alderman_ai>`
>
>You did it! all done! A couple things to couple check:
>- (you did delete the `[brackets]` right?)
>- this file should be in the `to-process/<speaker-name>/` along with:
	>- their speaker photo — **square (1:1)**, PNG/JPG/JPEG, at most 800×800; 262×262 is the ideal supply
	>- their company logo - **accepted file types**: (PNG | ICO | JPG | JPEG)
>
>`</from_alderman_ai>`

That's everything to type. Two things remain to **drop into this folder as
files** (see the folder's README): the company logo (`company-logo.png`) and
the speaker photo (`speaker.png`) — **square, at most 800×800**, framed the
way you want it shown. The skill scales a square photo into the slot
exactly; it never crops or reframes one. Everything below this line is for
the agents.

---

# Actor Card — input contract

This file is **input only**. All markup, CSS, fonts and render logic live in
the skill. Filling the fenced inputs above and pointing the skill at the file
is the entire authoring step; the assistant copies each fence into its
frontmatter variable (step 1 of processing) before anything renders.

---

## 1. Insertion map

Where every frontmatter key lands. This is the lookup table — if a value
renders in the wrong place, the bug is one row of this table.

The card is Apify's `ActorStoreItem` used as a **shell for a speaker
lineup**. The slot names are Apify's; the meanings are yours. The speaker
photo does not land on the card at all — it fills the separate speaker
element (§1b).

| Frontmatter key | Means | Hook in the card | How it is applied |
|---|---|---|---|
| `company_logo` | speaker's company logo | `[data-slot="icon"]` | `src`, via `assets_root` |
| `speaker_name` | speaker's name | `[data-slot="title"]` | `textContent` |
| `speaker_position` + `speaker_company` | position / company | `[data-slot="slug"]` | joined with a spaced ` / `, see §2 |
| *description fence (body)* | the talk blurb | `[data-slot="description"]` | `textContent`, from the fence, not frontmatter |
| *(static, bundled)* | orange `?` in an orange-ringed circle | `[data-slot="authorAvatar"]` | fixed asset, **not an input** |
| `topic_category` | e.g. "Content Ops" | `[data-slot="authorName"]` | `textContent`, two spaces after the icon |
| `level` | all / easy / int. / adv. | `[data-slot="users"]` | `textContent` |
| `duration_minutes` | minutes, number only | `[data-slot="rating"]` | `textContent` |
| *(static, in the shell)* | the text `(mins)` | `[data-slot="ratingCount"]` | fixed text, **not an input** |
| `speaker_image` | the portrait | `.SpeakerCard-photo` | square photo slot of the speaker element, §1b |
| *(static, in the shell)* | `Join me in PRAGUE` | `.SpeakerCard-bar` | fixed copy, **not an input** |
| `href` | — | `a.ActorCard` | `href` attribute |
| `card_width` | — | `:root` | CSS var `--actor-card-width` |
| `desc_lines` | — | `:root` | CSS var `--actor-card-desc-lines` |
| `base_image` | — | composite base layer | not part of the card; see §3 |
| `output` | — | — | destination path for the finished PNG |
| `assets_root` | — | — | prefix for `company_logo` and `speaker_image` |

Visual positions, for orientation:

```
┌──────────────────────────────────────────────┐  ← base_image supplies the
│  ┌────────────────────────────────────────┐  │    rectangle this sits in
│  │  [company_logo]  speaker_name          │  │
│  │      40x40       position / company    │  │
│  │                                        │  │
│  │  presentation_description ............ │  │
│  │  ..................................... │  │
│  └────────────────────────────────────────┘  │
│  (?) topic_category  ★ duration (mins) 👥 level │
└──────────────────────────────────────────────┘
```

Note the footer order: `duration_minutes` and its static `(mins)` sit
together to the LEFT of `level`, because the stats row is
`flex-direction: row-reverse` — the ★ pair paints first, the 👥 stat last.

---

## 1a. Card geometry — measured, not derived

**The card has no fixed aspect ratio.** Width is free. Height is quantised
in 16px steps (the description's line-height) and is otherwise independent
of width — it moves only when a width change alters the description's wrap
count.

| Description state | Card height (px) |
|---|---|
| description element omitted entirely | **113.667** |
| `description: ""` (present but empty) | **121.667** |
| renders 1 line | **137.667** |
| renders 2 lines — live Apify default | **153.667** |
| renders 3 lines — Apify's authored max | **169.667** |

Verified at widths 300 / 400 / 700: all give 153.667 with a 2-line
description.

Three behaviours that are easy to get wrong:

- **An empty string is not the same as omitting the key.** The empty `<p>`
  still contributes its 8px `margin-top`, so you get 121.667, not 113.667.
- **`desc_lines` is a ceiling, not a target.** `4` renders identically to
  `3` when the text only fills three lines. Height follows *rendered* lines.
- **Cards in a flex row stretch to the tallest sibling.** Irrelevant when
  rendering one card into one rectangle; it will silently equalise a stack.

### Corner rounding

On the actor card, all uniform on every corner:

| Element | Radius |
|---|---|
| Card shell — outer `<a>`, `#1d1d1d` | **8px** (CSS) |
| Body panel — inner `#020202` block | **6px** (CSS) |
| Company logo (`company_logo`), 40×40 | **8px** (CSS) |
| Footer help icon (static), 20×20 | **100%** — full circle |
| Footer row, stat divider | 0 — square |

The 8/6 pair is concentric, not arbitrary: the shell has 2px of padding,
and `8 − 2 = 6`, which keeps the inner curve parallel to the outer one.
**Scale the two together or not at all.** The speaker element (§1b) uses
its own 15/11.3 radii — the same look scaled to its size.

---

## 1b. Placement — the two blocks

Every template carries **two** placeholders, and **both must be present**.
A template missing either one is rejected before any rendering starts.

| Block | Placeholder colour | Geometry | Holds |
|---|---|---|---|
| **actor card** | purple `#AE81FF` | measured per run | the rendered card, scaled from CSS width 400 |
| **speaker element** | green `#20A34E` | measured per run | the `.SpeakerCard` component, rendered 1:1 |

### The speaker element

The green block receives the shell's **`.SpeakerCard` component** — a
`#454545` (button-grey) shell that IS the element's border, rendered 1:1
at the block's own size. On the current template the block spans the two
central orange crosses vertically (y 345..681) and is centred on the
header's right title box (x centre 852.5). Inside it:

- the portrait as an **exact square**, inset 16px from the left, top and
  right edges — the photo slot is **262×262** on the current template
  (294-wide block − 16px border each side);
- the remaining grey strip below holds only `Join me in PRAGUE` (PRAGUE
  in the crosses' orange `#f5641f`), its ink spanning 80% of the strip's
  width, centred on both axes.

No outer ring. Nothing about the element's chrome or copy is an input;
only the photo changes per speaker.

### The template image is canon

**Whatever is on the template is the geometry for that generation.** The
skill re-measures both blocks every run from the colour masks — green
`G > R+40 and G > B+40`, purple `B > G+40 and R > G+20`, dilated 3px to
absorb anti-aliased edges. The current canon template carries no printed
readout panels; if a future template adds them (Canva's Width/Height/X/Y
card pasted inside a block), the printed values win after agreeing with
the mask within 6px — a disagreement halts with both numbers reported.

The current canon template is **machine-built** (see
`visual-templates/README.md`): baked starfield, blocks drawn at the locked
layout. It supersedes the operator's original Canva export. Re-exporting
over it means re-staging that recipe, not just dropping in a new PNG.

The eight `*_x / *_y / *_w / *_h` keys in the frontmatter are **outputs**.
The skill measures both blocks, writes the values back into the intake file
as a record of what it used, and reports them. Hand-typed values are
overwritten; the frontmatter is never consulted for geometry.

### The one check: can the card block hold a card?

The **card block is constrained**, because the card's height is quantised
(§1a) and cannot be stretched. Per run:

```
scale      = card_w / card_width         # card_width = the CSS render width
implied_h  = card_h / scale              #   == card_h * card_width / card_w
actual_h   = height the card really renders at, at card_width
require |implied_h - actual_h| <= 2px    # else HALT
```

Worked against the current template: `scale = 799/400 = 1.9975`,
`implied_h = 306.949/1.9975 = 153.667`, `actual_h = 153.667` → exact.
Passes. The same check catches a block drawn for a two-line card when the
intake's description would render one line.

On failure: report the block's ratio, the card's actual ratio, and the
height the block should be for the current text. **Never stretch,
letterbox or crop the card to fit** — halt and let the operator fix the
template.

### Render small, scale up (actor card only)

`card_width` is the CSS width the card is built at; `scale` is derived
from the block. **Do not set the CSS width to the block's pixel width** —
at ~800 CSS px the description stops wrapping to two lines and the card
collapses a height step, breaking the ratio and thinning the type below
production weight. Build at 400 and scale. The speaker element is the
opposite: it renders 1:1 at the block's own size, no scaling.

---

## 2. Field schema

Operator-typed values (`speaker_name`, `speaker_position`,
`speaker_company`, `topic_category`, `level`, `duration_minutes`, and the
description) arrive via the fenced inputs in "Input presentation details
here" and are transferred into the frontmatter by the assistant; on any
disagreement the fences win. The schema below describes the frontmatter
after that transfer.

### `base_image` — string, **required**

Path to the template PNG the render composites into.

- Must carry the two coloured placeholder blocks of §1b.
- Path is relative to this intake file, or absolute.
- The template's dimensions define the output's dimensions; the skill
  refuses to write an output whose size does not match.

**States:** valid path → render proceeds · missing file → hard fail · a
missing or ambiguous placeholder → hard fail, no partial write.

### `assets_root` — string, optional, default `""`

Directory prefix applied to `company_logo` and `speaker_image` only,
relative to this intake file. The empty default means the images live
beside the form, in the speaker folder.

### `company_logo` — string, **required**

The speaker's company logo, 40×40, upper-left of the card body.

- Rendered at 40×40, `border-radius: 8px`, 1px `#2a2a2a` border.
- Sits on a `#f3f3f3` plate, so transparent PNGs read as light-backed. A
  white-on-transparent logo will vanish.
- `object-fit: cover` — non-square art is centre-cropped, not squashed.
  Supply square, ideally 80×80 or larger to survive the render scale.

**States:** valid path → drawn · empty string → slot renders blank but the
40px box is still reserved · missing file → hard fail.

### `speaker_image` — string, **required**

The portrait filling the square photo slot of the speaker element (§1b).
Resolved against `assets_root`.

- **Accepted:** exactly square (1:1), PNG / JPG / JPEG, **at most
  800×800**. The ideal supply is **262×262** — the slot's exact rendered
  size on the current template — but any square within the cap works: the
  skill scales it into the slot losslessly (square onto square, nothing
  cropped) and archives a slot-sized copy as `speaker.png`.
- **Rejected, with the actual dimensions reported:** non-square, larger
  than 800×800, or any other format. The skill never crops, pads or
  reframes a photo — squaring it is the operator's call to make.
- The element's chrome (grey border, radii, `Join me in PRAGUE` bar) comes
  from the render shell, never from this asset.

**States:** valid square ≤800 → scaled and drawn · empty or omitted →
**rejected**; both blocks must be filled · missing file → hard fail ·
off-spec dimensions/format → **halt**, actual size reported, square
resubmit requested.

### The presentation description — **required**, lives in the BODY

The first fenced block after the frontmatter whose info string starts with
`presentation-description`. The number in the label is the budget:

    ```presentation-description-140-char-max
    How we cut lead research from six hours a week to twenty minutes.
    ```

- Inter 12px / 400 / `#a3a3a3`, clamped to `desc_lines` (2 lines).
- **Budget: 140 characters** — the two-line capacity of the description
  slot at the card's native CSS render width, operator-calibrated against
  real renders. The budget is parsed from the fence label itself, so
  retuning it for a different card width means renaming the fence.
- Newlines inside the fence are collapsed to spaces; leading/trailing
  whitespace is trimmed. One plain paragraph — no markdown.

**States:** present, ≤ budget → renders · **over budget → the form is
rejected** with the count, never silently cut · fence missing or empty →
rejected; a card without a description is a height step shorter and fails
the §1b ratio check anyway.

### `speaker_name` — string, **required**

- Inter 14px / 600 / `#dfdfdf`. **Title Case**, the form the speaker uses
  publicly. Single line; overflow ellipsises, never wraps or resizes.
- Budget: **30 characters**.

### `speaker_position` + `speaker_company` — strings, **required**

Joined by the skill into the monospace line `position / company` — a
spaced slash. The only non-Inter text on the card.

- IBM Plex Mono 12px / 500 / `#888888`. **All lowercase, kebab-case**,
  mirroring Apify's `owner/actor-name` slug pastiche.
- Supply the two halves separately, **without** any slash. The skill joins
  them with ` / ` (3 characters).
- Single line, ellipsises. Combined budget: **39 characters** including
  the joiner (monospace, so this one is exact).

**States:** each half matching `^[a-z0-9-]+$` → renders · uppercase,
spaces or a slash in either half → rendered as given but flagged
off-spec · either half empty → the line collapses.

### `topic_category` — string, **required**

- Inter 12px / 500 / `#a3a3a3`, right of the footer help icon with two
  spaces of clearance. Title Case. Single line, ellipsises.
- Budget: **33 characters** before it collides with the footer's
  right-hand group.

### `level` — string, **required**

- One of exactly: `all` · `easy` · `int.` · `adv.` — including the
  trailing dot on the two abbreviations. Renders right of the 👥 glyph.

**States:** one of the four → renders · anything else → renders as given,
flagged as off-vocabulary rather than failing.

### `duration_minutes` — string, **required**

- **Number only**, no unit — the card renders `10 (mins)`; the `(mins)`
  is static in the shell. Quote the value in YAML so `10` stays a string.

### The footer help icon — **not an input**

The 20×20 circle in the footer: an orange (`#f5641f`) question mark on the
shell colour, ringed by a 1px orange border (the ring is CSS in the render
shell; the `?` is the SVG). Identical on every generated card. Ships as
`internal/render/footer-help-icon.svg`; SVG, so it stays crisp at any
scale.

### `output` — string, optional

Destination for the finished PNG. Left empty, it defaults to
`final-output/<speaker-name>-final.png`.

**States:** path free → written · file exists → written under a `-<NN>`
suffix (lowest free number, first dupe = 01) and reported; **the existing
file is never touched.**

### `card_width` — integer, optional, default `400`

The CSS width the card renders at before scaling into the block. Changing
it changes every text budget — recheck them all if you move it.

### `desc_lines` — integer, optional, default `2`

Description clamp. `2` matches the live render. Above `3` the card grows
past the production silhouette.

### `href` — string, optional, default `""`

Sets the `href` on the wrapping `<a>`. Cosmetic for a static render.

---

## 3. Process — the queue

```
to-process/<speaker-folder>/     one folder per card:
    intake.md                      the filled form (this file)
    README.md                      what belongs in the folder (auto-written)
    company-logo.png               the two image assets, beside the form
    speaker.png
processed/<speaker-folder>/      the whole folder lands here on success
                                 (archive: form + assets)
final-output/<speaker>-final.png the finished render, delivered separately
```

1. **Scaffold**: tell the assistant *"new speaker Jana Novakova"* (the
   `new-speaker` skill) — it creates `to-process/jana-novakova/` with the
   form (name pre-filled) and the README. With no name it creates
   `new-speaker-<NN>/`, renamed to the kebab speaker name at processing.
2. **Fill**: the fenced inputs under "Input presentation details here",
   and drop the two images into the folder.
3. **Run**: tell the assistant *"process the queue"* — it takes every
   folder in `to-process/`, or just the ones you name. No batch cap.
4. Per form the skill: transfers the fenced inputs into the frontmatter
   and validates them → checks both image assets are present and the
   photo is an accepted square (§2) → measures both placeholders off
   `base_image` (colour mask; printed panels win when present, §1b) →
   card-ratio check → renders in a headless Chromium browser, all fonts
   local → verifies output dimensions and pixels by inspection → writes
   the PNG to `final-output/<speaker>-final.png` → **moves the whole
   folder to `processed/`**, geometry written back into the form.
5. **Missing images**: the run stops for that folder and asks —
   *"resubmit with the image(s) added, or generate now with a placeholder
   outline?"* In placeholder mode the missing slot renders as a dashed
   outline drawn INSIDE the block, so the real image pasted over it
   post-hoc covers it completely.
6. **Failures** stay in `to-process/`, nothing partial is written, the
   reason is printed, the rest of the batch continues.
7. **Nothing is ever overwritten** — a name already taken in `processed/`
   or `final-output/` lands under `<name>-<NN>` (lowest free number,
   first dupe = 01), the same NN on the archive folder and the PNG.
   Duplicate names are legitimate (a rebuilt card, a fresh start, a
   namesake) and are detected by folder/file names only — a repeated
   `speaker_name` in the frontmatter breaks nothing.

## 4. Batch discipline

Any number of forms per run. Partial completion is normal and reported —
succeeded forms move, failed forms stay.

Keep `base_image`, `card_width` and `desc_lines` identical across a batch.
Those three drive the card's silhouette, and mixing them produces a set
that does not read as a series.

---

## 5. Licence note

The card itself uses Inter and IBM Plex Mono — both SIL OFL 1.1, free for
commercial use, and both are what apify.com actually serves.

**GT Walsheim must never be added to this repo** — it is a commercial
Grilli Type font; apify.com's headings use it, this pipeline does not
need it, and no trial licence covers a shipped deliverable.
