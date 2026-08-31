# internal/render/

- `shell.html` — the render page. The generator skill fills a **copy** of it
  (saved in this folder as `_run-<name>.html` so `../fonts/fonts.css`
  resolves), then screenshots it with headless Chrome at the template's
  size. Every double-brace token is documented in the generator SKILL.md;
  the card CSS is a pixel-verified reproduction of apify.com's actor card —
  its oddities are deliberate, do not tidy them.
- `footer-cross-icon.svg` — the static 20x20 footer circle icon, identical
  on every generated card, never an input.
- `_run-*.html` — transient, gitignored; delete after capture.
