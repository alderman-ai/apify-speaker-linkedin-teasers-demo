# core-templates-please-dont-touch/ — stop, read this first

The two **source-of-truth files every card is generated from**. Do not
edit, re-save, re-export, rename or "tidy" either of them in passing —
every future card inherits any change, silently. A run only ever reads
them.

| file | what |
|---|---|
| `speaker-teaser-linkedin_v3.png` | the canon visual template (1200×1200), currently version 3. Its coloured blocks ARE the layout: purple = actor card, green = speaker element; geometry is measured off this image every run |
| `intake-template.md` | the blank intake form — the whole input contract, versioned in its own frontmatter (`version` / `versioned_at`). Copied into each new speaker folder as `intake.md`; the copy is what operators fill, never this file |

## Changing a template

A template change is a deliberate, versioned event:

- the intake form bumps `version` / `versioned_at` in its frontmatter, and
  the two filled copies are refreshed from it — the completed example in
  `demo-and-more-help/filling-in-the-form/` and the bundled demo speaker's
  `intake.md` in `_internal/demo-speaker/`;
- the visual template bumps its `_v<N>` filename suffix and every
  reference to it (`CLAUDE.md` / `AGENTS.md`, both skills, the READMEs,
  the completed example in `demo-and-more-help/filling-in-the-form/`, the
  demo speaker's `intake.md`);
- render one card afterwards to confirm the result.

Want a different look for one speaker without changing canon? Copy the
template, edit the copy, and point that speaker's `base_image` at it.

## About the canon template

The pipeline supports exactly **one** visual template with fixed
dimensions. The purple block is where the card lands, the green block is
where the speaker element lands, and their geometry is read off the image
every run. For sub-pixel accuracy a future template may paste Canva's
Width/Height/X/Y readout panel inside each block; the skill reads it and
sanity-checks it against the visible block. One constraint is enforced at
run time: the purple block's proportions must match the card as it
actually renders (see `intake-template.md` section 1b).

The current canon template is **machine-built** (2026-09-01), superseding
the operator's original Canva export: baked gradient starfield + blocks
redrawn at the operator's locked layout — green speaker block 294×336 @
(705.5, 345) (spanning the two central orange crosses vertically, centred
on the header's right title box) and purple card block 799×307 @
(200.5, 748) (right edge shared with the speaker block, centred on the
canvas x-axis, vertically centred between the speaker block and the bottom
frame line). There are no printed readout panels; geometry reads are
mask-only. Earlier operator drafts (`template base edit*.png`) live only
in git history.

### Starfield rebuild recipe

`_internal/render/particles.svg` is apify.com's particle pattern
(`/img/pattern/particles.svg`, 80×80 tile) — the source of the starfield
stamped into the canon template (`#D2D3D6` with its authored `opacity .4`
dropped, then a vertical opacity gradient: 100% at the bottom frame line
fading to 25% at the top of the central box, y=269 just below the header
band; background pixels only, inside the orange frame margins). Kept local
so a template re-export can be re-stamped without refetching. Re-exporting
the template means re-staging this recipe, not just dropping in a new PNG —
and it is a version bump, per the rules above.
