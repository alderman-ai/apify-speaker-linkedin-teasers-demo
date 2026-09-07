---
name: apify-speaker-card
description: >
  Mass-produce Apify-styled speaker teaser images: renders a pixel-faithful
  Apify actor card (repurposed as a speaker card) and a card-style speaker
  portrait element into the canon template, one finished PNG per speaker.
  Use when the operator picks line 1 of the session menu ("make your own
  speaker card" / "show me the demo" — the Line 1 section), says "process
  the intake forms", "process the queue", "generate the speaker cards",
  "new speaker <name>", or drops folders into to-process/.
---

# Apify speaker card generator

One speaker folder in → one finished template-sized PNG out. There is **no
build script and no dependency beyond a Chromium-based browser** (Chrome, or
the Edge that ships with Windows) — you are the engine. Everything is done
with reading, writing, one headless-browser command, and your own eyes.
`_internal/core-templates-please-dont-touch/intake-template.md` is the input
contract (`demo-and-more-help/filling-in-the-form/intake-template-completed-example.md` is a
filled reference copy); consult it for field rules rather than improvising.

## The queue

```
to-process/<speaker>/              intake.md + README.md + company-logo + speaker images
processed/<speaker>/               the whole folder moves here on success (archive)
generated-images/<speaker>.png     the finished render, delivered separately
```

A folder is always in exactly one queue. Failures stay put with the reason
stated; nothing partial is ever written; nothing is ever overwritten — a
name already taken in processed/ or generated-images/ gets a `-<NN>` suffix
(lowest free number, first dupe = 01), never a refusal and never a
replacement.

## Scaffolding

Handled by the sibling **`new-speaker`** skill (asks for the name, then
collects every other field and the two images in chat and writes them into
the form; declined name → `new-speaker-<NN>`, lowest free number,
zero-padded). If the operator asks this skill to scaffold, invoke that one.

## Line 1 of the session menu — "make your own speaker card"

Line 1 is the whole demo: the visitor makes a card for a speaker of their
choosing, in about ten minutes, and sees exactly where the pipeline's
inputs stop and its fixed parts begin. Nothing about geometry is ever
asked or explained during it — the template's block positions are
constants (step 3) and the visitor only supplies text and two images.

1. **Ask for the speaker's name** — one question. Then scaffold the folder
   with the `new-speaker` skill (steps 1–4 there): `to-process/<name>/`
   holding a fresh copy of the intake form (name pre-filled) and the
   README. Say the folder's path.
2. **Offer the fork** — one question, two options, verbatim in spirit:
   - **Fill it in yourself.** *"Open `<absolute path>/intake.md`, type in
     each labelled fence under 'Input presentation details here', drop the
     two square images into the folder as `company-logo.png` and
     `speaker.png`, and tell me 'done' when it's ready. I'll check it
     and ask you here for anything that's missing."*
   - **Give me the answers here.** *"Tell me the role, company, topic,
     minutes, blurb and the two image paths, in any order, and I'll fill
     the form for you."* → continue with `new-speaker` steps 5–7.
3. **Self-filled path — the gate.** On "done" (or any message saying the
   form is ready), run step 1 and step 2 of the processing procedure below
   on that folder. Step 1 begins with the **frontmatter check**: anything
   the visitor changed in the frontmatter is reverted and mentioned in a
   friendly line. Then:
   - **Everything present and compliant** → continue straight to 4.
   - **Something missing** — an empty or `[type here]` fence, a missing
     image — → ask for exactly those things, inline, in one message; as
     each answer arrives, write it into the form yourself (fence and
     frontmatter key) or copy the image in, exactly as `new-speaker`
     step 6 does. Do not send them back to the file.
   - **Something non-compliant** — a blurb over the fence's budget (give
     the count), a non-square or oversized photo, position/company not
     kebab-case — → say what and why, ask for a replacement inline, and
     write the replacement in yourself. Never trim, crop or reword a
     value on your own; the kebab-case offer used on a "yes" is the one
     exception.
   Repeat until the form is complete, then continue.
4. **Process that one folder** with the procedure below, narrating each
   stage in a single line as you pass it: form validated → both assets
   checked → fixed geometry loaded → card-ratio sanity check → rendered
   headless → verified by inspection → delivered.
5. Report as in step 7 and show the finished PNG's path.

The bundled demo speaker in `_internal/demo-speaker/` (the repo author's
complete, filled folder) is the **worked example** of a finished form, not
an input to line 1. Point a visitor at it when they ask what a filled form
looks like. **Never process or edit it in place**; if someone explicitly
asks to render it, copy it into `to-process/` under the `new-speaker`
duplicate rule (`alex-alderman-<NN>`) and process the copy.

## Processing ("process the queue")

For each folder in `to-process/` (or the ones named), in order:

### 1 · Parse and validate `intake.md`

- **Frontmatter check — first, before anything is transferred.** Compare
  the form's frontmatter against the current
  `_internal/core-templates-please-dont-touch/intake-template.md`, key by
  key. Every key except the five the fences feed (`speaker_name`,
  `speaker_position`, `speaker_company`, `topic_category`,
  `duration_minutes`; the description has no key) must be byte-identical
  to the template: `version`, `versioned_at`, `base_image`,
  `assets_root`, `company_logo`, `speaker_image`, `level`, the eight
  geometry keys, `output`, `card_width`, `desc_lines`, `href`, and every
  comment line. Any difference — an edited number, a deleted comment, a
  changed path — is **reverted to the template's text**, then mentioned
  kindly in one line (*"I put the frontmatter back the way the template
  has it — those values are fixed for the template, and nothing you typed
  in the fences was touched."*). Never halt on it, never keep an edited
  value, never argue.
- Operator values live in labelled body fences: `speaker-name`,
  `speaker-position`, `speaker-company`, `topic-category`,
  `duration-minutes`, and the first ```presentation-description-NN-char-max
  fence. The `new-speaker` skill normally wrote them there from chat,
  mirrored into the frontmatter already; a hand-filled form has fences
  only. **First: transfer each fence's trimmed contents into its matching
  frontmatter variable** — fences win over any hand-typed frontmatter; a
  fence that is empty or still reads `[type here]` counts as not provided.
- The fence's NN **is** the description budget. Over budget → **reject the
  form with the count; never trim the text yourself.** Collapse internal
  newlines to spaces.
- Required: base_image, company_logo, speaker_image, speaker_name,
  speaker_position, speaker_company, topic_category, duration_minutes.
  `level` is fixed at `For All Levels` since template v3 — never an input;
  fill `{{LEVEL}}` from the frontmatter value as is.
- Warn (don't fail): position/company not lowercase-kebab.

### 2 · Check the two image assets

Missing logo or photo → stop that folder and ask the operator:
*"N asset(s) missing — resubmit the folder with the image(s) added, or
generate now with a dashed placeholder outline you can drop the image onto
post-hoc?"* Only on an explicit "placeholder" answer, render that slot as an
inline-SVG data URI: `#0d0d0d` fill, `#666` dashed border (12 9) inset 4px
**inside** the block bounds, thin `#3a3a3a` corner-to-corner diagonals, a
centred `#9a9a9a` label ("logo" / "speaker image"); the logo variant gets
`rx=12` so it matches the card's 8px corner clip at 40px. Never invent or
fetch a real stand-in image.

**Speaker photo acceptance** — ask for the ideal, accept the reasonable:

- **Ideal supply**: the photo slot's exact rendered size — **262×262** on
  the current template.
- **Accepted**: any **exactly square (1:1)** PNG / JPG / JPEG up to
  **800×800**. Scale an accepted square to the slot size with
  high-quality resampling and archive the result beside the form as
  `speaker.png` (keep the operator's original file untouched); the render
  uses the slot-sized copy. Square onto square — nothing is ever cropped.
- **Halt** that folder, reporting the actual dimensions/format, for
  anything non-square, larger than 800×800, or in another format. Never
  crop, pad or reframe a photo — squaring it is the operator's decision.

The company logo stays flexible: square, ideally 80×80 or larger
(`object-fit: cover` centre-crops a non-square logo).

### 3 · Geometry — fixed constants, read from the form

Read `base_image` (the Read tool shows its true pixel size — use that as
page size; it is 1200×1200 for template v4). **Do not measure the image.**
The two coloured blocks are how the template was designed; where they sit
was measured once when template v4 was built and is recorded as a constant
in §6 of the intake form — the eight keys `card_x/y/w/h` and
`speaker_x/y/w/h`. Read them from the frontmatter verbatim:

| block | placeholder colour | fixed geometry (template v4) |
|---|---|---|
| actor card | purple `#AE81FF` | 799×307 @ (201, 748) |
| speaker element | green `#20A34E` | 294×336 @ (706, 345) |

They cannot differ from the template's by the time you get here — the
frontmatter check in step 1 already reverted any hand edit — so read them
and go. Never derive them from colour masks:
the Apify logo in the header shares the placeholder green and a company
logo can share the purple, so any mask read is unreliable by construction.

**The green block hosts the speaker element, not a bare photo**: the
shell's `.SpeakerCard` component, rendered 1:1 at the block's own size —
a `#454545` (button-grey) shell that IS the element's border. On template
v4 the block spans the two central orange crosses vertically and is
centred on the header's right title box. Inside: the portrait as an exact
square inset 16px from the left, top and right edges (slot 262×262), and
below it the grey strip holding only `Join me in PRAGUE` (PRAGUE in the
crosses' orange `#f5641f`) at 80% of the strip's width, centred on both
axes. No outer ring — that stays exclusive to the actor card's hover state.

### 4 · The card-ratio sanity check

```
scale     = card_w / card_width          (card_width default 400)
implied_h = card_h / scale
```

`implied_h` must land within **±2px** of the card's real height for this
form's text — the quantised table: no desc 113.667 · empty-string desc
121.667 · 1 line 137.667 · 2 lines 153.667 · 3 lines 169.667. (A ≤115-char
description at width 400 renders 2 lines when over ~70 chars, 1 line under.)
With fixed geometry this only fails when a form's `desc_lines` or
`card_width` has been changed, or a blurb renders one line; on failure,
report the height the block would need (`actual_h × scale`) and halt that
folder. **Never stretch, letterbox or crop the card to fit.**

Worked against template v4: `scale = 799/400 = 1.9975`,
`implied_h = 307/1.9975 = 153.69` → within tolerance of 153.667.

### 5 · Fill the shell and render

Copy `_internal/render/shell.html` to `_internal/render/_run-<name>.html`
(same folder, so `../fonts/fonts.css` resolves — fonts are local on
purpose; the render must never touch a network). Replace every `{{TOKEN}}`:

| token | value |
|---|---|
| `PAGE_W/H` | template pixel size |
| `CARD_X/Y/W/H`, `SPK_X/Y/W/H` | the fixed geometry from step 3, verbatim |
| `CARD_CSS_WIDTH` | `card_width` (default 400) |
| `CARD_SCALE` | `card_w / card_width`, 6 decimals |
| `SPK_SCALE` | `1.000000` — the speaker element renders 1:1 at the block's own output size |
| `SPK_CSS_W`, `SPK_CSS_H` | `spk_w`, `spk_h` verbatim (1:1) |
| `DESC_LINES` | `desc_lines` (default 2) |
| `BASE_IMAGE_URI`, `COMPANY_LOGO_URI`, `SPEAKER_IMAGE_URI` | `file:///` absolute URLs (or the placeholder data URI) |
| `HELP_ICON_URI` | `file:///` URL of `_internal/render/footer-help-icon.svg` |
| `SPEAKER_NAME`, `POSITION_COMPANY` (join with a spaced ` / `), `DESCRIPTION`, `TOPIC_CATEGORY`, `LEVEL` (the fixed `For All Levels`), `DURATION_MINUTES` (number only — the static `(mins)` is baked into the shell) | HTML-escaped text |

Verify no `{{` remains. Then screenshot — **into the temp dir first; the
browser cannot write into Desktop folders** (observed: Access denied):

```
chrome --headless=new --disable-gpu --hide-scrollbars --force-color-profile=srgb
  --force-device-scale-factor=1 --virtual-time-budget=4000
  --window-size=<PAGE_W>,<PAGE_H> --screenshot=<%TEMP% path> <file:/// url of _run page>
```

Any Chromium-based browser runs these exact flags. Find one, in order: the
CHROME env var · Chrome at
`C:\Program Files\Google\Chrome\Application\chrome.exe` · Edge (preinstalled
on Windows) at `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe` ·
`google-chrome` / `chromium` / `msedge` on PATH. Never download a browser —
one of these is already on the machine.

### 6 · Verify with your eyes, then deliver

Read the screenshot. Confirm: exact template dimensions; both blocks fully
covered — **look at each block's four edges and corners** for any
placeholder purple or green peeking out (a centre-only check once passed
while most of a block showed). Judge this at the block boundaries only:
purple or green *inside* a rendered element is content, not a leak — the
Apify header logo is placeholder-green and a company logo may well be
placeholder-purple, and neither is a fault. Untouched template areas
identical; text right; footer reads
`(?) topic · ★ N (mins) · 👥 For All Levels` with the `?` icon orange-ringed and two
spaces before the topic; hover ring visible around the actor card body;
the speaker element shows a square portrait framed by the `#454545` shell
(16px border on its left/top/right) over a centred `Join me in PRAGUE`
(PRAGUE orange); starfield visible below the actor card. Anything off →
halt that folder, delete the bad screenshot, report.

Then: move the PNG to `generated-images/<kebab-name>.png`, delete the
`_run-*.html`, and move the whole folder to `processed/<kebab-name>/`
(renaming `new-speaker-<NN>` to the kebab speaker name from the form).
**Duplicate names suffix, never refuse**: if `<kebab-name>` is already
taken in `processed/` or `generated-images/`, use `<kebab-name>-<NN>` — the
lowest number (zero-padded, first dupe = 01) free in BOTH locations, the
same NN on the folder and the PNG so archive and render stay paired.
Duplicates are detected by folder/file names only — never by frontmatter
`speaker_name`, which may legitimately repeat (a rebuilt card, a restart
after text edits, a namesake).

### 7 · Report

Per folder: OK/FAIL, output path, the eight fixed geometry values as read
from the form, the implied-vs-actual card heights, every warning. Then
totals. One bad folder never stops the rest.

## Fixed facts (do not rediscover)

- The card CSS in the shell is a pixel-verified reproduction of apify.com's
  ActorStoreItem (400 × 153.667 at width 400). Do not "clean up" its
  oddities — the 2px-padding "border", the `display:inline` icon, the
  row-reverse stats, the permanent hover ring are all deliberate.
- Render the actor card small and scale up — **never set CARD_CSS_WIDTH to
  the block's pixel width**; at block width the description rewraps and
  the card collapses a height step. The speaker element is the opposite:
  it renders 1:1 at the block's own size.
- The footer circle icon (orange `?`, 1px orange ring) is static on every
  card; never an input. The ring is the avatar's CSS border in the shell;
  the `?` is `_internal/render/footer-help-icon.svg`.
- Text budgets: name 30 · position/company 39 (including the 3-char
  ` / ` joiner) · description 115 (from the fence label,
  measured against real two-line renders at width 400 on 2026-09-03) ·
  topic 26 (shortened from 33 by the fixed level text). `level` is
  static since template v3: `For All Levels`, never an input.
- The speaker photo is accepted as any exact square PNG/JPG/JPEG up to
  800×800 and scaled to the slot (262×262 currently); it is never
  cropped or reframed, and the element's chrome and `Join me in PRAGUE`
  copy live in the shell, not in any asset.
- The canon template is machine-built (baked gradient starfield, drawn
  blocks — recipe in `_internal/core-templates-please-dont-touch/README.md`)
  and supersedes the operator's original Canva export. **Template v4 is
  final for this demo**; its block geometry is a constant in the intake
  form, measured once at build time (exact-colour footprint, whole pixels,
  render-verified 2026-09-07), never re-measured per run.
- GT Walsheim is not part of the render and must never be added to this
  repo.
