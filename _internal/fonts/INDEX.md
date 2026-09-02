# _internal/fonts/

Self-hosted webfonts the render shell links relatively
(`../fonts/fonts.css`) so a capture never depends on a network fetch:
**Inter 400/500/600** (all card text) and **IBM Plex Mono 500** (the
position/company line) — exactly what apify.com serves for the card. Both
SIL OFL 1.1; licence texts in `licenses/`.

`jic/` is not a font folder: it holds byte-identical reference copies of
the two core templates (`BU_intake-template.md`,
`BU_speaker-teaser-linkedin_v3.png`), used for the drift check when there
is no `.git` to diff against — a ZIP download. See its own `INDEX.md`.

GT Walsheim (apify.com's heading face) is deliberately absent and must
never be added — commercial Grilli Type licence, and the card render does
not use it.
