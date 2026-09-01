# internal/render/

- `shell.html` — the render page. The generator skill fills a **copy** of it
  (saved in this folder as `_run-<name>.html` so `../fonts/fonts.css`
  resolves), then screenshots it with a headless Chromium browser (Chrome
  or Edge) at the template's size. Every double-brace token is documented
  in `skills/apify-speaker-card.md`. It holds both elements: the card CSS
  is a pixel-verified reproduction of apify.com's actor card (its oddities
  are deliberate, do not tidy them), and `.SpeakerCard` is the grey-framed
  speaker element with its fixed `Join me in PRAGUE` copy.
- `footer-help-icon.svg` — the static 20x20 footer circle icon (orange `?`;
  its 1px orange ring is the avatar border in the shell CSS), identical on
  every generated card, never an input.
- `_run-*.html` — transient, gitignored; delete after capture.
