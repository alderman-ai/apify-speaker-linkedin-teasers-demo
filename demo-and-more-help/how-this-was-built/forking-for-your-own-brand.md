# Forking it for your own brand

The demo is Apify-styled, but nothing in the pipeline knows what Apify
looks like. Rebranding is a swap of the coded parts and a re-measure of
the numbers that depend on them; the queue, the gates and the render step
stay as they are.

## What you replace

| piece | where | what changes |
|---|---|---|
| the card markup and CSS | [`_internal/render/shell.html`](../../_internal/render/shell.html) — the `.ActorCard` block and its tokens | your component's markup and styles: colours (the `--bg`, `--bg-subtle`, `--border`, `--text` variables), type sizes and weights, spacing. Keep the double-brace tokens, or rename them and the skill's token table together |
| the quantised height table | [`intake-template.md`](../../_internal/core-templates-please-dont-touch/intake-template.md) §1a and the skill's ratio check | re-measure your card's heights per description line count on a real render, and write the new steps in; the ±2px ratio check reads them |
| the backdrop | [`_internal/core-templates-please-dont-touch/`](../../_internal/core-templates-please-dont-touch/README.md) | a new 1200×1200 PNG with your own placeholder blocks, saved as `_v<N>` and referenced everywhere the old name appears |
| the eight constants | `intake-template.md` §6 | re-measure both blocks on the new image as exact-colour footprints in whole pixels and write `card_x/y/w/h`, `speaker_x/y/w/h` in |
| the fonts | [`_internal/fonts/`](../../_internal/fonts/fonts.css) | your faces as local `woff2` with their licence texts; update the `@font-face` set and the `font-family` names in the shell. Ship only fonts you may redistribute |
| the text budgets | `intake-template.md` §2 and the fence label `presentation-description-<NN>-char-max` | re-measure on your card (binary-search the longest string that renders without truncation, the way the current budgets were found) and update the form, the skills and the two help graphics |
| the speaker element | the `.SpeakerCard` block in the shell | your photo frame and your fixed copy in place of `Join me in PRAGUE` |

## What stays

- The queue: `to-process/` → `processed/` + `generated-images/`, never
  overwriting, `-NN` on repeats.
- The gates: fences win over frontmatter, budgets rejected not trimmed, the
  roughly-square photo rule with its even trim, halt-don't-degrade with a
  printed reason.
- The render step: fill a copy of the shell, one headless Chromium
  screenshot at the template's size, local fonts, no network, no scripts.
- The two skills' procedures and the `CLAUDE.md` / `AGENTS.md` routing —
  they name files, not brands.

The bump is a versioned event, as the template-change routing in
`CLAUDE.md` describes: new image suffix, new form `version` /
`versioned_at`, the completed example and the demo speaker refreshed, one
card rendered to confirm both blocks are covered to the last pixel.

## Licensing

There is no `LICENSE` file in this repo yet; the author decides its
licensing. Until one exists, treat the code and documents as
all-rights-reserved and ask before reusing them. The bundled fonts carry
their own SIL Open Font License texts in `_internal/fonts/licenses/`, and
the card design itself is an unofficial homage — Apify's name, logo and
visual design belong to Apify, and a fork should replace them with your
own.

Back to the [index](INDEX.md).
