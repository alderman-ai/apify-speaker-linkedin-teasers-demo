---
name: apify-speaker-card
description: >
  Mass-produce Apify-styled speaker teaser images: renders a pixel-faithful
  Apify actor card (repurposed as a speaker card) and a card-style speaker
  portrait element into the canon template, one finished PNG per speaker.
  Use when the operator picks line 1 of the session menu ("see the workflow
  in action" / "show me the demo" — the Demo run section), says "process
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
generated-images/<speaker>-final.png   the finished render, delivered separately
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

## Demo run (line 1 of the session menu — "show me the demo")

The bundled demo speaker lives in `_internal/demo-speaker/`: a complete
speaker folder (filled `intake.md`, `README.md`, `company-logo.png`,
`speaker.png`) for the repo author, Alex Alderman. It is input, never a
working folder — **never process or edit it in place.**

1. Say in one line what is about to happen: the bundled demo speaker is
   copied into the queue, then goes through exactly the pipeline a real
   speaker goes through.
2. **Copy** (never move) the whole folder to `to-process/alex-alderman/`,
   applying the `new-speaker` duplicate rule: if `alex-alderman` is already
   taken in `to-process/`, `processed/` or `generated-images/` — it is in
   the shipped repo, where the author's own card sits as the worked
   example — the copy becomes `to-process/alex-alderman-<NN>/`, lowest
   free number, first dupe = 01. Say which name it got and why (nothing is
   ever overwritten).
3. Process **that one folder** with the procedure below, narrating each
   stage in a single line as you pass it: form validated → both assets
   checked → geometry measured off the template → card-ratio check →
   rendered headless → verified by inspection → delivered.
4. Report as in step 7, show the finished PNG's path, and offer the next
   step: the same for a real speaker — name, role, company, blurb, topic,
   minutes and two square images, all given here in chat (the
   `new-speaker` skill). Line 1 of the menu asks no further questions
   before running; the offer at the end is the only prompt.

## Processing ("process the queue")

For each folder in `to-process/` (or the ones named), in order:

### 1 · Parse and validate `intake.md`

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

### 3 · Geometry — the template image is canon

Read `base_image` (the Read tool shows its true pixel size — use that as
page size, don't assume 1200×1200). Two placeholders: **purple `#AE81FF` =
actor card block, green `#20A34E` = speaker element block**; both must
exist or the template is rejected.

Measure each block's bounding box by colour mask — green
`G > R+40 and G > B+40`, purple `B > G+40 and R > G+20` — dilated 3px to
recover anti-aliased edges. The current canon template is machine-built
and carries **no printed readout panels**; if a future template shows them
(Canva's Width/Height/X/Y card pasted inside a block), read them off the
image and require agreement with the mask within 6px — on disagreement,
halt and report both numbers. Precedence: printed panel → colour mask.
The frontmatter is never consulted for geometry.

**The green block hosts the speaker element, not a bare photo**: the
shell's `.SpeakerCard` component, rendered 1:1 at the block's own size —
a `#454545` (button-grey) shell that IS the element's border. On the
current template the block spans the two central orange crosses
vertically (y 345..681) and is centred on the header's right title box
(x centre 852.5). Inside: the portrait as an exact square inset 16px from
the left, top and right edges (slot 262×262), and below it the grey strip
holding only `Join me in PRAGUE` (PRAGUE in the crosses' orange
`#f5641f`) at 80% of the strip's width, centred on both axes. No outer
ring — that stays exclusive to the actor card's hover state.

Write the eight values back into the frontmatter (`card_x/y/w/h`,
`speaker_x/y/w/h`) — they are outputs, never inputs.

### 4 · The card-ratio check

```
scale     = card_w / card_width          (card_width default 400)
implied_h = card_h / scale
```

`implied_h` must land within **±2px** of the card's real height for this
form's text — the quantised table: no desc 113.667 · empty-string desc
121.667 · 1 line 137.667 · 2 lines 153.667 · 3 lines 169.667. (A ≤100-char
description at width 400 renders 2 lines when over ~70 chars, 1 line under.)
On failure, report the height the block should be (`actual_h × scale`) and
halt that folder. **Never stretch, letterbox or crop the card to fit.**

Worked against the current template: `scale = 799/400 = 1.9975`,
`implied_h = 306.949/1.9975 = 153.667` → exact.

### 5 · Fill the shell and render

Copy `_internal/render/shell.html` to `_internal/render/_run-<name>.html`
(same folder, so `../fonts/fonts.css` resolves — fonts are local on
purpose; the render must never touch a network). Replace every `{{TOKEN}}`:

| token | value |
|---|---|
| `PAGE_W/H` | template pixel size |
| `CARD_X/Y/W/H`, `SPK_X/Y/W/H` | the geometry from step 3 |
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
covered (no purple or green anywhere — check edges and corners, not just
centres; a centre-only check once passed while most of a block showed);
untouched template areas identical; text right; footer reads
`(?) topic · ★ N (mins) · 👥 For All Levels` with the `?` icon orange-ringed and two
spaces before the topic; hover ring visible around the actor card body;
the speaker element shows a square portrait framed by the `#454545` shell
(16px border on its left/top/right) over a centred `Join me in PRAGUE`
(PRAGUE orange); starfield visible below the actor card. Anything off →
halt that folder, delete the bad screenshot, report.

Then: move the PNG to `generated-images/<kebab-name>-final.png`, delete the
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

Per folder: OK/FAIL, output path, the eight geometry values and their
source (panel vs visual), the implied-vs-actual card heights, every
warning. Then totals. One bad folder never stops the rest.

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
  ` / ` joiner) · description 100 (from the fence label,
  measured against real two-line renders at width 400 on 2026-09-03) ·
  topic 26 (shortened from 33 by the fixed level text). `level` is
  static since template v3: `For All Levels`, never an input.
- The speaker photo is accepted as any exact square PNG/JPG/JPEG up to
  800×800 and scaled to the slot (262×262 currently); it is never
  cropped or reframed, and the element's chrome and `Join me in PRAGUE`
  copy live in the shell, not in any asset.
- The canon template is machine-built (baked gradient starfield, drawn
  blocks — recipe in `_internal/core-templates-please-dont-touch/README.md`)
  and supersedes the
  operator's original Canva export.
- GT Walsheim is not part of the render and must never be added to this
  repo.
