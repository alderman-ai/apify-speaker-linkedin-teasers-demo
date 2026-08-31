# templates/ — Canva-exported base images

The base PNG a card is composited into. **The template image is canon**: the
purple block is where the card lands, the green block is where the speaker
photo lands, and their geometry is read off the image every run — resize or
move blocks in Canva, re-export here, done.

For sub-pixel accuracy, paste Canva's Width/Height/X/Y readout panel inside
each block; the skill reads it and sanity-checks it against the visible
block. One constraint is enforced at run time: the purple block's
proportions must match the card as it actually renders (see
`INTAKE-TEMPLATE.md` section 1b).
