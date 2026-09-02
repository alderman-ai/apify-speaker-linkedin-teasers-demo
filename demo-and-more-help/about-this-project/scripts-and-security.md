# Scripts and security — the complete inventory

You just cloned (or were handed) a folder from the internet. Healthy
question: *what in here can run code on my machine?*

**Short answer: nothing, on its own.** This repo contains **zero
executable scripts** — no PowerShell (`.ps1`), no shell scripts, no batch
files, no Python, no standalone JavaScript, no binaries, no installers, no
package manifests that pull dependencies. That's deliberate design, not an
accident: the AI assistant *is* the engine, following plain-markdown
procedures you can read yourself in `_internal/skills/`.

## Every piece of code in this repo, itemised

This is the complete list. If code is ever added to this repo, this file
must be updated in the same change — an assistant working here is
instructed to keep it truthful.

| where | what it is | when it runs | what it can do |
|---|---|---|---|
| `demo-and-more-help/filling-in-the-form/field-mapping.html` | one embedded `<script>` block, ~25 lines of JavaScript | **only** if a person deliberately opens this HTML file in a browser | draws the grey connector wires on the annotation graphic by measuring where the labels sit on the page. It stays entirely inside its own page: no network requests, no file access, no storage, no downloads |
| `demo-and-more-help/filling-in-the-form/text-budgets.html` | HTML + CSS only — **no JavaScript** | opened manually, same as above | displays a static table |
| `_internal/render/shell.html` | HTML + CSS only — **no JavaScript** | the assistant screenshots a filled copy of it with a headless browser during "process the queue" | displays the card so the browser can photograph it |
| the `.svg` files | vector images, **no scripts inside** | displayed as images | draw shapes |

Everything else in the repo is markdown, PNG/JPG images, and font files.

## When does anything execute at all?

Only when **you ask the assistant to generate a card**. It then launches
the Chrome or Edge **already installed on your machine** in headless mode
to screenshot the render page — using local files only. The repo's docs
explicitly forbid downloading a browser, fetching from the network during
renders, or adding scripts. There are no hooks, no auto-run configuration,
nothing triggered by cloning, opening, or browsing this folder.

## Related safety rails

- The two generation sources (the canon template image and the blank
  intake form) live in `_internal/core-templates-please-dont-touch/`. A
  run only reads them, never writes them; changing one is a deliberate,
  versioned edit. Details in that folder's README.
- Nothing in the pipeline ever overwrites your files: over-budget text,
  off-spec images, or geometry mismatches halt with a printed reason.
- The bundled fonts (Inter, IBM Plex Mono) are under the SIL Open Font
  License, redistributable, with licence texts in
  `_internal/fonts/licenses/`.
