# The render step: headless Chromium, local fonts, no network

There is no renderer in this repo. There is a static HTML page, a browser
already on the machine, and one screenshot.

## Fill a copy, never the original

The assistant copies
[`_internal/render/shell.html`](../../_internal/render/shell.html) to
`_internal/render/_run-<name>.html` — **the same folder**, so the shell's
relative `../fonts/fonts.css` still resolves — and replaces every
double-brace token: page size, the eight fixed geometry values read verbatim
from the intake form, `CARD_SCALE`, `SPK_SCALE`, the escaped text, and
`file:///` URLs for the template, the logo, the portrait and the footer icon,
then checks that no `{{` survives. `_run-*.html` is
gitignored and deleted after capture.

## The capture

```
chrome --headless=new --disable-gpu --hide-scrollbars --force-color-profile=srgb
  --force-device-scale-factor=1 --virtual-time-budget=4000
  --window-size=<PAGE_W>,<PAGE_H> --screenshot=<%TEMP% path> <file:/// url of _run page>
```

- `--headless=new` — Chromium's current headless mode, no window.
- `--disable-gpu` — no GPU path, so rasterisation does not vary by machine.
- `--hide-scrollbars` — a scrollbar would steal pixels from the right edge.
- `--force-color-profile=srgb` — identical colour regardless of the display.
- `--force-device-scale-factor=1` — 1 CSS px = 1 image px; no HiDPI doubling.
- `--virtual-time-budget=4000` — waits for layout and the local
  `font-display:block` faces before shooting.
- `--window-size` — the viewport, set to the template's true pixel size
  (1200x1200 on v4), which is also the PNG's size.
- `--screenshot` — full-viewport capture. The skill writes it to a temp folder
  first and moves the PNG into `generated-images/` afterwards.

Any Chromium-based browser takes these exact flags. The lookup order is the
`CHROME` env var, then Chrome's install path, then the Edge preinstalled on
Windows, then `google-chrome` / `chromium` / `msedge` on `PATH`. A browser is
never downloaded.

## There is no compositing step

The shell **is** the finished 1200x1200 canvas. The canon template is a
full-bleed `<img id="base">` at `left:0; top:0`, and the two elements are
absolutely positioned on top of it at the fixed geometry — `#cardmount` at
`CARD_X/Y`, `#spkmount` at `SPK_X/Y`. The actor card is laid out at its native
CSS width (400) and scaled into the 799-wide block with
`transform:scale(1.9975); transform-origin:top left`, because at block width the
description rewraps and the card drops a height step. `.SpeakerCard` is the
opposite: `SPK_SCALE` is always `1.000000`, rendered 1:1 at 294x336. Under each
one sits an opaque plate, outset 1px on all sides, so the coloured block's square
corners cannot peek past the element's rounded ones. The screenshot is the
delivered PNG; no image library ever opens it.

## Local fonts, zero JavaScript

The shell links [`_internal/fonts/fonts.css`](../../_internal/fonts/fonts.css)
as `../fonts/fonts.css`: Inter 400/500/600 and IBM Plex Mono 500 as local
`woff2`, latin and latin-ext subsets, SIL OFL texts in
`_internal/fonts/licenses/`. Nothing is fetched over a network during a render,
so a face can never silently fall back to a system one. The shell contains **no
JavaScript at all** — one `<style>` block and a static DOM, as
[`scripts-and-security.md`](../about-this-project/scripts-and-security.md)
inventories.

## Verify by inspection

The assistant reads the PNG back and looks at it: exact dimensions, both blocks
covered to their four edges and corners with no placeholder purple or green
leaking, untouched template areas identical, text right, the fixed footer line,
the hover ring, the starfield. Anything off halts that folder, deletes the
screenshot and reports.

## The same trick draws the docs

The two help graphics are rendered exactly this way — from
[`filling-in-the-form/INDEX.md`](../filling-in-the-form/INDEX.md):
`field-mapping.html` is "the source of `Actor card field mapping.png` —
re-renderable with headless Chrome at window size 1400x440", and
`text-budgets.html` "the source of `Actor card text budgets.png` — same, at
window size 1400x640". Same flags, different `--window-size`.

Reproducible by construction: the same HTML, the same bundled fonts and the same
browser engine give the same pixels on any machine, with nothing fetched and
nothing installed.

Back to the [index](INDEX.md).
