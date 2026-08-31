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
speaker_image:      "speaker.png"
# speaker_focus:    [x, y]           # optional: centre of the crop, in pixels of
                                     # speaker_image. Written by the skill when it
                                     # reads a mark ("x"/scribble) off the photo.

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
level:              "int."          # all | easy | int. | adv.
duration_minutes:   "10"            # number only — the card renders 10 (mins)

# --- 6. placement -- DERIVED, DO NOT AUTHOR ----------------------------------
# The template image is canon. The skill measures both placeholders from
# base_image at generation time and writes the eight values back here as a
# record of what it used. Anything you type is overwritten. See §1b.
# Values below are from the last run, not a specification.

# actor card block — purple placeholder
card_x:             150
card_y:             769
card_w:             900
card_h:             346.2

# speaker photo block — green placeholder
speaker_x:          650
speaker_y:          318
speaker_w:          400
speaker_h:          400

# --- 7. render options -------------------------------------------------------
# output is optional. Default: processed/<this-file's-name>.png
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

Lowercase kebab-case again. Position and company render joined as
`position/company`; together they fit up to **39 characters** counting the
slash.

```speaker-company
acme-analytics
```

## Topic category

The track or theme, Title Case. Fits up to **33 characters**.

```topic-category
Content Ops
```

## Audience level

Exactly one of: `all` · `easy` · `int.` · `adv.` — keep the trailing dot on
the two abbreviations.

```level
int.
```

### Audience level notes
- All --  mostly conceptual, all levels can benefit
- Easy -- more basic, and specific enough that advanced might be bored
- Int. -- Intermediate, for those who know what an md file and a repo are
## Duration

Talk length in minutes — the number only, no unit; the card renders it as
`10 (mins)` automatically.

```duration-minutes
10
```

## Presentation description

The short blurb under the speaker's name on the card. The number in the
fence label is the enforced character budget: the measured two-line capacity
of the **actor card's description text slot** with the card at its native
CSS render width (`card_width: 400`) minus an 8% buffer — nothing to do with
the 400×400 speaker image; the matching number is coincidence. Exceeding the
budget rejects the form; the card is never silently truncated.

```presentation-description-82-char-max
How we cut lead research from six hours a week to twenty minutes.
```

## End of inputs

That's everything to type. Two things remain to **drop into this folder as
files** (see the folder's README): the company logo (`company-logo.png`) and
the speaker photo (`speaker.png`) — each either exactly 400×400, or any size
with a visible mark drawn at the intended centre of the shot. Everything
below this line is for the agents.

---

# Actor Card — input contract

This file is **input only**. All markup, CSS, fonts and render logic live in
the skill. Filling the fenced inputs above and pointing the skill at the file
is the entire authoring step; the assistant copies each fence into its
frontmatter variable (step 1 of processing) before anything renders.

---

## 1. Insertion map

Where every frontmatter key lands in the card. This is the lookup table — if a
value renders in the wrong place, the bug is one row of this table.

The card is Apify's `ActorStoreItem` used as a **shell for a speaker lineup**.
The slot names are Apify's; the meanings are yours.

| Frontmatter key | Means | Hook in the card | How it is applied |
|---|---|---|---|
| `company_logo` | speaker's company logo | `[data-slot="icon"]` | `src`, via `assets_root` |
| `speaker_name` | speaker's name | `[data-slot="title"]` | `textContent` |
| `speaker_position` + `speaker_company` | position / company | `[data-slot="slug"]` | joined with `/`, see §2 |
| *description fence (body)* | the talk blurb | `[data-slot="description"]` | `textContent`, from the fence, not frontmatter |
| *(static, bundled)* | Apify cross in a circle | `[data-slot="authorAvatar"]` | fixed asset, **not an input** |
| `topic_category` | e.g. "Content Ops" | `[data-slot="authorName"]` | `textContent` |
| `level` | all / easy / int. / adv. | `[data-slot="users"]` | `textContent` |
| `duration_minutes` | minutes, number only | `[data-slot="rating"]` | `textContent` |
| *(static, in the shell)* | the text `(mins)` | `[data-slot="ratingCount"]` | fixed text, **not an input** |
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
│  │      40x40       position/company      │  │
│  │                                        │  │
│  │  presentation_description ............ │  │
│  │  ..................................... │  │
│  └────────────────────────────────────────┘  │
│  (cross) topic_category  ★ duration (mins) 👥 level │
└──────────────────────────────────────────────┘
```

Note the footer order: `duration_minutes` and its static `(mins)` sit together
to the LEFT of `level`, because the stats row is `flex-direction: row-reverse`
— the ★ pair paints first, the 👥 stat last.

---

## 1a. Card geometry — measured, not derived

**The card has no fixed aspect ratio.** Width is free. Height is quantised in
16px steps (the description's line-height) and is otherwise independent of
width — it moves only when a width change alters the description's wrap count.

| Description state | Card height (px) |
|---|---|
| description element omitted entirely | **113.667** |
| `description: ""` (present but empty) | **121.667** |
| renders 1 line | **137.667** |
| renders 2 lines — live Apify default | **153.667** |
| renders 3 lines — Apify's authored max | **169.667** |

Verified at widths 300 / 400 / 700: all give 153.667 with a 2-line description.

Three behaviours that are easy to get wrong:

- **An empty string is not the same as omitting the key.** The empty `<p>` still
  contributes its 8px `margin-top`, so you get 121.667, not 113.667.
- **`desc_lines` is a ceiling, not a target.** `4` renders identically to `3`
  when the text only fills three lines. Height follows *rendered* lines.
- **Cards in a flex row stretch to the tallest sibling.** Irrelevant when
  rendering one card into one rectangle; it will silently equalise a stack.

### Placeholder sizing

For a single card, size the rectangle `W × 153.667` (2-line description) — round
to `W × 154`. A near-square placeholder cannot hold one card: filling a 425px
height would require scaling ~2.77×, forcing the CSS width to ~145px, at which
point the title ellipsises to about eight characters.

To fill a tall box, stack instead. Worked examples for a 400 × 425 box:

| Layout | Arithmetic |
|---|---|
| 3 cards, no description | `3 × 113.667 = 341`, two 42px gaps → **exactly 425** |
| 2 cards, 2-line description | `2 × 153.667 = 307.33`, centred, ~59px top and bottom |

### Corner rounding

All uniform on every corner.

| Element | Radius |
|---|---|
| Card shell — outer `<a>`, `#1d1d1d` | **8px** |
| Body panel — inner `#020202` block | **6px** |
| Company logo (`company_logo`), 40×40 | **8px** |
| Footer cross icon (static), 20×20 | **100%** — full circle |
| Footer row, stat divider | 0 — square |

The 8/6 pair is concentric, not arbitrary: the shell has 2px of padding, and
`8 − 2 = 6`, which keeps the inner curve parallel to the outer one. **Scale the
two together or not at all** — taking the shell to 12px without moving the body
to 10px pinches the gap at each corner.

---

## 1b. Placement — the two blocks

Every template carries **two** placeholders, and **both must be present**. A
template missing either one is rejected before any rendering starts.

| Block | Placeholder colour | Geometry | Holds |
|---|---|---|---|
| **actor card** | purple `#AE81FF` | measured per run | the rendered card |
| **speaker photo** | green `#20A34E` | measured per run | `speaker_image` |

### The template image is canon

**Whatever is on the template is the geometry for that generation.** There are
no canonical coordinates here and nothing to author: the skill re-measures the
blocks every run, so a tweak to the template in Canva (re-exported over the
same file) is picked up automatically. This demo ships exactly **one** visual
template at fixed dimensions — `visual-templates/speaker-teaser-linkedin.png`,
1200×1200 — and is not meant to support multiple templates.

The eight `*_x / *_y / *_w / *_h` keys in the frontmatter are **outputs**. The
skill measures both blocks, writes the values back into the intake file as a
record of what it used, and reports them. Hand-typed values are overwritten.

### The one check: can the card block hold a card?

The speaker photo block is unconstrained — any shape, any ratio; `object-fit: cover`
absorbs it. The **card block is not**, because the card's height is quantised
(§1a) and cannot be stretched to fit.

So, per run:

```
scale      = card_w / card_width         # card_width = the CSS render width
implied_h  = card_h / scale              #   == card_h * card_width / card_w
actual_h   = height the card really renders at, at card_width
require |implied_h - actual_h| <= 2px    # else HALT
```

Worked against the current template: `scale = 900/400 = 2.25`,
`implied_h = 346.2/2.25 = 153.867`, `actual_h = 153.667` → out by **0.2px**.
Passes.

This is deliberately **not** a fixed ratio constant. `actual_h` is whatever the
card genuinely renders at — 113.667 with no description, 137.667 at one line,
153.667 at two, 169.667 at three. So the same check also catches a block drawn
for a two-line card when the intake omits the description.

On failure: report the block's ratio, the card's actual ratio, and the height
the block should be for the current text. **Never stretch, letterbox or crop the
card to fit** — halt and let the operator fix the template.

### Render small, scale up

`card_width` is the CSS width the card is built at; `scale` is derived from the
block. **Do not set the CSS width to the block's pixel width.** At 900 CSS px
the description stops wrapping to two lines and the card collapses to 137.667
(measured), breaking the ratio and thinning the type below production weight.
Build at ~400 and scale.

### Corner radius does not match — by design

The purple placeholder's radius measures **≈60px**. The card at 2.25× carries
**8 × 2.25 = 18px** on the shell and `6 × 2.25 = 13.5px` on the body. The
placeholder is far rounder than what lands in it. That is fine — the placeholder
is covered — but it means **the template preview is not a preview of the
result**. Do not tune the card's radius to match the box.

### Detection is colour-keyed, not luminance-keyed

Both placeholders are strongly saturated fills on a near-black ground, so they
isolate on hue alone: `G > R+40 and G > B+40` for green, `B > G+40 and R > G+20`
for purple. This sidesteps the light-on-dark polarity assumption that the
earlier paper-app detector was built around, and which was the largest porting
risk on this project. No luminance threshold is involved.

### Self-describing templates

The colour-mask bounds are the primary measurement. If a template also carries
the Canva readout panel inside a placeholder — the white `Width / Height / X / Y`
card visible in `02` — the skill **reads those values off the image** and uses
them, since they carry Canva's sub-pixel precision that the mask cannot recover
from an anti-aliased edge. Either way the operator types nothing.

The read is cross-checked, never trusted alone:

1. Detect the placeholder's bounding box by colour mask (above).
2. Read the printed `Width / Height / X / Y` from the panel inside it.
3. **Require the two to agree** within a 6px tolerance — enough to absorb the
   anti-aliased edge, tight enough to catch a misread digit.
4. On disagreement, **halt and report both numbers.** Never silently prefer one.

Precedence: printed panel → colour-mask bounds. **The frontmatter is never
consulted** — it only receives the result. The skill reports which source it
used and the numbers it took.

If no panel is present, the mask bounds stand alone; dilate by 3px before taking
them, to recover the anti-aliased edge.

The readout panel is part of the placeholder and gets covered by the render, so
it never appears in the output.

---

## 2. Field schema

Operator-typed values (`speaker_name`, `speaker_position`, `speaker_company`,
`topic_category`, `level`, `duration_minutes`, and the description) arrive via the
fenced inputs in "Input presentation details here" and are transferred into
the frontmatter by the assistant; on any disagreement the fences win. The
schema below describes the frontmatter after that transfer.

### `base_image` — string, **required**

Path to the template PNG containing the placeholder the card is composited into.

- The template must carry the two coloured placeholder blocks of §1b
  (purple card block, green speaker block).
- Path is relative to this intake file, or absolute.
- Format is preserved end to end: a PNG in produces a PNG out.
- The template's dimensions define the output's dimensions. The skill will
  refuse to write an output whose size does not match the input.
- The skill locates the rectangle by pixel scan; it is not given coordinates.

**States:** valid path → render proceeds · missing file → hard fail · no
detectable rectangle → hard fail, no partial write · more than one rectangle →
hard fail, ambiguous target.

### `assets_root` — string, optional, default `"assets"`

Directory prefix applied to `company_logo` and `speaker_image` only. Relative to
this intake file. Set to `""` to treat those two keys as paths relative to the
intake file itself.

### `company_logo` — string, **required**

The speaker's company logo, 40×40, upper-left of the card body.

- Rendered at 40×40, `border-radius: 8px`, 1px `#2a2a2a` border.
- Sits on a `#f3f3f3` plate, so transparent PNGs read as light-backed, matching
  production. A logo with its own white lockup will look correct here; a
  white-on-transparent logo will vanish.
- `object-fit: cover` — non-square art is centre-cropped, not squashed. Supply
  square, or pre-crop.
- Supply at 80×80 or larger to survive the 2.25× render scale.

**States:** valid path → drawn · empty string → slot renders blank but the 40px
box is still reserved, so the layout does not shift · missing file → hard fail.

### The footer cross icon — **not an input**

The 20×20 circle in the footer is Apify's registration cross in the card's muted
grey (`#a3a3a3`) on the shell colour, and is **identical on every generated
card**. It ships with the skill as `assets/footer-cross-icon.svg` and cannot be
overridden from an intake file. SVG, so it stays crisp at any render scale.

### `speaker_name` — string, **required**

Upper of the two top strings.

- Inter 14px / 600 / `#dfdfdf`.
- **Title Case**, as a name would normally be written. No trailing punctuation.
- Single line. Overflow ellipsises — it does not wrap and does not resize.
- Budget: **30 characters** (measured average fit ~33, minus 8%). Long names
  ellipsis; prefer the form the speaker uses publicly over a full legal name.

**States:** ≤30 chars → renders whole · longer → truncated with `…` · empty →
the name line collapses and the position line rises to fill.

### `speaker_position` + `speaker_company` — strings, **required**

Joined by the skill into the monospace line: `position/company`. The only
non-Inter text on the card.

- IBM Plex Mono 12px / 500 / `#888888`.
- **All lowercase, kebab-case**, mirroring Apify's `owner/actor-name` slug form
  — `head-of-growth/acme-analytics`. This is the pastiche; keep it.
- Supply the two halves separately and **without** the slash. The skill joins
  them.
- Single line, ellipsises. Combined budget: **39 characters** including the
  slash (monospace, so this one is exact: 43 fit, minus 8%).

**States:** each half matching `^[a-z0-9-]+$` → renders · uppercase, spaces or a
slash in either half → rendered as given but off-spec, and the skill flags it ·
either half empty → the line collapses.

### The presentation description — **required**, lives in the BODY, not frontmatter

The first fenced block after the frontmatter whose info string starts with
`presentation-description`. The number in the label is the budget:

    ```presentation-description-82-char-max
    How we cut lead research from six hours a week to twenty minutes.
    ```

- Inter 12px / 400 / `#a3a3a3`, clamped to `desc_lines` (2 lines).
- **Budget: 82 characters** — the card's description text slot measured
  with the card at its native CSS render width (`card_width: 400` — not the
  400×400 speaker image): two-line capacity ~90 chars across mixed
  real-text samples, worst sample 87, minus an 8% buffer. The budget is parsed from the fence label itself,
  so retuning it for a different card width means renaming the fence.
- Newlines inside the fence are collapsed to spaces; leading/trailing
  whitespace is trimmed. Write one plain paragraph — no markdown.

**States:** present, ≤ budget → renders · **over budget → the form is
rejected** with the count, never silently cut mid-sentence · fence missing or
empty → rejected; the card without a description is 40px shorter and fails the
§1b ratio check anyway.

### `topic_category` — string, **required**

The track or category, e.g. `Content Ops`, `Engineering`, `Growth`.

- Inter 12px / 500 / `#a3a3a3`, immediately right of the footer cross icon.
- Title Case. Single line, ellipsises. Budget: **33 characters** (measured
  ~36, minus 8%) before it collides with the footer's right-hand group.

### `level` — string, **required**

Audience level, immediately right of the 👥 glyph.

- Inter 12px / 500 / `#a3a3a3`.
- One of: `all` · `easy` · `int.` · `adv.` — including the trailing full stops
  on the two abbreviations.

**States:** one of the four → renders · anything else → renders as given, and
the skill flags it as off-vocabulary rather than failing.

### `duration_minutes` — string, **required**

Talk length in minutes, right of the ★ glyph.

- Inter 12px / 500 / `#a3a3a3`.
- **Number only**, no unit — the card renders it as `10 (mins)`. The
  `(mins)` text sits in Apify's review-count slot and is static in the
  render shell, never an input. Quote the value in YAML so `10` stays a
  string.

### `speaker_image` — string, **required**

The portrait image filling the green block. Resolved against `assets_root`.

- Drawn at `speaker_w × speaker_h`, `object-fit: cover` — centre-cropped, never
  squashed.
- Supply at **720 × 960 or larger** (2× the block) so it stays crisp; the block
  is 3:4, so anything near that ratio crops cleanly.
- Unlike the two card images this one has no border and no radius applied. If
  you want rounded corners on it, say so — it is not in the card's CSS.

**States:** valid path → drawn · empty or omitted → **rejected**; both blocks
must be filled · missing file → hard fail · wrong aspect → centre-cropped, which
may cut faces or text, so pre-crop anything composition-sensitive.

### `output` — string, **required**

Destination for the finished PNG, relative to this intake file or absolute.

**States:** path free → written · **file exists → the skill refuses and
reports; it never overwrites.** Change `output` or move the old file.

### `card_width` — integer, optional, default `384`

Card width in px, matching the live homepage grid. Changing it changes how much
text fits before ellipsis — recheck `speaker_name` and the position/company
line if you move it.

### `desc_lines` — integer, optional, default `2`

Description clamp. `2` matches the live render. `3` is what Apify's authored CSS
says. Above 3 the card grows past the production silhouette.

### `href` — string, optional, default `""`

Sets the `href` on the wrapping `<a>`. Cosmetic for a static render; carried so
the same contract works if the card is ever emitted as live HTML.

---

## 3. Process — the queue

```
to-process/<speaker-folder>/     one folder per card:
    intake.md                      the filled form (this file)
    README.md                      what belongs in the folder (auto-written)
    company-logo.png               the two image assets, beside the form
    speaker.png
processed/<speaker-folder>/      the whole folder lands here on success,
                                 finished PNG inside it
```

1. **Scaffold**: tell the assistant *"new speaker Jana Novakova"* (the
   `new-speaker` skill) — it creates
   `to-process/jana-novakova/` with the form (name pre-filled) and the README.
   With no name it creates `new-speaker-<N>/`, and the folder is renamed to
   the kebab speaker name from the form at processing time.
2. **Fill**: the fenced inputs under "Input presentation details here", and
   drop the two images into the folder. The assistant transfers the fence
   values into the frontmatter at processing time.
3. **Run**: tell the assistant *"process the queue"* — it takes every folder
   in `to-process/`, or just the ones you name. No batch cap.
4. Per form the skill: transfers the fenced inputs into the frontmatter
   and validates them → checks both image assets
   are present (see below) → measures both placeholders off `base_image`
   (printed panel first, colour mask second, §1b) → card-ratio check →
   renders at `card_width` in headless Chrome, all fonts local → scales and
   composites → verifies output dimensions and pixels → writes the PNG into
   the folder → **moves the whole folder to `processed/`**, geometry written
   back into the form.
5. **Missing images**: the run stops for that folder and asks — *"resubmit
   with the image(s) added, or generate now with a placeholder outline?"* In
   placeholder mode the missing slot renders as a dashed outline drawn
   INSIDE the block (logo slot keeps the card's 8px corner rounding), so the
   real image pasted over it post-hoc covers it completely.
6. **Failures** stay in `to-process/`, nothing partial is written, the reason
   is printed, the rest of the batch continues.
7. **Nothing is ever overwritten** — a `processed/` name collision refuses
   that folder.

### Image sizing — two accepted forms

For `speaker.png` (and `company-logo.png` alike, PNG or JPG/JPEG):

- **Exactly 400×400** → used as-is; a correctly sized image also fully covers
  a placeholder outline, which sits inside those bounds; or
- **any size, with a visible mark at the intended centre** — an "x", a
  scribble, any clear annotation drawn on the image. The skill reads the
  mark's position, records it as `speaker_focus: [x, y]`, and the generator
  crops to the block's proportions around that point. The original file is
  untouched.

Odd-sized and unmarked → plain centre crop (`object-fit: cover`).

## 4. Batch discipline

Any number of forms per run. Partial completion is normal and reported —
succeeded forms move, failed forms stay.

Keep `base_image`, `card_width` and `desc_lines` identical across a batch.
Those three drive the card's silhouette, and mixing them produces a set that
does not read as a series.

---

## 5. Licence note

The card itself uses Inter and IBM Plex Mono — both SIL OFL 1.1, free for
commercial use, and both are what apify.com actually serves.

If a template also sets headings in **GT Walsheim Trial**, that face is licensed
for **mockups and testing only**. It must not reach a shipped commercial
deliverable without a retail licence from grillitype.com, and it must not be
redistributed — no public repos, no handing the files to a client.
