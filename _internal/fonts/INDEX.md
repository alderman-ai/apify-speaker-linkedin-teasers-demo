# _internal/fonts/

Self-hosted webfonts the render shell links relatively
(`../fonts/fonts.css`) so a capture never depends on a network fetch:
**Inter 400/500/600** (all card text) and **IBM Plex Mono 500** (the
position/company line) — exactly what apify.com serves for the card. Both
SIL OFL 1.1; licence texts in `licenses/`.

GT Walsheim (apify.com's heading face) is deliberately absent and must
never be added — commercial Grilli Type licence, and the card render does
not use it.
