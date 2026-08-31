# visual-templates/ — the Canva-exported base image

The base PNG a card is composited into. This demo ships exactly **one**,
`speaker-teaser-linkedin.png` (1200×1200), and its dimensions are fixed —
the pipeline is not meant to support multiple visual templates.

**The template image is canon**: the purple block is where the card lands,
the green block is where the speaker photo lands, and their geometry is read
off the image every run — tweak the blocks in Canva, re-export over this
file, done.

For sub-pixel accuracy, paste Canva's Width/Height/X/Y readout panel inside
each block; the skill reads it and sanity-checks it against the visible
block. One constraint is enforced at run time: the purple block's
proportions must match the card as it actually renders (see
`intake-template.md` section 1b).

`particles.svg` is apify.com's particle pattern (`/img/pattern/particles.svg`,
80×80 tile) — the source of the starfield stamped into the canon template
(`#D2D3D6` with its authored `opacity .4` dropped, then a vertical opacity
gradient: 100% at the bottom frame line fading to 25% at the top of the
central box, y=269 just below the header band; background pixels only,
inside the orange frame margins). Kept here so a template re-export can be
re-stamped without refetching.

The current canon template's placeholder blocks were machine-redrawn
(2026-09-01) to the operator's layout: green speaker block 294×336 @
(705.5, 345) — spanning the two central orange crosses vertically, centred
on the header's right title box — and purple card block 799×307 @
(200.5, 748) — right edge shared with the speaker block, centred on the
canvas x-axis, vertically centred between the speaker block and the bottom
frame line. There are currently no printed readout panels; geometry reads
are mask-only. `template base edit*.png` are earlier operator drafts, kept
for history.
