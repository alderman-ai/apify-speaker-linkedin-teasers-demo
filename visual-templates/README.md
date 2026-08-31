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
