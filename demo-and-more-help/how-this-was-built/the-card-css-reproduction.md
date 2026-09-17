# The card: a verified CSS reproduction, oddities included

The actor card is apify.com's `ActorStoreItem`, rebuilt from scratch in plain
HTML and CSS in [`_internal/render/shell.html`](../../_internal/render/shell.html)
and **verified box-for-box against the live site to 0.001px**. Honesty note:
that verification was a one-time measurement at build time (the extracted
boxes compared against the live page's); no fixture or test ships in this
repo, and the only check that runs here is the generator's verify-by-
inspection of each finished PNG. No framework, no JavaScript, no design
export — one `<style>` block and a static DOM that the generator fills and
screenshots with headless Chromium.

## The measurements

At `card_width` 400 the card renders **400 × 153.667**. The shell's own
comment says "384x153.667 at native width": 384 is the outer width the live
card had in apify.com's grid when it was extracted, and the CSS itself sets
no fixed width — `.ActorCard{ width:var(--card-css-width) }`, filled from the
form's `card_width`, default 400 — so this pipeline renders the same box
model 16px wider (outer `padding:2px`, then `.ActorCard-body{ padding:16px }`,
leaving 364px of text width at 400). The height is the same at both, and at
300 and 700 (the intake template's check), because it depends on the
description's line count, not the width. Width is free; height is not
continuous. It quantises in 16px steps (the description's line-height):

| description state | height |
|---|---|
| element omitted entirely | 113.667 |
| `description: ""` (present but empty) | 121.667 |
| 1 rendered line | 137.667 |
| 2 rendered lines (the live Apify default) | 153.667 |
| 3 rendered lines (Apify's authored max) | 169.667 |

The empty-string row is the trap: an empty `<p>` is still a box, so it still
contributes its `margin:8px 0 0` — 121.667, not 113.667. Rendered lines govern,
not `-webkit-line-clamp`, which is a ceiling. That table is what the pipeline's
ratio check tests `card_h / scale` against, with a ±2px tolerance; a mismatch
halts the run rather than stretching the card.

## Why the oddities stay

The fractional heights are not rounding noise — they fall out of real
line-height arithmetic on a header row whose image is inline, not block:

```css
.ActorCard-icon{
  display:inline;                      /* deliberately not block */
  width:40px; height:40px;
```

An inline replaced element sits on the text baseline, so the 18px/28px root
metrics push the header row to 47.667px instead of a tidy 40px. Change that one
declaration and every height in the table moves. Same for the hover ring, which
is painted permanently and on the *outside*, so the edge reads ring / `#1d1d1d`
shell / body:

```css
  box-shadow:0 0 0 2px var(--border);
```

There is no `border` property anywhere on the card: the `#1d1d1d` shell plus
2px of padding *is* the border. The stats row is `flex-direction:row-reverse`,
so DOM order `[level][divider][duration]` paints as duration `(mins)` | divider
| level. `.ActorCard-slug` is the `position / company` line — IBM Plex Mono
12px/16px, weight 500, `#888888` — with a real two-line clamp and a real
ellipsis above it on the title. The footer chrome is fixed on every card: `For
All Levels`, the static
[`footer-help-icon.svg`](../../_internal/render/footer-help-icon.svg) orange `?`
whose 1px orange ring is the avatar `border` in CSS, and the literal `(mins)`
suffix baked into the markup. The repo rule is blunt: **do not tidy them.**

## The speaker element

`.SpeakerCard` renders 1:1 (`SPK_SCALE` is always `1.000000`) — a `#454545`
shell that is itself the border, a square photo inset 16px left/top/right, and
`Join me in PRAGUE` centred in the grey strip below, PRAGUE in `#f5641f`. Chrome
and copy are fixed; only the photo changes per speaker.

Every double-brace token the generator fills: `PAGE_W`, `PAGE_H`,
`BASE_IMAGE_URI`, `SPK_X`, `SPK_Y`, `SPK_W`, `SPK_H`, `SPK_SCALE`, `SPK_CSS_W`,
`SPK_CSS_H`, `SPEAKER_IMAGE_URI`, `CARD_X`, `CARD_Y`, `CARD_W`, `CARD_H`,
`CARD_SCALE`, `CARD_CSS_WIDTH`, `DESC_LINES`, `COMPANY_LOGO_URI`,
`HELP_ICON_URI`, `SPEAKER_NAME`, `POSITION_COMPANY`, `DESCRIPTION`,
`TOPIC_CATEGORY`, `LEVEL`, `DURATION_MINUTES`.

Fonts are local woff2 in [`_internal/fonts/`](../../_internal/fonts/fonts.css) —
Inter 400/500/600 and IBM Plex Mono 500, both SIL OFL — so a render never
touches a network and can never silently fall back to a system face. GT
Walsheim, apify.com's heading face, is deliberately absent: it is a commercial
Grilli Type font, and the card does not use it.

Unofficial, non-commercial homage; Apify's name, logo and visual design belong
to Apify.

Back to the [index](INDEX.md).
